# Authentication

## API Key

All Colect API operations require authentication via an API key. This key identifies your collection and must be included in every request.

{% hint style="warning" %}
Your API key is collection-specific and should be kept confidential. Never expose it in client-side code or public repositories.
{% endhint %}

***

## Obtaining Your API Key

If you cannot find your API key, please check the **Backend** section under the **Collections** tab, where your collections (brand/tenant) are configured. For more information, [**click here**](https://docs.colect.io/admin/backend-console/collections).

***

## Using the API Key

The `apiKey` parameter is the first parameter in every API operation:

{% tabs %}
{% tab title="SOAP Request" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:getInfo>
            <api:apiKey>your-api-key-here</api:apiKey>
        </api:getInfo>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwfgHgtgNgBAbgUwE4GcCWB7AdgXgEQCMAdAAx4wJYDGGAJmlgOb4CqAKgGIC0AHHiAD4AUMBQYAhgAdKcAFwBRLIigZpQmBpiQoWFLLFSZ+ABYAXU5NkB6KyirGEEcSiLaDkohiSNbEyVZkEFWkrPHVNbV1ZKTQTc0sbGKIqKiIUcSgEdMkPKUlTNFMAV1oEIiwoUOFNGFE-GVkACQRxUqQras06wyVZACE6AE9Omo1gGNlGBFMASSwAMwwR0ZrxyTRo9YBpBGHBjCKkLhiuAGtdrgckBGArCZid4fCV24mp2YWl59rfHrkB2hPW7uBqKZSqBACIA)
{% endtab %}

{% tab title="Response" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <ns2:getInfoResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/">
            <return>Collection: Your Brand Name</return>
        </ns2:getInfoResponse>
    </soap:Body>
</soap:Envelope>
```
{% endtab %}
{% endtabs %}

***

## Connection Details

<table><thead><tr><th width="254.05078125">Property</th><th>Value</th></tr></thead><tbody><tr><td><strong>Endpoint URL</strong></td><td><code>https://connector.colect.services:443/services/api/3.0</code></td></tr><tr><td><strong>WSDL URL</strong></td><td><code>https://connector.colect.services/services/api/3.0?wsdl</code></td></tr><tr><td><strong>Protocol</strong></td><td>SOAP 1.1 / SOAP 1.2</td></tr><tr><td><strong>Transport</strong></td><td>HTTPS (TLS required)</td></tr><tr><td><strong>Port</strong></td><td>443</td></tr></tbody></table>

***

## Namespace Reference

When constructing SOAP requests, use these XML namespaces:

```xml
xmlns:api="http://api.cc.salesapp.apptitude.nl/"
xmlns:ws="http://ws.cc.salesapp.apptitude.nl/"
```

### Understanding Namespace Prefixes

XML namespace prefixes (like `api:`, `ns2:`, `soap:`) are **arbitrary labels** - only the namespace URI matters. The prefix is just a shorthand that maps to the actual namespace URI defined in the `xmlns` attribute.

{% tabs %}
{% tab title="Using api prefix" %}
```xml
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Body>
        <api:getInfo>
            <api:apiKey>your-key</api:apiKey>
        </api:getInfo>
    </soapenv:Body>
</soapenv:Envelope>
```
{% endtab %}

{% tab title="Using ns2 prefix" %}
```xml
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:ns2="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Body>
        <ns2:getInfo>
            <ns2:apiKey>your-key</ns2:apiKey>
        </ns2:getInfo>
    </soapenv:Body>
</soapenv:Envelope>
```
{% endtab %}

{% tab title="Using col prefix" %}
```xml
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:col="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Body>
        <col:getInfo>
            <col:apiKey>your-key</col:apiKey>
        </col:getInfo>
    </soapenv:Body>
</soapenv:Envelope>
```
{% endtab %}
{% endtabs %}

All three examples above are **functionally identical**. The server only cares about the namespace URI (`http://api.cc.salesapp.apptitude.nl/`), not the prefix you choose.

{% hint style="info" %}
**Convention:** This documentation uses `api:` for requests and `ns2:` for responses, but your SOAP client may generate different prefixes. As long as the namespace URI is correct, the request will work.
{% endhint %}

### Namespaces Used in This API

<table><thead><tr><th width="182.33203125">Prefix (convention)</th><th>Namespace URI</th><th>Used For</th></tr></thead><tbody><tr><td><code>soapenv:</code> or <code>soap:</code></td><td><code>http://schemas.xmlsoap.org/soap/envelope/</code></td><td>SOAP envelope structure</td></tr><tr><td><code>api:</code> or <code>ns2:</code></td><td><code>http://api.cc.salesapp.apptitude.nl/</code></td><td>All API operations and data types</td></tr><tr><td><code>ws:</code></td><td><code>http://ws.cc.salesapp.apptitude.nl/</code></td><td>Product relations only</td></tr></tbody></table>

***

## Verifying Your Connection

Use the `getInfo` operation to verify your API key and connection:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:getInfo>
            <api:apiKey>your-api-key-here</api:apiKey>
        </api:getInfo>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwfgHgtgNgBAbgUwE4GcCWB7AdgXgEQCMAdAAx4wJYDGGAJmlgOb4CqAKgGIC0AHHiAD4AUMBQYAhgAdKcAFwBRLIigZpQmBpiQoWFLLFSZ+ABYAXU5NkB6KyirGEEcSiLaDkohiSNbEyVZkEFWkrPHVNbV1ZKTQTc0sbGKIqKiIUcSgEdMkPKUlTNFMAV1oEIiwoUOFNGFE-GVkACQRxUqQras06wyVZACE6AE9Omo1gGNlGBFMASSwAMwwR0ZrxyTRo9YBpBGHBjCKkLhiuAGtdrgckBGArCZid4fCV24mp2YWl59rfHrkB2hPW7uBqKZSqBACIA)

A successful response confirms your API key is valid and returns your collection name.

{% hint style="success" %}
**Tip:** Use `getInfo` as a health check in your integration to verify connectivity before performing data synchronization.
{% endhint %}
