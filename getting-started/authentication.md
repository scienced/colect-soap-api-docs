# Authentication

## API Key

All Colect API operations require authentication via an API key. This key identifies your collection and must be included in every request.

{% hint style="warning" %}
Your API key is collection-specific and should be kept confidential. Never expose it in client-side code or public repositories.
{% endhint %}

---

## Obtaining Your API Key

Contact your Colect account manager or support to receive your API key. Each collection (brand/tenant) has a unique key.

---

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHNvYXBlbnY6RW52ZWxvcGUKICAgIHhtbG5zOnNvYXBlbnY9Imh0dHA6Ly9zY2hlbWFzLnhtbHNvYXAub3JnL3NvYXAvZW52ZWxvcGUvIgogICAgeG1sbnM6YXBpPSJodHRwOi8vYXBpLmNjLnNhbGVzYXBwLmFwcHRpdHVkZS5ubC8iPgogICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgIDxzb2FwZW52OkJvZHk+CiAgICAgICAgPGFwaTpnZXRJbmZvPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXktaGVyZTwvYXBpOmFwaUtleT4KICAgICAgICA8L2FwaTpnZXRJbmZvPgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)
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

---

## Connection Details

| Property | Value |
|----------|-------|
| **Endpoint URL** | `https://connector.colect.services:443/services/api/3.0` |
| **WSDL URL** | `https://connector.colect.services:443/services/api/3.0?wsdl` |
| **Protocol** | SOAP 1.1 / SOAP 1.2 |
| **Transport** | HTTPS (TLS required) |
| **Port** | 443 |

---

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

| Prefix (convention) | Namespace URI | Used For |
|---------------------|---------------|----------|
| `soapenv:` or `soap:` | `http://schemas.xmlsoap.org/soap/envelope/` | SOAP envelope structure |
| `api:` or `ns2:` | `http://api.cc.salesapp.apptitude.nl/` | All API operations and data types |
| `ws:` | `http://ws.cc.salesapp.apptitude.nl/` | Product relations only |

---

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHNvYXBlbnY6RW52ZWxvcGUKICAgIHhtbG5zOnNvYXBlbnY9Imh0dHA6Ly9zY2hlbWFzLnhtbHNvYXAub3JnL3NvYXAvZW52ZWxvcGUvIgogICAgeG1sbnM6YXBpPSJodHRwOi8vYXBpLmNjLnNhbGVzYXBwLmFwcHRpdHVkZS5ubC8iPgogICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgIDxzb2FwZW52OkJvZHk+CiAgICAgICAgPGFwaTpnZXRJbmZvPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXktaGVyZTwvYXBpOmFwaUtleT4KICAgICAgICA8L2FwaTpnZXRJbmZvPgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)

A successful response confirms your API key is valid and returns your collection name.

{% hint style="success" %}
**Tip:** Use `getInfo` as a health check in your integration to verify connectivity before performing data synchronization.
{% endhint %}
