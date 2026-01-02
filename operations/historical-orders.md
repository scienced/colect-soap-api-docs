# Historical Order Operations

Push historical order data to Colect so your customers and sales reps can view past orders, track shipments, and reorder from order history.

{% hint style="success" %}
**Full Example:** [Download complete storeFullHistoricalOrderSetForCustomer.xml](https://assets.apptitude.nl/KnowledgeBase/Webservice_3_0/External/storeFullHistoricalOrderSetForCustomer.xml) - A comprehensive example showing all available historical order fields.
{% endhint %}

---

## Operations Overview

| Operation | Type | Description |
|-----------|------|-------------|
| [`storeHistoricalOrders`](#storehistoricalorders) | Partial Sync | Add or update historical orders |
| [`storeFullHistoricalOrderSetForCustomer`](#storefullhistoricalordersetforcustomer) | Full Sync | Replace all historical orders for a customer |
| [`deleteHistoricalOrders`](#deletehistoricalorders) | Delete | Remove specific historical orders |
| [`deleteAllHistoricalOrders`](#deleteallhistoricalorders) | Delete | Clear all historical orders |

---

## storeHistoricalOrders

Add new historical orders or update existing ones. Orders not included remain unchanged.

{% hint style="info" %}
Historical orders are primarily for display purposes - they show customers their order history from your ERP system.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeHistoricalOrders>
            <api:apiKey>your-api-key</api:apiKey>
            <api:historicalOrder>
                <api:customerNo>CUST-001</api:customerNo>
                <api:orderNumber>ERP-2024-00542</api:orderNumber>
                <api:erpOrderReference>ERP-2024-00542</api:erpOrderReference>
                <api:customerOrderReference>PO-2024-0089</api:customerOrderReference>
                <api:timestamp>2024-11-15T10:30:00</api:timestamp>
                <api:salesPerson>sales@yourcompany.com</api:salesPerson>
                <api:customerPriceGroup>RETAIL</api:customerPriceGroup>
                <api:collection>Fall/Winter 2024</api:collection>
                <api:channel>B2B App</api:channel>
                <api:comment>Rush order - priority shipping</api:comment>
                <api:internalOrderType>ORDER</api:internalOrderType>

                <api:shippingLocation>
                    <api:code>MAIN</api:code>
                    <api:name>Main Store</api:name>
                    <api:street>Kalverstraat</api:street>
                    <api:houseNumber>123</api:houseNumber>
                    <api:postalCode>1012 PA</api:postalCode>
                    <api:city>Amsterdam</api:city>
                    <api:country>Netherlands</api:country>
                </api:shippingLocation>

                <api:orderLine>
                    <api:status>DELIVERED</api:status>
                    <api:type>ST</api:type>
                    <api:productUniqueId>STYLE-001</api:productUniqueId>
                    <api:productColorCode>BLK</api:productColorCode>
                    <api:productDescription>Classic T-Shirt - Black</api:productDescription>
                    <api:productCategory>T-Shirts</api:productCategory>
                    <api:productSize>M</api:productSize>
                    <api:productSizeSortCode>2</api:productSizeSortCode>
                    <api:eanCode>8712345678902</api:eanCode>
                    <api:quantity>10</api:quantity>
                    <api:quantityDelivered>10</api:quantityDelivered>
                    <api:currency>EUR</api:currency>
                    <api:netWholesalePrice>25.00</api:netWholesalePrice>
                    <api:grossWholesalePrice>25.00</api:grossWholesalePrice>
                    <api:netLineAmount>250.00</api:netLineAmount>
                    <api:grossLineAmount>250.00</api:grossLineAmount>
                    <api:shipmentDate>2024-11-20T00:00:00</api:shipmentDate>
                    <api:packingSlipNumber>PS-2024-01234</api:packingSlipNumber>
                    <api:invoiceNumber>INV-2024-00678</api:invoiceNumber>
                    <api:trackTraceUrl>https://tracking.carrier.com/ABC123</api:trackTraceUrl>
                    <api:returnAllowed>true</api:returnAllowed>
                    <api:productImageUrl>https://cdn.example.com/style-001-blk.jpg</api:productImageUrl>
                </api:orderLine>
                <api:orderLine>
                    <api:status>ORDERED</api:status>
                    <api:type>PS</api:type>
                    <api:productUniqueId>STYLE-002</api:productUniqueId>
                    <api:productColorCode>NVY</api:productColorCode>
                    <api:productDescription>Premium Polo - Navy</api:productDescription>
                    <api:productSize>L</api:productSize>
                    <api:quantity>5</api:quantity>
                    <api:quantityDelivered>0</api:quantityDelivered>
                    <api:currency>EUR</api:currency>
                    <api:netWholesalePrice>45.00</api:netWholesalePrice>
                    <api:netLineAmount>225.00</api:netLineAmount>
                    <api:returnAllowed>false</api:returnAllowed>
                </api:orderLine>
            </api:historicalOrder>
        </api:storeHistoricalOrders>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOnN0b3JlSGlzdG9yaWNhbE9yZGVycz4KICAgICAgICAgICAgPGFwaTphcGlLZXk+eW91ci1hcGkta2V5PC9hcGk6YXBpS2V5PgogICAgICAgICAgICA8YXBpOmhpc3RvcmljYWxPcmRlcj4KICAgICAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJObz5DVVNULTAwMTwvYXBpOmN1c3RvbWVyTm8+CiAgICAgICAgICAgICAgICA8YXBpOm9yZGVyTnVtYmVyPkVSUC0yMDI0LTAwNTQyPC9hcGk6b3JkZXJOdW1iZXI+CiAgICAgICAgICAgICAgICA8YXBpOmVycE9yZGVyUmVmZXJlbmNlPkVSUC0yMDI0LTAwNTQyPC9hcGk6ZXJwT3JkZXJSZWZlcmVuY2U+CiAgICAgICAgICAgICAgICA8YXBpOmN1c3RvbWVyT3JkZXJSZWZlcmVuY2U+UE8tMjAyNC0wMDg5PC9hcGk6Y3VzdG9tZXJPcmRlclJlZmVyZW5jZT4KICAgICAgICAgICAgICAgIDxhcGk6dGltZXN0YW1wPjIwMjQtMTEtMTVUMTA6MzA6MDA8L2FwaTp0aW1lc3RhbXA+CiAgICAgICAgICAgICAgICA8YXBpOnNhbGVzUGVyc29uPnNhbGVzQHlvdXJjb21wYW55LmNvbTwvYXBpOnNhbGVzUGVyc29uPgogICAgICAgICAgICAgICAgPGFwaTpjdXN0b21lclByaWNlR3JvdXA+UkVUQUlMPC9hcGk6Y3VzdG9tZXJQcmljZUdyb3VwPgogICAgICAgICAgICAgICAgPGFwaTpjb2xsZWN0aW9uPkZhbGwvV2ludGVyIDIwMjQ8L2FwaTpjb2xsZWN0aW9uPgogICAgICAgICAgICAgICAgPGFwaTpjaGFubmVsPkIyQiBBcHA8L2FwaTpjaGFubmVsPgogICAgICAgICAgICAgICAgPGFwaTpjb21tZW50PlJ1c2ggb3JkZXIgLSBwcmlvcml0eSBzaGlwcGluZzwvYXBpOmNvbW1lbnQ+CiAgICAgICAgICAgICAgICA8YXBpOmludGVybmFsT3JkZXJUeXBlPk9SREVSPC9hcGk6aW50ZXJuYWxPcmRlclR5cGU+CgogICAgICAgICAgICAgICAgPGFwaTpzaGlwcGluZ0xvY2F0aW9uPgogICAgICAgICAgICAgICAgICAgIDxhcGk6Y29kZT5NQUlOPC9hcGk6Y29kZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOm5hbWU+TWFpbiBTdG9yZTwvYXBpOm5hbWU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpzdHJlZXQ+S2FsdmVyc3RyYWF0PC9hcGk6c3RyZWV0PgogICAgICAgICAgICAgICAgICAgIDxhcGk6aG91c2VOdW1iZXI+MTIzPC9hcGk6aG91c2VOdW1iZXI+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpwb3N0YWxDb2RlPjEwMTIgUEE8L2FwaTpwb3N0YWxDb2RlPgogICAgICAgICAgICAgICAgICAgIDxhcGk6Y2l0eT5BbXN0ZXJkYW08L2FwaTpjaXR5PgogICAgICAgICAgICAgICAgICAgIDxhcGk6Y291bnRyeT5OZXRoZXJsYW5kczwvYXBpOmNvdW50cnk+CiAgICAgICAgICAgICAgICA8L2FwaTpzaGlwcGluZ0xvY2F0aW9uPgoKICAgICAgICAgICAgICAgIDxhcGk6b3JkZXJMaW5lPgogICAgICAgICAgICAgICAgICAgIDxhcGk6c3RhdHVzPkRFTElWRVJFRDwvYXBpOnN0YXR1cz4KICAgICAgICAgICAgICAgICAgICA8YXBpOnR5cGU+U1Q8L2FwaTp0eXBlPgogICAgICAgICAgICAgICAgICAgIDxhcGk6cHJvZHVjdFVuaXF1ZUlkPlNUWUxFLTAwMTwvYXBpOnByb2R1Y3RVbmlxdWVJZD4KICAgICAgICAgICAgICAgICAgICA8YXBpOnByb2R1Y3RDb2xvckNvZGU+QkxLPC9hcGk6cHJvZHVjdENvbG9yQ29kZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnByb2R1Y3REZXNjcmlwdGlvbj5DbGFzc2ljIFQtU2hpcnQgLSBCbGFjazwvYXBpOnByb2R1Y3REZXNjcmlwdGlvbj4KICAgICAgICAgICAgICAgICAgICA8YXBpOnByb2R1Y3RDYXRlZ29yeT5ULVNoaXJ0czwvYXBpOnByb2R1Y3RDYXRlZ29yeT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnByb2R1Y3RTaXplPk08L2FwaTpwcm9kdWN0U2l6ZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnByb2R1Y3RTaXplU29ydENvZGU+MjwvYXBpOnByb2R1Y3RTaXplU29ydENvZGU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTplYW5Db2RlPjg3MTIzNDU2Nzg5MDI8L2FwaTplYW5Db2RlPgogICAgICAgICAgICAgICAgICAgIDxhcGk6cXVhbnRpdHk+MTA8L2FwaTpxdWFudGl0eT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnF1YW50aXR5RGVsaXZlcmVkPjEwPC9hcGk6cXVhbnRpdHlEZWxpdmVyZWQ+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpjdXJyZW5jeT5FVVI8L2FwaTpjdXJyZW5jeT4KICAgICAgICAgICAgICAgICAgICA8YXBpOm5ldFdob2xlc2FsZVByaWNlPjI1LjAwPC9hcGk6bmV0V2hvbGVzYWxlUHJpY2U+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpncm9zc1dob2xlc2FsZVByaWNlPjI1LjAwPC9hcGk6Z3Jvc3NXaG9sZXNhbGVQcmljZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOm5ldExpbmVBbW91bnQ+MjUwLjAwPC9hcGk6bmV0TGluZUFtb3VudD4KICAgICAgICAgICAgICAgICAgICA8YXBpOmdyb3NzTGluZUFtb3VudD4yNTAuMDA8L2FwaTpncm9zc0xpbmVBbW91bnQ+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpzaGlwbWVudERhdGU+MjAyNC0xMS0yMFQwMDowMDowMDwvYXBpOnNoaXBtZW50RGF0ZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnBhY2tpbmdTbGlwTnVtYmVyPlBTLTIwMjQtMDEyMzQ8L2FwaTpwYWNraW5nU2xpcE51bWJlcj4KICAgICAgICAgICAgICAgICAgICA8YXBpOmludm9pY2VOdW1iZXI+SU5WLTIwMjQtMDA2Nzg8L2FwaTppbnZvaWNlTnVtYmVyPgogICAgICAgICAgICAgICAgICAgIDxhcGk6dHJhY2tUcmFjZVVybD5odHRwczovL3RyYWNraW5nLmNhcnJpZXIuY29tL0FCQzEyMzwvYXBpOnRyYWNrVHJhY2VVcmw+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpyZXR1cm5BbGxvd2VkPnRydWU8L2FwaTpyZXR1cm5BbGxvd2VkPgogICAgICAgICAgICAgICAgICAgIDxhcGk6cHJvZHVjdEltYWdlVXJsPmh0dHBzOi8vY2RuLmV4YW1wbGUuY29tL3N0eWxlLTAwMS1ibGsuanBnPC9hcGk6cHJvZHVjdEltYWdlVXJsPgogICAgICAgICAgICAgICAgPC9hcGk6b3JkZXJMaW5lPgogICAgICAgICAgICAgICAgPGFwaTpvcmRlckxpbmU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpzdGF0dXM+T1JERVJFRDwvYXBpOnN0YXR1cz4KICAgICAgICAgICAgICAgICAgICA8YXBpOnR5cGU+UFM8L2FwaTp0eXBlPgogICAgICAgICAgICAgICAgICAgIDxhcGk6cHJvZHVjdFVuaXF1ZUlkPlNUWUxFLTAwMjwvYXBpOnByb2R1Y3RVbmlxdWVJZD4KICAgICAgICAgICAgICAgICAgICA8YXBpOnByb2R1Y3RDb2xvckNvZGU+TlZZPC9hcGk6cHJvZHVjdENvbG9yQ29kZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnByb2R1Y3REZXNjcmlwdGlvbj5QcmVtaXVtIFBvbG8gLSBOYXZ5PC9hcGk6cHJvZHVjdERlc2NyaXB0aW9uPgogICAgICAgICAgICAgICAgICAgIDxhcGk6cHJvZHVjdFNpemU+TDwvYXBpOnByb2R1Y3RTaXplPgogICAgICAgICAgICAgICAgICAgIDxhcGk6cXVhbnRpdHk+NTwvYXBpOnF1YW50aXR5PgogICAgICAgICAgICAgICAgICAgIDxhcGk6cXVhbnRpdHlEZWxpdmVyZWQ+MDwvYXBpOnF1YW50aXR5RGVsaXZlcmVkPgogICAgICAgICAgICAgICAgICAgIDxhcGk6Y3VycmVuY3k+RVVSPC9hcGk6Y3VycmVuY3k+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpuZXRXaG9sZXNhbGVQcmljZT40NS4wMDwvYXBpOm5ldFdob2xlc2FsZVByaWNlPgogICAgICAgICAgICAgICAgICAgIDxhcGk6bmV0TGluZUFtb3VudD4yMjUuMDA8L2FwaTpuZXRMaW5lQW1vdW50PgogICAgICAgICAgICAgICAgICAgIDxhcGk6cmV0dXJuQWxsb3dlZD5mYWxzZTwvYXBpOnJldHVybkFsbG93ZWQ+CiAgICAgICAgICAgICAgICA8L2FwaTpvcmRlckxpbmU+CiAgICAgICAgICAgIDwvYXBpOmhpc3RvcmljYWxPcmRlcj4KICAgICAgICA8L2FwaTpzdG9yZUhpc3RvcmljYWxPcmRlcnM+CiAgICA8L3NvYXBlbnY6Qm9keT4KPC9zb2FwZW52OkVudmVsb3BlPg==)

### Response

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Body>
        <ns2:storeHistoricalOrdersResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
    </soapenv:Body>
</soapenv:Envelope>
```

---

## storeFullHistoricalOrderSetForCustomer

Replace all historical orders for a specific customer.

{% hint style="warning" %}
Only affects orders for the specified customer. Other customers' historical orders remain unchanged.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeFullHistoricalOrderSetForCustomer>
            <api:apiKey>your-api-key</api:apiKey>
            <api:customerNo>CUST-001</api:customerNo>
            <api:historicalOrder>
                <api:customerNo>CUST-001</api:customerNo>
                <api:orderNumber>ERP-2024-00542</api:orderNumber>
                <!-- ... full order data ... -->
            </api:historicalOrder>
            <api:historicalOrder>
                <api:customerNo>CUST-001</api:customerNo>
                <api:orderNumber>ERP-2024-00489</api:orderNumber>
                <!-- ... full order data ... -->
            </api:historicalOrder>
        </api:storeFullHistoricalOrderSetForCustomer>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOnN0b3JlRnVsbEhpc3RvcmljYWxPcmRlclNldEZvckN1c3RvbWVyPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJObz5DVVNULTAwMTwvYXBpOmN1c3RvbWVyTm8+CiAgICAgICAgICAgIDxhcGk6aGlzdG9yaWNhbE9yZGVyPgogICAgICAgICAgICAgICAgPGFwaTpjdXN0b21lck5vPkNVU1QtMDAxPC9hcGk6Y3VzdG9tZXJObz4KICAgICAgICAgICAgICAgIDxhcGk6b3JkZXJOdW1iZXI+RVJQLTIwMjQtMDA1NDI8L2FwaTpvcmRlck51bWJlcj4KICAgICAgICAgICAgICAgIDwhLS0gLi4uIGZ1bGwgb3JkZXIgZGF0YSAuLi4gLS0+CiAgICAgICAgICAgIDwvYXBpOmhpc3RvcmljYWxPcmRlcj4KICAgICAgICAgICAgPGFwaTpoaXN0b3JpY2FsT3JkZXI+CiAgICAgICAgICAgICAgICA8YXBpOmN1c3RvbWVyTm8+Q1VTVC0wMDE8L2FwaTpjdXN0b21lck5vPgogICAgICAgICAgICAgICAgPGFwaTpvcmRlck51bWJlcj5FUlAtMjAyNC0wMDQ4OTwvYXBpOm9yZGVyTnVtYmVyPgogICAgICAgICAgICAgICAgPCEtLSAuLi4gZnVsbCBvcmRlciBkYXRhIC4uLiAtLT4KICAgICAgICAgICAgPC9hcGk6aGlzdG9yaWNhbE9yZGVyPgogICAgICAgIDwvYXBpOnN0b3JlRnVsbEhpc3RvcmljYWxPcmRlclNldEZvckN1c3RvbWVyPgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)

---

## deleteHistoricalOrders

Remove specific historical orders.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:deleteHistoricalOrders>
            <api:apiKey>your-api-key</api:apiKey>
            <api:historicalOrder>
                <api:customerNo>CUST-001</api:customerNo>
                <api:orderNumber>ERP-2024-00542</api:orderNumber>
                <!-- Minimal identification fields -->
            </api:historicalOrder>
        </api:deleteHistoricalOrders>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOmRlbGV0ZUhpc3RvcmljYWxPcmRlcnM+CiAgICAgICAgICAgIDxhcGk6YXBpS2V5PnlvdXItYXBpLWtleTwvYXBpOmFwaUtleT4KICAgICAgICAgICAgPGFwaTpoaXN0b3JpY2FsT3JkZXI+CiAgICAgICAgICAgICAgICA8YXBpOmN1c3RvbWVyTm8+Q1VTVC0wMDE8L2FwaTpjdXN0b21lck5vPgogICAgICAgICAgICAgICAgPGFwaTpvcmRlck51bWJlcj5FUlAtMjAyNC0wMDU0MjwvYXBpOm9yZGVyTnVtYmVyPgogICAgICAgICAgICA8L2FwaTpoaXN0b3JpY2FsT3JkZXI+CiAgICAgICAgPC9hcGk6ZGVsZXRlSGlzdG9yaWNhbE9yZGVycz4KICAgIDwvc29hcGVudjpCb2R5Pgo8L3NvYXBlbnY6RW52ZWxvcGU+)

---

## deleteAllHistoricalOrders

Remove all historical orders from your collection.

{% hint style="danger" %}
**Warning:** This removes ALL historical orders for ALL customers.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:deleteAllHistoricalOrders>
            <api:apiKey>your-api-key</api:apiKey>
        </api:deleteAllHistoricalOrders>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOmRlbGV0ZUFsbEhpc3RvcmljYWxPcmRlcnM+CiAgICAgICAgICAgIDxhcGk6YXBpS2V5PnlvdXItYXBpLWtleTwvYXBpOmFwaUtleT4KICAgICAgICA8L2FwaTpkZWxldGVBbGxIaXN0b3JpY2FsT3JkZXJzPgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)

---

## Historical Order Data Structure

### Order Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `customerNo` | String | Yes | Customer identifier |
| `orderNumber` | String | No | Original Colect order number (12-char) |
| `erpOrderReference` | String | No | Your ERP order reference |
| `customerOrderReference` | String | No | Customer's PO number |
| `timestamp` | DateTime | No | Order creation date/time |
| `salesPerson` | String | No | Salesperson email |
| `customerPriceGroup` | String | No | Customer's price group |
| `collection` | String | No | Collection name |
| `channel` | String | No | Order channel |
| `comment` | String | No | Order comment |
| `externalUrl` | String | No | Link to order in external system |
| `internalOrderType` | Enum | No | `ORDER` or `RETURN` |
| `orderTypeCode` | String | No | External order type code |
| `accountManager` | String | No | Account manager name |
| `shippingLocation` | Object | No | Shipping address |
| `agreement` | Object | No | Customer agreement used |

### Order Line Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `status` | String | No | Line status (ORDERED, DELIVERED, etc.) |
| `type` | String | No | ST=stock, PS=pre-order, RE=return |
| `productUniqueId` | String | No | Product identifier |
| `productColorCode` | String | No | Color code |
| `productDescription` | String | No | Product name/description |
| `productCategory` | String | No | Category name |
| `productGroup` | String | No | Product group |
| `productGender` | String | No | Gender (Men, Women, etc.) |
| `productSeason` | String | No | Season code |
| `productSize` | String | No | Size name |
| `productSizeSortCode` | Long | No | Size sort order |
| `productSubSize` | String | No | Sub-size (width, length) |
| `eanCode` | String | No | EAN/barcode |
| `quantity` | Integer | Yes | Quantity ordered |
| `quantityDelivered` | Integer | Yes | Quantity shipped |
| `currency` | String | No | Line currency |
| `netWholesalePrice` | Float | No | Net price per unit |
| `grossWholesalePrice` | Float | No | Gross price per unit |
| `netLineAmount` | Float | No | Total line value |
| `grossLineAmount` | Float | No | Gross line value |
| `netLineAmountDelivered` | Float | No | Delivered value |
| `shipmentDate` | DateTime | No | Shipment date |
| `packingSlipNumber` | String | No | Packing slip reference |
| `invoiceNumber` | String | No | Invoice reference |
| `trackTraceUrl` | String | No | Tracking URL |
| `returnAllowed` | Boolean | Yes | Can this line be returned? |
| `remark` | String | No | Line comment |
| `productImageUrl` | String | No | Product image URL |

---

## Line Status Values

| Status | Description |
|--------|-------------|
| `ORDERED` | Order placed, not yet shipped |
| `CONFIRMED` | Order confirmed by warehouse |
| `SHIPPED` | Shipped, in transit |
| `DELIVERED` | Delivered to customer |
| `BACKORDERED` | On backorder |
| `CANCELED` | Line canceled |
| `RETURNED` | Returned by customer |

---

## Line Type Values

| Type | Description |
|------|-------------|
| `ST` | Stock order |
| `PS` | Pre-order |
| `RE` | Return |

---

## Track & Trace

Provide tracking URLs for shipped orders:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeHistoricalOrders>
            <api:apiKey>your-api-key</api:apiKey>
            <api:historicalOrder>
                <!-- ... -->
                <api:orderLine>
                    <!-- ... -->
                    <api:status>SHIPPED</api:status>
                    <api:shipmentDate>2024-11-20T00:00:00</api:shipmentDate>
                    <api:trackTraceUrl>https://tracking.dhl.com/ABC123456789</api:trackTraceUrl>
                </api:orderLine>
            </api:historicalOrder>
        </api:storeHistoricalOrders>
    </soapenv:Body>
</soapenv:Envelope>
```

Customers can click to track their shipment directly.

---

## Enabling Reorders

Allow customers to reorder items from their order history:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeHistoricalOrders>
            <api:apiKey>your-api-key</api:apiKey>
            <api:historicalOrder>
                <!-- ... -->
                <api:orderLine>
                    <!-- Include product identifiers for reorder -->
                    <api:productUniqueId>STYLE-001</api:productUniqueId>
                    <api:productColorCode>BLK</api:productColorCode>
                    <api:productSize>M</api:productSize>
                    <api:eanCode>8712345678902</api:eanCode>

                    <!-- Image for visual reference -->
                    <api:productImageUrl>https://cdn.example.com/style-001-blk.jpg</api:productImageUrl>

                    <!-- Allow returns -->
                    <api:returnAllowed>true</api:returnAllowed>
                </api:orderLine>
            </api:historicalOrder>
        </api:storeHistoricalOrders>
    </soapenv:Body>
</soapenv:Envelope>
```

---

## Extra Fields

Add custom information to order lines:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeHistoricalOrders>
            <api:apiKey>your-api-key</api:apiKey>
            <api:historicalOrder>
                <!-- ... -->
                <api:orderLine>
                    <!-- ... -->
                    <api:extraField>
                        <api:name>warehouse</api:name>
                        <api:description>Shipped From</api:description>
                        <api:value>Warehouse Amsterdam</api:value>
                    </api:extraField>
                    <api:extraField>
                        <api:name>carrier</api:name>
                        <api:description>Carrier</api:description>
                        <api:value>DHL Express</api:value>
                    </api:extraField>
                </api:orderLine>
            </api:historicalOrder>
        </api:storeHistoricalOrders>
    </soapenv:Body>
</soapenv:Envelope>
```

---

## Best Practices

{% tabs %}
{% tab title="Sync Strategy" %}
```
Recommended sync patterns:

New Orders:
└── Push immediately when order is created in ERP

Status Updates:
├── When order ships → update status, add tracking
├── When delivered → update quantityDelivered
└── When invoiced → add invoiceNumber

Full Sync:
└── Nightly sync of last 12 months per customer
```
{% endtab %}

{% tab title="Data Quality" %}
**Essential fields:**
- `productUniqueId` + `productColorCode` for reordering
- `productImageUrl` for visual reference
- `trackTraceUrl` for shipment tracking
- `returnAllowed` for return functionality

**Status accuracy:**
- Keep `quantityDelivered` current
- Update `status` promptly on shipment
- Add `shipmentDate` when shipped
{% endtab %}

{% tab title="Performance" %}
- Use `storeFullHistoricalOrderSetForCustomer` for customer-level sync
- Limit history to 12-24 months to reduce data volume
- Batch multiple orders in single API calls
{% endtab %}
{% endtabs %}
