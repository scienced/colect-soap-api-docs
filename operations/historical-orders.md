# Historical Orders

Push historical order data to Colect so your customers and sales reps can view past orders, track shipments, and reorder from order history.

{% hint style="success" %}
**Full Example:** [Download complete storeFullHistoricalOrderSetForCustomer.xml](https://assets.apptitude.nl/KnowledgeBase/Webservice_3_0/External/storeFullHistoricalOrderSetForCustomer.xml) - A comprehensive example showing all available historical order fields.
{% endhint %}

***

## Operations Overview

<table><thead><tr><th width="355.21484375">Operation</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><a href="historical-orders.md#storehistoricalorders"><code>storeHistoricalOrders</code></a></td><td>Partial Sync</td><td>Add or update historical orders</td></tr><tr><td><a href="historical-orders.md#storefullhistoricalordersetforcustomer"><code>storeFullHistoricalOrderSetForCustomer</code></a></td><td>Full Sync</td><td>Replace all historical orders for a customer</td></tr><tr><td><a href="historical-orders.md#deletehistoricalorders"><code>deleteHistoricalOrders</code></a></td><td>Delete</td><td>Remove specific historical orders</td></tr><tr><td><a href="historical-orders.md#deleteallhistoricalorders"><code>deleteAllHistoricalOrders</code></a></td><td>Delete</td><td>Clear all historical orders</td></tr></tbody></table>

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#url=https%3A%2F%2Fraw.githubusercontent.com%2Fscienced%2Fcolect-soap-api-docs%2Frefs%2Fheads%2Fmain%2Fexamples%2FstoreHistoricalOrders.xml)

### Response

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Body>
        <ns2:storeHistoricalOrdersResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
    </soapenv:Body>
</soapenv:Envelope>
```

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgzIGSRgAMXcMDAj9ECKJfU5bAHkJWIBlGAYSyQBhdxqwAhgJXLy84AKDAGkYHKywdwkAWgNFgGsZ4DMofT1t6Zzg0cTx7aROPqLBiQA5MADugFVWgBVFgAY3gEZNgvP+q9uIyOmhOOzo1Vq9SaLSGQOBx1+FwGQ0Bjxe7y+P1Of0uKLuh3hCNOkli13cBAARrCUAAlAAKiwATG9GQAWDEAVlZjKxOxJKPJVOGBMJIIAhItFjgBDKcAAzcoYHD8iQ4TwQBgQaWyyVwo68pDg-p1BoYZqxPWjUGGiGSKFmmHC0VjRH-PH3J6vD7fLY7HHIm7451EvmOsmU6n0pks9kfVkADgAnAaVeGhZb4cAJVKZQJ5YrlY61RqtbmcLqRfrfTbjfbzbDKyDq8bSoqqrXTfWJO1Oj0kVcgZtROEMtkgkOwqk0AglLAAkA)

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgyRvOwYYCP0QBkl9TlsAeQlYkFy8vOACgwBpGByssHcJAFoDAYBrbuAzKH09Ka6c4JbEtqmkOnLKiWq6hpgJZsWW5enOdwqwAl2AOTAAgGEAVQBlABUBgAY3gEYJgpOzi4k132ByWBUksUu7gIACNdgEUAAlAAKAwATG9UQAWd5vACsmNRPxW4KuUNhewWIKJ0zWZ02NQw9ViwJCk2mRRgJTKdK2jJ2EialImonCGWyQWFYVSaAQSlgASAA)

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgyRvOwYYAEEMDAj9EAZJfU5bAHkJWJBcvLzgAoMAaRgcrLB3CQBaAxGAa37gMyh9PTm+nOCOmYKimBLyyuraiXqmlpgJNpWk2RTkDOygmdFwtAQlWACgA)

***

## Historical Order Data Structure

### Order Fields

| Field                    | Type     | Required | Description                            |
| ------------------------ | -------- | -------- | -------------------------------------- |
| `customerNo`             | String   | Yes      | Customer identifier                    |
| `orderNumber`            | String   | No       | Original Colect order number (12-char) |
| `erpOrderReference`      | String   | No       | Your ERP order reference               |
| `customerOrderReference` | String   | No       | Customer's PO number                   |
| `timestamp`              | DateTime | No       | Order creation date/time               |
| `salesPerson`            | String   | No       | Salesperson email                      |
| `customerPriceGroup`     | String   | No       | Customer's price group                 |
| `collection`             | String   | No       | Collection name                        |
| `channel`                | String   | No       | Order channel                          |
| `comment`                | String   | No       | Order comment                          |
| `externalUrl`            | String   | No       | Link to order in external system       |
| `internalOrderType`      | Enum     | No       | `ORDER` or `RETURN`                    |
| `orderTypeCode`          | String   | No       | External order type code               |
| `accountManager`         | String   | No       | Account manager name                   |
| `shippingLocation`       | Object   | No       | Shipping address                       |
| `agreement`              | Object   | No       | Customer agreement used                |

### Order Line Fields

| Field                    | Type     | Required | Description                            |
| ------------------------ | -------- | -------- | -------------------------------------- |
| `status`                 | String   | No       | Line status (ORDERED, DELIVERED, etc.) |
| `type`                   | String   | No       | ST=stock, PS=pre-order, RE=return      |
| `productUniqueId`        | String   | No       | Product identifier                     |
| `productColorCode`       | String   | No       | Color code                             |
| `productDescription`     | String   | No       | Product name/description               |
| `productCategory`        | String   | No       | Category name                          |
| `productGroup`           | String   | No       | Product group                          |
| `productGender`          | String   | No       | Gender (Men, Women, etc.)              |
| `productSeason`          | String   | No       | Season code                            |
| `productSize`            | String   | No       | Size name                              |
| `productSizeSortCode`    | Long     | No       | Size sort order                        |
| `productSubSize`         | String   | No       | Sub-size (width, length)               |
| `eanCode`                | String   | No       | EAN/barcode                            |
| `quantity`               | Integer  | Yes      | Quantity ordered                       |
| `quantityDelivered`      | Integer  | Yes      | Quantity shipped                       |
| `currency`               | String   | No       | Line currency                          |
| `netWholesalePrice`      | Float    | No       | Net price per unit                     |
| `grossWholesalePrice`    | Float    | No       | Gross price per unit                   |
| `netLineAmount`          | Float    | No       | Total line value                       |
| `grossLineAmount`        | Float    | No       | Gross line value                       |
| `netLineAmountDelivered` | Float    | No       | Delivered value                        |
| `shipmentDate`           | DateTime | No       | Shipment date                          |
| `packingSlipNumber`      | String   | No       | Packing slip reference                 |
| `invoiceNumber`          | String   | No       | Invoice reference                      |
| `trackTraceUrl`          | String   | No       | Tracking URL                           |
| `returnAllowed`          | Boolean  | Yes      | Can this line be returned?             |
| `remark`                 | String   | No       | Line comment                           |
| `productImageUrl`        | String   | No       | Product image URL                      |

***

## Line Status Values

| Status        | Description                   |
| ------------- | ----------------------------- |
| `ORDERED`     | Order placed, not yet shipped |
| `CONFIRMED`   | Order confirmed by warehouse  |
| `SHIPPED`     | Shipped, in transit           |
| `DELIVERED`   | Delivered to customer         |
| `BACKORDERED` | On backorder                  |
| `CANCELED`    | Line canceled                 |
| `RETURNED`    | Returned by customer          |

***

## Line Type Values

| Type | Description |
| ---- | ----------- |
| `ST` | Stock order |
| `PS` | Pre-order   |
| `RE` | Return      |

***

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

***

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

***

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

***

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

* `productUniqueId` + `productColorCode` for reordering
* `productImageUrl` for visual reference
* `trackTraceUrl` for shipment tracking
* `returnAllowed` for return functionality

**Status accuracy:**

* Keep `quantityDelivered` current
* Update `status` promptly on shipment
* Add `shipmentDate` when shipped
{% endtab %}

{% tab title="Performance" %}
* Use `storeFullHistoricalOrderSetForCustomer` for customer-level sync
* Limit history to 12-24 months to reduce data volume
* Batch multiple orders in single API calls
{% endtab %}
{% endtabs %}
