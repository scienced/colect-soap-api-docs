---
description: >-
  How the Colect SOAP API reports errors, how rate limits and
  simultaneous-call   protection work, and how to build an integration that
  handles them reliably.
---

# Error Handling

The Colect SOAP API returns a standard SOAP Fault when a call fails. This page explains the common errors, how rate limits and simultaneous-call protection work, and how to design your integration so it recovers from errors without help.

Your integration receives every error described here in the response to its own call. Log those responses on your side: Colect does not provide call logs, log exports, or a monitoring API for integrations.

### SOAP Fault structure

When an error occurs, the API returns HTTP status 500 with a SOAP Fault. For example, a rate-limit rejection:

{% code collapsedlinecount="10" %}
```xml
<S:Envelope xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
    <S:Body>
        <S:Fault>
            <faultcode>S:Server</faultcode>
            <faultstring>2026-10-19 15:20:04.365 - Rate limit exceeded (apiKey=XXXXX-XXXXX-XXXXX-XXXXX-XXXX); it's only allowed to make one call per 5 seconds for each apiKey+function</faultstring>
        </S:Fault>
    </S:Body>
</S:Envelope>
```
{% endcode %}

* The `faultstring` holds the full error. There is no `detail` element.
* The `faultstring` starts with a timestamp in the format `yyyy-MM-dd HH:mm:ss.SSS`, followed by the error text.
* The timestamp is Amsterdam local time (CET in winter, CEST in summer) and carries no offset. Convert it before you compare it with logs in another timezone.
* Match on the fault code's local name, `Server`. The namespace prefix can differ.

{% hint style="warning" %}
Most error messages include your API key in plain text. Mask the key before you store or share logs.
{% endhint %}

***

### Common error scenarios

#### Authentication errors

| Error           | Cause                              | Solution                                                                           |
| --------------- | ---------------------------------- | ---------------------------------------------------------------------------------- |
| Invalid API key | API key not recognized             | Verify the API key in the collection's **General** settings in the Backend Console |
| API key missing | No `apiKey` element in the request | Add the required `apiKey` parameter                                                |

#### Validation errors

| Error                   | Cause                                         | Solution                                                                                                                           |
| ----------------------- | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| Missing mandatory field | Required field not provided                   | Check the data type documentation for mandatory fields                                                                             |
| Invalid field format    | Field value doesn't match the expected format | Verify currency codes and similar values. Date fields never return this error: see [Date formats](error-handling.md#date-formats). |
| Duplicate identifier    | `uniqueId` + `colorCode` already exists       | Use unique combinations or use update operations                                                                                   |

#### Data integrity errors

| Error                              | Cause                                                               | Solution                                                |
| ---------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------- |
| Unknown product reference          | Referenced product doesn't exist                                    | Make sure products are synced before you reference them |
| Unknown customer reference         | Referenced customer doesn't exist                                   | Sync customers before you create access rules           |
| `Order not found`                  | `markOrdersAsProcessed` received an order number that doesn't exist | Only mark orders returned by `getUnprocessedOrders`     |
| `javax.xml.ws.WebServiceException` | `markOrdersAsProcessed` was sent with an empty order list           | Skip the call when there are no orders to mark          |

#### Throttling errors

| Error                                                                                                         | Cause                                                                                               | Solution                                                                                                         |
| ------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `Rate limit exceeded (apiKey=...); it's only allowed to make one call per 5 seconds for each apiKey+function` | A call for the same API key and operation arrived less than five seconds after the previous attempt | Wait at least six seconds, then retry. [See Rate limits](error-handling.md#rate-limits).                         |
| `Another '<group>' call is already in progress; simultaneous calls are not allowed`                           | A call in the same operation group is still running for this API key                                | Wait for the running call to return, then retry. [See Simultaneous calls](error-handling.md#simultaneous-calls). |

Both throttling errors are safe to retry. Neither error means your data was rejected: the call was not processed at all.

#### Service availability errors

| Error                                                                                | Cause                                                                                                                                                                       | Solution                                                                                                                                               |
| ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Cannot invoke "nl.apptitude.salesapp.ws.Brand.getApiKey()" because "brand" is null` | Colect could not reach one of its internal services while it handled your call, most often during a deployment. The message is misleading: your API key is not the problem. | Wait at least 30 seconds, then resend the same call. [See Deployments and brief interruptions](error-handling.md#deployments-and-brief-interruptions). |
| `Failed to access the WSDL` or `Connection refused`                                  | The same cause: a service was being replaced when your call arrived.                                                                                                        | Wait at least 30 seconds, then resend the same call.                                                                                                   |

These two messages are the same failure at different moments. If Colect cannot reach the internal service before it has resolved your API key, it cannot name the collection in the error and reports the `Brand.getApiKey()` message instead of the real cause. If the failure happens later in the call, the underlying message is passed through. Handle both the same way.

Either of these two faults means Colect did not process the call at all, so you can resend it unchanged. This does not apply to validation or data integrity faults: see [Retry with the right strategy](error-handling.md#retry-with-the-right-strategy).

If you receive this error for several minutes in a row outside the deployment window, or on every call, contact support. See [Getting help](error-handling.md#getting-help).

***

### Date formats

Send every date as `yyyy-MM-dd`, for example `2026-04-01`. Colect stores the value without separators, so this date appears as `20260401` in the Backend Console.

Colect does not validate dates. A date in any other format does not return a SOAP Fault. The call succeeds with HTTP status 200 and the usual empty response, and Colect either clears the stored date or stores an incorrect one.

| What you send for a date field | Result                                        |
| ------------------------------ | --------------------------------------------- |
| `2026-04-01`                   | Stored correctly as `20260401`                |
| `15-03-2026`                   | Not recognized. The stored date is cleared.   |
| `20260315`                     | Not recognized. An incorrect value is stored. |
| No element                     | The stored date is cleared.                   |

{% hint style="warning" %}
Because no error is returned, an invalid or missing date replaces the date already stored for the product. Your integration receives a success response either way.
{% endhint %}

To avoid this:

* Validate and format dates in your integration before you send them.
* Include every date element you want to keep in each `storeProducts` call.
* After the first run of a new integration, open a sample product in the Backend Console and compare its dates with what you sent.

***

### Rate limits

Colect allows one call per five seconds for each combination of API key and operation. The error message states the limit that applies to your collection.

#### How the limit is measured

* **Per API key and operation.** Each operation has its own limit. `getUnprocessedOrders` and `markOrdersAsProcessed` are counted separately, so calling one doesn’t use up the other’s limit. They do share a simultaneous-call lock: see [Simultaneous calls](error-handling.md#simultaneous-calls).
* **From the start of one call to the start of the next.** Colect records the time a call arrives, before it processes the call. How long the previous call took doesn't matter.
* **Rejected calls count too.** A call that is rejected for exceeding the rate limit still resets the five-second window. The wait always counts from the last attempt, successful or not. If your integration retries too early, every retry is rejected and pushes the window out again.

{% hint style="warning" %}
A short automatic retry, for example after a fraction of a second, never succeeds. Each early retry restarts the five-second window, so the integration can stay blocked for as long as it keeps retrying.
{% endhint %}

#### What this means for your integration

* Serialize calls that use the same API key and operation. Don't send them from parallel processes or threads.
* If several collections or integration jobs share an API key, route all their calls through one queue, so the spacing applies to all of them together.
* Leave more than five seconds between the starts of two calls for the same operation. Six seconds gives a safe margin for network delay.
* After a rate-limit error, wait at least six seconds before you retry. Send the retry through the same queue.
* For high-volume synchronization, batch data into larger payloads instead of sending many small requests.

***

### Simultaneous calls

Colect processes only one call at a time per API key within each operation group. If a call arrives while another call in the same group is still running, Colect rejects it immediately. It doesn't wait for the first call to finish.

Operations that work on the same data share a group:

| Group              | Operations                                                                                                                        |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| Products           | `storeFullProductSet`, `storeProducts`, `deleteProducts`, `deleteAllProducts`, `updateStock`, `updatePrices`, `updateExtraFields` |
| Unprocessed orders | `getUnprocessedOrders`, `markOrdersAsProcessed`                                                                                   |
| Customers          | Customer store and delete operations                                                                                              |
| Customer access    | Customer access store and delete operations                                                                                       |
| Product access     | Product access store and delete operations                                                                                        |
| Product relations  | Product relation store and delete operations                                                                                      |
| Historical orders  | Historical order store and delete operations                                                                                      |
| Invoices           | Invoice store and delete operations                                                                                               |

Operations in different groups can run at the same time. For example, a product update can run while your ERP fetches orders.

#### Why a time-based delay isn't enough

A fixed delay between calls doesn't prevent this error. If a `getUnprocessedOrders` call takes longer than your delay, the next call arrives while the first is still running and is rejected. The same applies to `markOrdersAsProcessed`: if you send it before `getUnprocessedOrders` has returned, it is rejected, because both operations share a group.

Wait for each call to return a response before you send the next call in the same group. Then apply the rate-limit spacing.

{% hint style="info" %}
Colect's own scheduled connector runs use the same products and customers groups. A product or customer call can be rejected while a scheduled run for the collection is in progress. Retry after a short wait.
{% endhint %}

***

### Deployments and brief interruptions

Colect updates its services regularly. The SOAP API stays available during an update, but a call can fail while the service that handles it is replaced. A resend normally succeeds. If a running call was cut off, the resend can be rejected for a while, as described below.

#### When deployments happen

* **Every morning** between 06:00 and 07:00 UTC. That is 07:00 to 08:00 in Amsterdam during winter time (CET) and 08:00 to 09:00 during summer time (CEST).
* **Occasionally during the day**, for example when Colect releases a change. These deployments carry the same risk as the morning deployment.

Most morning deployments take about two minutes. Colect does not announce the daily deployment in advance.

#### What happens to your call

| Situation                                               | What you see                                                                                                                                           | What to do                                            |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------- |
| Your call arrives while a service is being replaced     | An immediate SOAP Fault, see [Service availability errors](error-handling.md#service-availability-errors), or a plain HTTP error or dropped connection | Resend the call. Colect did not process it.           |
| Your call is already running when the deployment starts | The call is given a short period to finish. A short call normally completes; a large message can be cut off.                                           | If the call ends without a response, wait and resend. |

Not every deployment failure arrives as a SOAP Fault. Your client can also see a plain HTTP 502 or 503, or a dropped connection, with no SOAP envelope. Most SOAP clients report this as a transport or parse error. Treat these the same way and resend the call.

If the call ends without a response, wait and resend. The resend can be rejected with `Another '<group>' call is already in progress` while Colect is still holding the lock from the interrupted call. Keep retrying with the backoff in [Retry with the right strategy](error-handling.md#retry-with-the-right-strategy) until the call is accepted.

Large messages carry the most risk, because they run the longest. This applies to full product sets, large product access sets and large invoice batches.

{% hint style="warning" %}
When a call ends without any response, you cannot tell whether Colect processed it. Most Colect operations are idempotent, so sending the same data twice results in the same state. Resend the call, and see [Use idempotent operations](error-handling.md#use-idempotent-operations).
{% endhint %}

#### How to design for it

* Resend failed calls automatically, using the backoff in [Retry with the right strategy](error-handling.md#retry-with-the-right-strategy).
* Treat a failed call as not completed until you receive a response without a SOAP Fault.
* Log every failed call with the time it was sent, so you can see whether a failure falls in the deployment window.
* Where you can choose, start large synchronization jobs outside 06:00 to 07:00 UTC. This lowers the risk but does not remove it, because deployments also happen during the day.

Colect is changing how it deploys to reduce these interruptions.

***

### Handling unknown products

Some operations include a `failOnUnknownProducts` parameter:

{% tabs %}
{% tab title="Fail on unknown (safe)" %}
{% code collapsedlinecount="10" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:updateStock>
            <api:apiKey>your-api-key</api:apiKey>
            <api:failOnUnknownProducts>true</api:failOnUnknownProducts>
            <api:stockInfoElement>
                <api:productUniqueId>UNKNOWN-SKU</api:productUniqueId>
                <api:productColorCode>BLK</api:productColorCode>
                <!-- ... -->
            </api:stockInfoElement>
        </api:updateStock>
    </soapenv:Body>
</soapenv:Envelope>
```
{% endcode %}

The operation fails if any product doesn't exist, which protects against typos.
{% endtab %}

{% tab title="Ignore unknown (tolerant)" %}
{% code collapsedlinecount="10" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:updateStock>
            <api:apiKey>your-api-key</api:apiKey>
            <api:failOnUnknownProducts>false</api:failOnUnknownProducts>
            <api:stockInfoElement>
                <api:productUniqueId>UNKNOWN-SKU</api:productUniqueId>
                <api:productColorCode>BLK</api:productColorCode>
                <!-- ... -->
            </api:stockInfoElement>
        </api:updateStock>
    </soapenv:Body>
</soapenv:Envelope>
```
{% endcode %}

Unknown products are skipped without an error. Use this when you sync from a system that holds more products than the collection.
{% endtab %}
{% endtabs %}

Operations that support this parameter:

* `updateStock`
* `updatePrices`
* `updateExtraFields`
* `storeFullProductRelations`
* `storeProductRelations`

***

### Success responses

Successful operations return an empty response body:

{% code collapsedlinecount="10" %}
```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <ns2:storeProductsResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
    </soap:Body>
</soap:Envelope>
```
{% endcode %}

An empty response body with no SOAP Fault means the operation completed successfully.

***

### Best practices

#### Log every request and response

Your integration's logs are the record of what it sent to Colect and what came back. Log the following for every call:

* Operation name
* Time the request was sent
* Time the response was received
* Full request XML
* Full response XML, including any SOAP Fault

With these logs, you can match each error to the call that caused it and see whether calls overlapped or arrived too close together.

#### Retry with the right strategy

Retry throttling errors and transient errors, such as a dropped connection, a timeout, or a failure during a [deployment](error-handling.md#deployments-and-brief-interruptions). Use a backoff, and never retry immediately after a throttling error.

{% code collapsedlinecount="10" %}
```
Rate-limit or simultaneous-call error:
├── Wait at least 6 seconds (and for any running call in the same group to return)
└── Retry through the same queue

Other transient errors:
├── Attempt 2: wait 5 seconds
├── Attempt 3: wait 30 seconds
└── Attempt 4: wait 2 minutes
```
{% endcode %}

Don't retry validation or data integrity errors unchanged. Fix the data first.

#### Validate before sending

Check mandatory fields before you make API calls to reduce error responses.

#### Use idempotent operations

Most Colect operations are idempotent: sending the same product data twice results in the same state. This makes retries safe.

***

### Monitoring emails

Colect sends a monitoring email for every error a call returns for a collection. This includes rate-limit and simultaneous-call rejections and errors such as `Order not found`. Emails go to the addresses in the company's monitoring settings. An integration that keeps retrying too early causes one email per retry.

For who receives these emails and how to change the recipients, see [Connector monitoring emails](https://docs.colect.io/admin/getting-started/connector-monitoring-emails).

***

### Getting help

If you encounter persistent errors:

* Check the error message for specific details
* Verify your request against the WSDL schema
* Review the data type documentation for field requirements
* Check your integration's logs for overlapping or closely spaced calls

If the problem remains, create a ticket in the [Colect support portal](https://support.apptitude.nl/) with the operation name, the full error message, and the request and response times from your logs.

***

### See Also

* [Making Your First Call](first-call.md) — Authentication and a first request
* [Connector monitoring emails](https://docs.colect.io/admin/getting-started/connector-monitoring-emails) — Who receives API error emails
* [Connector Status](https://docs.colect.io/admin/getting-started/connector-status) — Last run state per collection
* [ERP Integration using SOAP API](https://docs.colect.io/admin/erp-integration-using-soap-api) — Order fetch and processing flow
