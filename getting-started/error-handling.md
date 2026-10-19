# Error Handling

The Colect SOAP API uses standard SOAP fault responses for error handling. This guide explains common errors and how to handle them.

***

## SOAP Fault Structure

When an error occurs, the API returns a SOAP Fault:

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <soap:Fault>
            <faultcode>soap:Server</faultcode>
            <faultstring>Invalid API key</faultstring>
            <detail>
                <message>The provided API key is not valid for any collection</message>
            </detail>
        </soap:Fault>
    </soap:Body>
</soap:Envelope>
```

***

## Common Error Scenarios

### Authentication Errors

| Error           | Cause                          | Solution                               |
| --------------- | ------------------------------ | -------------------------------------- |
| Invalid API key | API key not recognized         | Verify your API key within the backend |
| API key missing | No `apiKey` element in request | Add the required `apiKey` parameter    |

### Validation Errors

| Error                   | Cause                                     | Solution                                             |
| ----------------------- | ----------------------------------------- | ---------------------------------------------------- |
| Missing mandatory field | Required field not provided               | Check data type documentation for mandatory fields   |
| Invalid field format    | Field value doesn't match expected format | Verify date formats (ISO 8601), currency codes, etc. |
| Duplicate identifier    | `uniqueId` + `colorCode` already exists   | Use unique combinations or use update operations     |

### Data Integrity Errors

| Error                      | Cause                             | Solution                                           |
| -------------------------- | --------------------------------- | -------------------------------------------------- |
| Unknown product reference  | Referenced product doesn't exist  | Ensure products are synced before referencing them |
| Unknown customer reference | Referenced customer doesn't exist | Sync customers before creating access rules        |

***

## Handling Unknown Products

Some operations include a `failOnUnknownProducts` parameter:

{% tabs %}
{% tab title="Fail on Unknown (Safe)" %}
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

The operation will fail if any product doesn't exist, protecting against typos.
{% endtab %}

{% tab title="Ignore Unknown (Tolerant)" %}
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

Unknown products are silently ignored. Useful when syncing from systems with partial product sets.
{% endtab %}
{% endtabs %}

**Operations supporting this parameter:**

* `updateStock`
* `updatePrices`
* `updateExtraFields`
* `storeFullProductRelations`
* `storeProductRelations`

***

## Success Responses

Successful operations return an empty response body:

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <ns2:storeProductsResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
    </soap:Body>
</soap:Envelope>
```

{% hint style="success" %}
An empty response body (no SOAP Fault) indicates the operation completed successfully.
{% endhint %}

***

## Best Practices

### 1. Implement Retry Logic

```
Retry strategy for transient errors, excluding rate-limit responses:
├── Attempt 1: Immediate
├── Attempt 2: Wait 5 seconds
├── Attempt 3: Wait 30 seconds
└── Attempt 4: Wait 2 minutes
```

Do not use the immediate attempt after a rate-limit response. Follow the rate-limit policy below instead.

### 2. Log All Requests and Responses

Keep detailed logs for troubleshooting:

* Full request XML
* Full response XML
* Timestamp
* Operation name

### 3. Validate Before Sending

Check mandatory fields before making API calls to reduce error responses.

### 4. Use Idempotent Operations

Most Colect operations are idempotent - sending the same product data twice results in the same state. This makes retries safe.

***

## Rate-limit retry policy

{% hint style="warning" %}
The limit is one API call every five seconds for each API-key/function pair.
{% endhint %}

Treat a rate-limit response differently from other transient failures. Do not retry it immediately.

* Serialize calls that use the same API-key/function pair. Do not run them concurrently.
* Before the next call for that pair, wait for the prior call to complete. Then ensure five seconds have elapsed after its completion.
* After a rate-limit response, wait at least five seconds before retrying. Send that retry through the same serialized path.
* A retry after only fractions of a second, such as 0.1–0.4 seconds, continues to be rejected.

Reduce request frequency where possible. For high-volume synchronization, batch data into larger payloads instead of sending many small requests.

***

## Getting Help

If you encounter persistent errors:

1. Check the error message for specific details
2. Verify your request against the WSDL schema
3. Review the data type documentation for field requirements
