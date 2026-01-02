# Making Your First Call

This guide walks you through making your first API call to the Colect SOAP API.

---

## Prerequisites

- Your API key (contact Colect support)
- A SOAP client (SoapUI, Postman, or your programming language's SOAP library)

---

## Step 1: Download the WSDL

Most SOAP clients can auto-generate request templates from the WSDL:

```
https://connector.colect.services:443/services/api/3.0?wsdl
```

---

## Step 2: Test Connection with getInfo

The simplest operation to verify your setup:

{% tabs %}
{% tab title="Request" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:getInfo>
            <api:apiKey>your-api-key</api:apiKey>
        </api:getInfo>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHNvYXBlbnY6RW52ZWxvcGUKICAgIHhtbG5zOnNvYXBlbnY9Imh0dHA6Ly9zY2hlbWFzLnhtbHNvYXAub3JnL3NvYXAvZW52ZWxvcGUvIgogICAgeG1sbnM6YXBpPSJodHRwOi8vYXBpLmNjLnNhbGVzYXBwLmFwcHRpdHVkZS5ubC8iPgogICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgIDxzb2FwZW52OkJvZHk+CiAgICAgICAgPGFwaTpnZXRJbmZvPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgPC9hcGk6Z2V0SW5mbz4KICAgIDwvc29hcGVudjpCb2R5Pgo8L3NvYXBlbnY6RW52ZWxvcGU+)
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST \
  'https://connector.colect.services:443/services/api/3.0' \
  -H 'Content-Type: text/xml; charset=utf-8' \
  -H 'SOAPAction: ""' \
  -d '<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:getInfo>
            <api:apiKey>your-api-key</api:apiKey>
        </api:getInfo>
    </soapenv:Body>
</soapenv:Envelope>'
```
{% endtab %}
{% endtabs %}

---

## Step 3: Store Your First Product

{% tabs %}
{% tab title="Request" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeProducts>
            <api:apiKey>your-api-key</api:apiKey>
            <api:product>
                <api:uniqueId>STYLE-001</api:uniqueId>
                <api:name>Classic T-Shirt</api:name>
                <api:description>Premium cotton t-shirt</api:description>
                <api:brandName>Your Brand</api:brandName>
                <api:colorCode>BLK</api:colorCode>
                <api:colorDesc>Black</api:colorDesc>
                <api:price>
                    <api:currencyCode>EUR</api:currencyCode>
                    <api:retailPrice>49.95</api:retailPrice>
                    <api:wholesalePrice>25.00</api:wholesalePrice>
                    <api:net>false</api:net>
                </api:price>
                <api:size>
                    <api:name>S</api:name>
                    <api:sortCode>1</api:sortCode>
                    <api:stockLevel>
                        <api:quantity>100</api:quantity>
                    </api:stockLevel>
                </api:size>
                <api:size>
                    <api:name>M</api:name>
                    <api:sortCode>2</api:sortCode>
                    <api:stockLevel>
                        <api:quantity>150</api:quantity>
                    </api:stockLevel>
                </api:size>
                <api:size>
                    <api:name>L</api:name>
                    <api:sortCode>3</api:sortCode>
                    <api:stockLevel>
                        <api:quantity>120</api:quantity>
                    </api:stockLevel>
                </api:size>
                <api:medium>
                    <api:type>IMAGE_PRIMARY</api:type>
                    <api:url>https://cdn.example.com/products/style-001-blk.jpg</api:url>
                </api:medium>
            </api:product>
        </api:storeProducts>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHNvYXBlbnY6RW52ZWxvcGUKICAgIHhtbG5zOnNvYXBlbnY9Imh0dHA6Ly9zY2hlbWFzLnhtbHNvYXAub3JnL3NvYXAvZW52ZWxvcGUvIgogICAgeG1sbnM6YXBpPSJodHRwOi8vYXBpLmNjLnNhbGVzYXBwLmFwcHRpdHVkZS5ubC8iPgogICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgIDxzb2FwZW52OkJvZHk+CiAgICAgICAgPGFwaTpzdG9yZVByb2R1Y3RzPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgICAgIDxhcGk6cHJvZHVjdD4KICAgICAgICAgICAgICAgIDxhcGk6dW5pcXVlSWQ+U1RZTEUtMDAxPC9hcGk6dW5pcXVlSWQ+CiAgICAgICAgICAgICAgICA8YXBpOm5hbWU+Q2xhc3NpYyBULVNoaXJ0PC9hcGk6bmFtZT4KICAgICAgICAgICAgICAgIDxhcGk6ZGVzY3JpcHRpb24+UHJlbWl1bSBjb3R0b24gdC1zaGlydDwvYXBpOmRlc2NyaXB0aW9uPgogICAgICAgICAgICAgICAgPGFwaTpicmFuZE5hbWU+WW91ciBCcmFuZDwvYXBpOmJyYW5kTmFtZT4KICAgICAgICAgICAgICAgIDxhcGk6Y29sb3JDb2RlPkJMSzwvYXBpOmNvbG9yQ29kZT4KICAgICAgICAgICAgICAgIDxhcGk6Y29sb3JEZXNjPkJsYWNrPC9hcGk6Y29sb3JEZXNjPgogICAgICAgICAgICAgICAgPGFwaTpwcmljZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOmN1cnJlbmN5Q29kZT5FVVI8L2FwaTpjdXJyZW5jeUNvZGU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpyZXRhaWxQcmljZT40OS45NTwvYXBpOnJldGFpbFByaWNlPgogICAgICAgICAgICAgICAgICAgIDxhcGk6d2hvbGVzYWxlUHJpY2U+MjUuMDA8L2FwaTp3aG9sZXNhbGVQcmljZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOm5ldD5mYWxzZTwvYXBpOm5ldD4KICAgICAgICAgICAgICAgIDwvYXBpOnByaWNlPgogICAgICAgICAgICAgICAgPGFwaTpzaXplPgogICAgICAgICAgICAgICAgICAgIDxhcGk6bmFtZT5TPC9hcGk6bmFtZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnNvcnRDb2RlPjE8L2FwaTpzb3J0Q29kZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnN0b2NrTGV2ZWw+CiAgICAgICAgICAgICAgICAgICAgICAgIDxhcGk6cXVhbnRpdHk+MTAwPC9hcGk6cXVhbnRpdHk+CiAgICAgICAgICAgICAgICAgICAgPC9hcGk6c3RvY2tMZXZlbD4KICAgICAgICAgICAgICAgIDwvYXBpOnNpemU+CiAgICAgICAgICAgICAgICA8YXBpOnNpemU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpuYW1lPk08L2FwaTpuYW1lPgogICAgICAgICAgICAgICAgICAgIDxhcGk6c29ydENvZGU+MjwvYXBpOnNvcnRDb2RlPgogICAgICAgICAgICAgICAgICAgIDxhcGk6c3RvY2tMZXZlbD4KICAgICAgICAgICAgICAgICAgICAgICAgPGFwaTpxdWFudGl0eT4xNTA8L2FwaTpxdWFudGl0eT4KICAgICAgICAgICAgICAgICAgICA8L2FwaTpzdG9ja0xldmVsPgogICAgICAgICAgICAgICAgPC9hcGk6c2l6ZT4KICAgICAgICAgICAgICAgIDxhcGk6c2l6ZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOm5hbWU+TDwvYXBpOm5hbWU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpzb3J0Q29kZT4zPC9hcGk6c29ydENvZGU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpzdG9ja0xldmVsPgogICAgICAgICAgICAgICAgICAgICAgICA8YXBpOnF1YW50aXR5PjEyMDwvYXBpOnF1YW50aXR5PgogICAgICAgICAgICAgICAgICAgIDwvYXBpOnN0b2NrTGV2ZWw+CiAgICAgICAgICAgICAgICA8L2FwaTpzaXplPgogICAgICAgICAgICAgICAgPGFwaTptZWRpdW0+CiAgICAgICAgICAgICAgICAgICAgPGFwaTp0eXBlPklNQUdFX1BSSU1BUlk8L2FwaTp0eXBlPgogICAgICAgICAgICAgICAgICAgIDxhcGk6dXJsPmh0dHBzOi8vY2RuLmV4YW1wbGUuY29tL3Byb2R1Y3RzL3N0eWxlLTAwMS1ibGsuanBnPC9hcGk6dXJsPgogICAgICAgICAgICAgICAgPC9hcGk6bWVkaXVtPgogICAgICAgICAgICA8L2FwaTpwcm9kdWN0PgogICAgICAgIDwvYXBpOnN0b3JlUHJvZHVjdHM+CiAgICA8L3NvYXBlbnY6Qm9keT4KPC9zb2FwZW52OkVudmVsb3BlPg==)
{% endtab %}

{% tab title="Response" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <ns2:storeProductsResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
    </soap:Body>
</soap:Envelope>
```

An empty response body indicates success.
{% endtab %}
{% endtabs %}

---

## Step 4: Store Your First Customer

{% tabs %}
{% tab title="Request" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeCustomers>
            <api:apiKey>your-api-key</api:apiKey>
            <api:customer>
                <api:customerNo>CUST-001</api:customerNo>
                <api:name>Fashion Boutique Amsterdam</api:name>
                <api:email>orders@fashionboutique.nl</api:email>
                <api:currencyCode>EUR</api:currencyCode>
                <api:retailCurrencyCode>EUR</api:retailCurrencyCode>
            </api:customer>
        </api:storeCustomers>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHNvYXBlbnY6RW52ZWxvcGUKICAgIHhtbG5zOnNvYXBlbnY9Imh0dHA6Ly9zY2hlbWFzLnhtbHNvYXAub3JnL3NvYXAvZW52ZWxvcGUvIgogICAgeG1sbnM6YXBpPSJodHRwOi8vYXBpLmNjLnNhbGVzYXBwLmFwcHRpdHVkZS5ubC8iPgogICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgIDxzb2FwZW52OkJvZHk+CiAgICAgICAgPGFwaTpzdG9yZUN1c3RvbWVycz4KICAgICAgICAgICAgPGFwaTphcGlLZXk+eW91ci1hcGkta2V5PC9hcGk6YXBpS2V5PgogICAgICAgICAgICA8YXBpOmN1c3RvbWVyPgogICAgICAgICAgICAgICAgPGFwaTpjdXN0b21lck5vPkNVU1QtMDAxPC9hcGk6Y3VzdG9tZXJObz4KICAgICAgICAgICAgICAgIDxhcGk6bmFtZT5GYXNoaW9uIEJvdXRpcXVlIEFtc3RlcmRhbTwvYXBpOm5hbWU+CiAgICAgICAgICAgICAgICA8YXBpOmVtYWlsPm9yZGVyc0BmYXNoaW9uYm91dGlxdWUubmw8L2FwaTplbWFpbD4KICAgICAgICAgICAgICAgIDxhcGk6Y3VycmVuY3lDb2RlPkVVUjwvYXBpOmN1cnJlbmN5Q29kZT4KICAgICAgICAgICAgICAgIDxhcGk6cmV0YWlsQ3VycmVuY3lDb2RlPkVVUjwvYXBpOnJldGFpbEN1cnJlbmN5Q29kZT4KICAgICAgICAgICAgPC9hcGk6Y3VzdG9tZXI+CiAgICAgICAgPC9hcGk6c3RvcmVDdXN0b21lcnM+CiAgICA8L3NvYXBlbnY6Qm9keT4KPC9zb2FwZW52OkVudmVsb3BlPg==)
{% endtab %}

{% tab title="Response" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <ns2:storeCustomersResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
    </soap:Body>
</soap:Envelope>
```

An empty response body indicates success.
{% endtab %}
{% endtabs %}

---

## Step 5: Check for Orders

After your customers place orders in the Colect app or B2B webstore:

{% tabs %}
{% tab title="Request" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:getUnprocessedOrders>
            <api:apiKey>your-api-key</api:apiKey>
        </api:getUnprocessedOrders>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPHNvYXBlbnY6RW52ZWxvcGUKICAgIHhtbG5zOnNvYXBlbnY9Imh0dHA6Ly9zY2hlbWFzLnhtbHNvYXAub3JnL3NvYXAvZW52ZWxvcGUvIgogICAgeG1sbnM6YXBpPSJodHRwOi8vYXBpLmNjLnNhbGVzYXBwLmFwcHRpdHVkZS5ubC8iPgogICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgIDxzb2FwZW52OkJvZHk+CiAgICAgICAgPGFwaTpnZXRVbnByb2Nlc3NlZE9yZGVycz4KICAgICAgICAgICAgPGFwaTphcGlLZXk+eW91ci1hcGkta2V5PC9hcGk6YXBpS2V5PgogICAgICAgIDwvYXBpOmdldFVucHJvY2Vzc2VkT3JkZXJzPgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)
{% endtab %}

{% tab title="Response" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <ns2:getUnprocessedOrdersResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/">
            <ns2:return>
                <ns2:orderNumber>ORD123456789</ns2:orderNumber>
                <ns2:customerNo>CUST-001</ns2:customerNo>
                <ns2:timestamp>2025-01-15T10:30:00</ns2:timestamp>
                <ns2:currency>EUR</ns2:currency>
                <ns2:totalAmount>250.00</ns2:totalAmount>
                <ns2:totalCount>10</ns2:totalCount>
                <!-- ... order lines and additional fields ... -->
            </ns2:return>
        </ns2:getUnprocessedOrdersResponse>
    </soap:Body>
</soap:Envelope>
```

Returns all orders that have not yet been marked as processed.
{% endtab %}
{% endtabs %}

---

## Common Integration Pattern

```
┌──────────────────────────────────────────────────────────────┐
│                    Daily Sync Schedule                        │
├──────────────────────────────────────────────────────────────┤
│  06:00  storeFullProductSet     Full product catalog sync     │
│  06:30  storeFullCustomerSet    Full customer sync            │
│  Every  updateStock             Real-time stock updates       │
│  hour   updatePrices            Price changes                 │
│  Every  getUnprocessedOrders    Poll for new orders           │
│  5 min  markOrdersAsProcessed   Confirm order processing      │
└──────────────────────────────────────────────────────────────┘
```

{% hint style="info" %}
**Best Practice:** Use `storeFullProductSet` and `storeFullCustomerSet` for nightly full syncs, and the partial update operations (`updateStock`, `updatePrices`) for real-time changes during the day.
{% endhint %}

---

## Next Steps

- [Product Operations](../operations/products.md) - Complete product sync guide
- [Customer Operations](../operations/customers.md) - Customer management
- [Order Operations](../operations/orders.md) - Order retrieval and processing
