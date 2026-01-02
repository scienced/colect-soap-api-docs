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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOnN0b3JlSGlzdG9yaWNhbE9yZGVycz4KICAgICAgICAgICAgPGFwaTphcGlLZXk+eW91ci1hcGkta2V5PC9hcGk6YXBpS2V5PgogICAgICAgICAgICA8YXBpOmhpc3RvcmljYWxPcmRlcj4KICAgICAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJObz5DVVNULTAwMTwvYXBpOmN1c3RvbWVyTm8+CiAgICAgICAgICAgICAgICA8YXBpOm9yZGVyTnVtYmVyPkVSUC0yMDI0LTAwNTQyPC9hcGk6b3JkZXJOdW1iZXI+CiAgICAgICAgICAgICAgICA8YXBpOmVycE9yZGVyUmVmZXJlbmNlPkVSUC0yMDI0LTAwNTQyPC9hcGk6ZXJwT3JkZXJSZWZlcmVuY2U+CiAgICAgICAgICAgICAgICA8YXBpOmN1c3RvbWVyT3JkZXJSZWZlcmVuY2U+UE8tMjAyNC0wMDg5PC9hcGk6Y3VzdG9tZXJPcmRlclJlZmVyZW5jZT4KICAgICAgICAgICAgICAgIDxhcGk6dGltZXN0YW1wPjIwMjQtMTEtMTVUMTA6MzA6MDA8L2FwaTp0aW1lc3RhbXA+CiAgICAgICAgICAgICAgICA8YXBpOnNhbGVzUGVyc29uPnNhbGVzQHlvdXJjb21wYW55LmNvbTwvYXBpOnNhbGVzUGVyc29uPgogICAgICAgICAgICAgICAgPGFwaTpjdXN0b21lclByaWNlR3JvdXA+UkVUQUlMPC9hcGk6Y3VzdG9tZXJQcmljZUdyb3VwPgogICAgICAgICAgICAgICAgPGFwaTpjb2xsZWN0aW9uPkZhbGwvV2ludGVyIDIwMjQ8L2FwaTpjb2xsZWN0aW9uPgogICAgICAgICAgICAgICAgPGFwaTpjaGFubmVsPkIyQiBBcHA8L2FwaTpjaGFubmVsPgogICAgICAgICAgICAgICAgPGFwaTpjb21tZW50PlJ1c2ggb3JkZXIgLSBwcmlvcml0eSBzaGlwcGluZzwvYXBpOmNvbW1lbnQ+CiAgICAgICAgICAgICAgICA8YXBpOmludGVybmFsT3JkZXJUeXBlPk9SREVSPC9hcGk6aW50ZXJuYWxPcmRlclR5cGU+CiAgICAgICAgICAgICAgICA8YXBpOnNoaXBwaW5nTG9jYXRpb24+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpjb2RlPk1BSU48L2FwaTpjb2RlPgogICAgICAgICAgICAgICAgICAgIDxhcGk6bmFtZT5NYWluIFN0b3JlPC9hcGk6bmFtZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnN0cmVldD5LYWx2ZXJzdHJhYXQ8L2FwaTpzdHJlZXQ+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpob3VzZU51bWJlcj4xMjM8L2FwaTpob3VzZU51bWJlcj4KICAgICAgICAgICAgICAgICAgICA8YXBpOnBvc3RhbENvZGU+MTAxMiBQQTwvYXBpOnBvc3RhbENvZGU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpjaXR5PkFtc3RlcmRhbTwvYXBpOmNpdHk+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpjb3VudHJ5Pk5ldGhlcmxhbmRzPC9hcGk6Y291bnRyeT4KICAgICAgICAgICAgICAgIDwvYXBpOnNoaXBwaW5nTG9jYXRpb24+CiAgICAgICAgICAgICAgICA8YXBpOm9yZGVyTGluZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnN0YXR1cz5ERUxJVkVSRUQ8L2FwaTpzdGF0dXM+CiAgICAgICAgICAgICAgICAgICAgPGFwaTp0eXBlPlNUPC9hcGk6dHlwZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnByb2R1Y3RVbmlxdWVJZD5TVFlMRS0wMDE8L2FwaTpwcm9kdWN0VW5pcXVlSWQ+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpwcm9kdWN0Q29sb3JDb2RlPkJMSzwvYXBpOnByb2R1Y3RDb2xvckNvZGU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpwcm9kdWN0RGVzY3JpcHRpb24+Q2xhc3NpYyBULVNoaXJ0IC0gQmxhY2s8L2FwaTpwcm9kdWN0RGVzY3JpcHRpb24+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpwcm9kdWN0Q2F0ZWdvcnk+VC1TaGlydHM8L2FwaTpwcm9kdWN0Q2F0ZWdvcnk+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpwcm9kdWN0U2l6ZT5NPC9hcGk6cHJvZHVjdFNpemU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTplYW5Db2RlPjg3MTIzNDU2Nzg5MDI8L2FwaTplYW5Db2RlPgogICAgICAgICAgICAgICAgICAgIDxhcGk6cXVhbnRpdHk+MTA8L2FwaTpxdWFudGl0eT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnF1YW50aXR5RGVsaXZlcmVkPjEwPC9hcGk6cXVhbnRpdHlEZWxpdmVyZWQ+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpjdXJyZW5jeT5FVVI8L2FwaTpjdXJyZW5jeT4KICAgICAgICAgICAgICAgICAgICA8YXBpOm5ldFdob2xlc2FsZVByaWNlPjI1LjAwPC9hcGk6bmV0V2hvbGVzYWxlUHJpY2U+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpuZXRMaW5lQW1vdW50PjI1MC4wMDwvYXBpOm5ldExpbmVBbW91bnQ+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpzaGlwbWVudERhdGU+MjAyNC0xMS0yMFQwMDowMDowMDwvYXBpOnNoaXBtZW50RGF0ZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnRyYWNrVHJhY2VVcmw+aHR0cHM6Ly90cmFja2luZy5jYXJyaWVyLmNvbS9BQkMxMjM8L2FwaTp0cmFja1RyYWNlVXJsPgogICAgICAgICAgICAgICAgICAgIDxhcGk6cmV0dXJuQWxsb3dlZD50cnVlPC9hcGk6cmV0dXJuQWxsb3dlZD4KICAgICAgICAgICAgICAgIDwvYXBpOm9yZGVyTGluZT4KICAgICAgICAgICAgPC9hcGk6aGlzdG9yaWNhbE9yZGVyPgogICAgICAgIDwvYXBpOnN0b3JlSGlzdG9yaWNhbE9yZGVycz4KICAgIDwvc29hcGVudjpCb2R5Pgo8L3NvYXBlbnY6RW52ZWxvcGU+)

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOnN0b3JlRnVsbEhpc3RvcmljYWxPcmRlclNldEZvckN1c3RvbWVyPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJObz5DVVNULTAwMTwvYXBpOmN1c3RvbWVyTm8+CiAgICAgICAgICAgIDxhcGk6aGlzdG9yaWNhbE9yZGVyPgogICAgICAgICAgICAgICAgPGFwaTpjdXN0b21lck5vPkNVU1QtMDAxPC9hcGk6Y3VzdG9tZXJObz4KICAgICAgICAgICAgICAgIDxhcGk6b3JkZXJOdW1iZXI+RVJQLTIwMjQtMDA1NDI8L2FwaTpvcmRlck51bWJlcj4KICAgICAgICAgICAgICAgIDxhcGk6ZXJwT3JkZXJSZWZlcmVuY2U+RVJQLTIwMjQtMDA1NDI8L2FwaTplcnBPcmRlclJlZmVyZW5jZT4KICAgICAgICAgICAgICAgIDxhcGk6dGltZXN0YW1wPjIwMjQtMTEtMTVUMTA6MzA6MDA8L2FwaTp0aW1lc3RhbXA+CiAgICAgICAgICAgIDwvYXBpOmhpc3RvcmljYWxPcmRlcj4KICAgICAgICAgICAgPGFwaTpoaXN0b3JpY2FsT3JkZXI+CiAgICAgICAgICAgICAgICA8YXBpOmN1c3RvbWVyTm8+Q1VTVC0wMDE8L2FwaTpjdXN0b21lck5vPgogICAgICAgICAgICAgICAgPGFwaTpvcmRlck51bWJlcj5FUlAtMjAyNC0wMDQ4OTwvYXBpOm9yZGVyTnVtYmVyPgogICAgICAgICAgICAgICAgPGFwaTplcnBPcmRlclJlZmVyZW5jZT5FUlAtMjAyNC0wMDQ4OTwvYXBpOmVycE9yZGVyUmVmZXJlbmNlPgogICAgICAgICAgICAgICAgPGFwaTp0aW1lc3RhbXA+MjAyNC0xMC0yMFQxNDowMDowMDwvYXBpOnRpbWVzdGFtcD4KICAgICAgICAgICAgPC9hcGk6aGlzdG9yaWNhbE9yZGVyPgogICAgICAgIDwvYXBpOnN0b3JlRnVsbEhpc3RvcmljYWxPcmRlclNldEZvckN1c3RvbWVyPgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)

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
