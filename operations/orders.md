# Order Operations

Retrieve orders placed by customers through the Colect app or B2B webstore. Orders flow from Colect to your ERP system for fulfillment.

{% hint style="success" %}
**Full Example:** [Download complete order output example](https://assets.apptitude.nl/KnowledgeBase/Webservice_3_0/External/OrderOutput_WS.xml) - Shows the complete order structure returned by getUnprocessedOrders.
{% endhint %}

---

## Operations Overview

| Operation | Description |
|-----------|-------------|
| [`getUnprocessedOrders`](#getunprocessedorders) | Get orders ready for ERP processing |
| [`getOrders`](#getorders) | Get specific orders by order number |
| [`markOrdersAsProcessed`](#markordersasprocessed) | Mark orders as processed in your ERP |

---

## Order Flow

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Customer  │     │   Colect    │     │  Connector  │     │  Your ERP   │
│  Places     │────►│   Cloud     │────►│   (API)     │────►│   System    │
│  Order      │     │   Cache     │     │             │     │             │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
                                               │
                                               ▼
                                    getUnprocessedOrders
                                               │
                                               ▼
                                    markOrdersAsProcessed
```

---

## getUnprocessedOrders

Retrieve all orders that haven't been marked as processed yet. This is your primary polling endpoint for new orders.

{% hint style="success" %}
**Recommended:** Poll this endpoint every 5-15 minutes to receive new orders promptly.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:getUnprocessedOrders>
            <api:apiKey>your-api-key</api:apiKey>
        </api:getUnprocessedOrders>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNfERJJoAS1qNmbVjoGdOAkBAwwLUMdCgNtDAK4ATGALgZVAPgBQOIE4oHKUSAASMBDuEqz+QcGiYQBCYK4AnvEJQcA6SFIwDACqcFASYJzWIDCuAPISMSBZ2dm5UNpa7QDSMJnpYM4SALQ6QwDWvcBG7Z3aPZkBLcHTHQXFpeWVINV1DTASTYuBU0mISKkZ-iehZ2gISrA+QA)

### Response

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Body>
        <ns2:getUnprocessedOrdersResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/">
            <ns2:return>
                <ns2:collectionId>your-collection-id</ns2:collectionId>
                <ns2:orderNumber>ABC123DEF456</ns2:orderNumber>
                <ns2:trackingNumber>TRK789012345</ns2:trackingNumber>
                <ns2:timestamp>2025-01-15T14:30:00</ns2:timestamp>
                <ns2:serverTimestamp>2025-01-15T14:30:05</ns2:serverTimestamp>
                <ns2:signed>true</ns2:signed>
                <ns2:salesPerson>sales@yourcompany.com</ns2:salesPerson>
                <ns2:customerNo>CUST-001</ns2:customerNo>
                <ns2:customerPriceGroup>RETAIL</ns2:customerPriceGroup>
                <ns2:currency>EUR</ns2:currency>
                <ns2:signee>John Buyer</ns2:signee>
                <ns2:comment>Please ship ASAP</ns2:comment>
                <ns2:status>AT_CONNECTOR</ns2:status>
                <ns2:customerOrderReference>PO-2025-0042</ns2:customerOrderReference>
                <ns2:requestedDeliveryDate>2025-02-01T00:00:00</ns2:requestedDeliveryDate>
                <ns2:totalCount>25</ns2:totalCount>
                <ns2:totalAmount>625.00</ns2:totalAmount>
                <ns2:internalOrderType>ORDER</ns2:internalOrderType>

                <ns2:shippingLocation>
                    <ns2:code>MAIN</ns2:code>
                    <ns2:name>Main Store</ns2:name>
                    <ns2:street>Kalverstraat</ns2:street>
                    <ns2:houseNumber>123</ns2:houseNumber>
                    <ns2:postalCode>1012 PA</ns2:postalCode>
                    <ns2:city>Amsterdam</ns2:city>
                    <ns2:country>Netherlands</ns2:country>
                </ns2:shippingLocation>

                <ns2:orderLine>
                    <ns2:type>STOCK_ORDER</ns2:type>
                    <ns2:productUniqueId>STYLE-001</ns2:productUniqueId>
                    <ns2:productColorCode>BLK</ns2:productColorCode>
                    <ns2:sizeName>M</ns2:sizeName>
                    <ns2:eanCode>8712345678902</ns2:eanCode>
                    <ns2:numberOfPieces>10</ns2:numberOfPieces>
                    <ns2:retailPrice>49.95</ns2:retailPrice>
                    <ns2:wholesalePrice>25.00</ns2:wholesalePrice>
                    <ns2:netWholesalePrice>25.00</ns2:netWholesalePrice>
                    <ns2:lineAmount>250.00</ns2:lineAmount>
                    <ns2:prePackUnitCount>1</ns2:prePackUnitCount>
                    <ns2:deliveryWindowCode>SS25-DROP1</ns2:deliveryWindowCode>
                </ns2:orderLine>
                <ns2:orderLine>
                    <ns2:type>STOCK_ORDER</ns2:type>
                    <ns2:productUniqueId>STYLE-001</ns2:productUniqueId>
                    <ns2:productColorCode>BLK</ns2:productColorCode>
                    <ns2:sizeName>L</ns2:sizeName>
                    <ns2:numberOfPieces>15</ns2:numberOfPieces>
                    <ns2:wholesalePrice>25.00</ns2:wholesalePrice>
                    <ns2:lineAmount>375.00</ns2:lineAmount>
                    <!-- ... -->
                </ns2:orderLine>

                <ns2:orderConfirmation>
                    <ns2:orderNumber>ABC123DEF456</ns2:orderNumber>
                    <ns2:pdfData>JVBERi0xLjQKJ...</ns2:pdfData>
                </ns2:orderConfirmation>
            </ns2:return>
        </ns2:getUnprocessedOrdersResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

---

## getOrders

Retrieve specific orders by their order number. Useful for re-fetching orders or verifying order details.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:getOrders>
            <api:apiKey>your-api-key</api:apiKey>
            <api:orderNumber>ABC123DEF456</api:orderNumber>
            <api:orderNumber>XYZ789GHI012</api:orderNumber>
        </api:getOrders>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNfERJJoAS1qNmbVjoGdOAkBAwwLUMdCgNtDAK4ATGALgZVAPgBQOIE4oHKUSAASMBDuEqz+QcGiYQBCYK4AnvEJQcA6SFIwDADyEjEgWdnZuVDaWjUA0jCZ6WDOEgC0Ou0A1k3ARjV12o2ZAZVVeZIxAHLOBABGMBI+AILJAMIAjABMAMwAIigAYgAsAKwAbP2TpUuzC0sV44HVtVN3c4vLABoAmgBaAHYABwATgA4uEAJIABh210G7wk9y+TwSCNqBWKtwk5TGL1kFEQSFSGX8-SSxLQCCUsB8QA)

### Response

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Body>
        <ns2:getOrdersResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/">
            <ns2:return>
                <ns2:orderNumber>ABC123DEF456</ns2:orderNumber>
                <!-- ... full order data ... -->
            </ns2:return>
            <ns2:return>
                <ns2:orderNumber>XYZ789GHI012</ns2:orderNumber>
                <!-- ... full order data ... -->
            </ns2:return>
        </ns2:getOrdersResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

---

## markOrdersAsProcessed

Mark orders as processed after successfully importing them into your ERP. Once marked, orders won't appear in `getUnprocessedOrders`.

{% hint style="warning" %}
Only mark orders as processed **after** they have been successfully imported into your ERP system.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:markOrdersAsProcessed>
            <api:apiKey>your-api-key</api:apiKey>
            <api:orderNumber>ABC123DEF456</api:orderNumber>
            <api:orderNumber>XYZ789GHI012</api:orderNumber>
        </api:markOrdersAsProcessed>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNfERJJoAS1qNmbVjoGdOAkBAwwLUMdCgNtDAK4ATGALgZVAPgBQOIE4oHKUSAASMBDuEqz+QcGiYQBCYK4AnvEJQcA6SHwSANYA8hIxIACCIAAKEmCc1iAwrlnZ2blQ2lqdANIwmelgzhIAtDojhf3ARp3d2n2ZAW3teZIxAHLOBABGMBI+FckAwgCMAEwAzAAiKABiACwArABs06tle5s7e63LgR1dNafLa7fYADQAmgAtADsAA4AJwAcXCAEkAAznN6zIESL6g34JbFdAolD4SSo1OoNEBNFpLf6yCiIJCpDL+aZJFloBBKWA+IA)

### Response

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Body>
        <ns2:markOrdersAsProcessedResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
    </soapenv:Body>
</soapenv:Envelope>
```

---

## Order Status Values

Orders progress through various statuses:

| Status | Description |
|--------|-------------|
| `PROCESSING` | Order is being processed |
| `COOLING` | Order in cooling period (cancellation window) |
| `AT_CACHE` | Order stored in Colect cloud |
| `AT_CONNECTOR` | Order available for retrieval via API |
| `AT_DESTINATION` | Order marked as processed |
| `NO_DESTINATION` | No connector configured |
| `DUPLICATE` | Duplicate order detected |
| `DRAFT` | Draft order (not submitted) |
| `TO_BE_EDITED` | Order needs editing |
| `TO_BE_APPROVED` | Awaiting approval |
| `APPROVAL_DECLINED` | Approval rejected |
| `WAITING_FOR_PAYMENT` | Awaiting prepayment |
| `PAYMENT_FAILED` | Payment failed |

{% hint style="info" %}
Status values ending in `_MOVED` indicate the order has been moved/processed but retained for reference.
{% endhint %}

---

## Order Line Types

| Type | Description |
|------|-------------|
| `STOCK_ORDER` | Regular stock order (default) |
| `PRE_ORDER` | Pre-order (not yet in stock) |
| `RETURN_ORDER` | Return/credit order |

---

## Order Confirmation PDF

Orders include a PDF confirmation accessible via `orderConfirmation`:

```xml
<ns2:orderConfirmation>
    <ns2:orderNumber>ABC123DEF456</ns2:orderNumber>
    <ns2:pdfData>JVBERi0xLjQKJeLjz9MKMSAwIG9iago8PC9UeXBl...</ns2:pdfData>
</ns2:orderConfirmation>
```

The `pdfData` field contains Base64-encoded PDF data.

{% tabs %}
{% tab title="Decode in Python" %}
```python
import base64

pdf_data = order.orderConfirmation.pdfData
pdf_bytes = base64.b64decode(pdf_data)

with open(f"order_{order.orderNumber}.pdf", "wb") as f:
    f.write(pdf_bytes)
```
{% endtab %}

{% tab title="Decode in C#" %}
```csharp
byte[] pdfBytes = Convert.FromBase64String(order.OrderConfirmation.PdfData);
File.WriteAllBytes($"order_{order.OrderNumber}.pdf", pdfBytes);
```
{% endtab %}
{% endtabs %}

---

## Order Data Reference

### Order Fields

| Field | Type | Description |
|-------|------|-------------|
| `orderNumber` | String | Unique 12-character order number |
| `trackingNumber` | String | Same for all operations in transaction |
| `canceledOrderNumber` | String | Reference to original if this is a cancellation |
| `timestamp` | DateTime | When order was created on device |
| `serverTimestamp` | DateTime | When order reached server |
| `signed` | Boolean | Whether order was signed |
| `salesPerson` | String | Email of salesperson |
| `customerNo` | String | Customer identifier |
| `customerPriceGroup` | String | Customer's price group |
| `currency` | String | Order currency |
| `signee` | String | Name of person who signed |
| `comment` | String | Customer comment |
| `internalComment` | String | Internal comment (not on confirmation) |
| `customerOrderReference` | String | Customer's PO number |
| `requestedDeliveryDate` | DateTime | Requested delivery date |
| `totalCount` | Integer | Total quantity ordered |
| `totalAmount` | Float | Total order value |
| `erpOrderReference` | String | For storing ERP reference |
| `internalOrderType` | Enum | `ORDER` or `RETURN` |

### Order Line Fields

| Field | Type | Description |
|-------|------|-------------|
| `type` | Enum | `STOCK_ORDER`, `PRE_ORDER`, or `RETURN_ORDER` |
| `productUniqueId` | String | Product identifier |
| `productColorCode` | String | Color code |
| `sizeName` | String | Size name |
| `subSize` | String | Sub-size (e.g., width for jeans) |
| `eanCode` | String | EAN/barcode |
| `numberOfPieces` | Integer | Quantity ordered |
| `retailPrice` | Float | Retail price |
| `wholesalePrice` | Float | Wholesale price with customer discount |
| `grossWholesalePrice` | Float | Price before all discounts |
| `netWholesalePrice` | Float | Price after all discounts |
| `lineAmount` | Float | Total line value |
| `discountPercentage` | Float | Manual discount applied |
| `deliveryWindowCode` | String | Selected delivery window |
| `deliveryDate` | DateTime | Line-level delivery date |
| `margin` | Float | Manual margin applied |
| `marginGroupCode` | String | Applied margin group |
| `discountGroupCode` | String | Applied discount group |
| `returnReason` | String | Reason code for returns |
| `remark` | String | Line-level comment |

---

## Integration Best Practices

{% tabs %}
{% tab title="Polling Strategy" %}
```
Recommended polling schedule:

During business hours (8:00-18:00):
└── Every 5 minutes

After hours:
└── Every 30 minutes

Process flow:
1. getUnprocessedOrders
2. Import each order to ERP
3. markOrdersAsProcessed (only successful imports)
4. Retry failed imports next cycle
```
{% endtab %}

{% tab title="Error Handling" %}
```
If ERP import fails:
├── Log the error with full order data
├── Do NOT call markOrdersAsProcessed
├── Retry on next polling cycle
└── Alert operations if order fails 3+ times

If order is duplicate:
├── Check if already in ERP by orderNumber
├── If exists, mark as processed
└── If not, import normally
```
{% endtab %}

{% tab title="Order Cancellations" %}
Orders can be canceled by sending a new order that references the original:

```xml
<ns2:return>
    <ns2:orderNumber>NEW123ORDER</ns2:orderNumber>
    <ns2:canceledOrderNumber>ABC123DEF456</ns2:canceledOrderNumber>
    <ns2:internalOrderType>ORDER</ns2:internalOrderType>
    <!-- Order lines with negative quantities or RETURN_ORDER type -->
</ns2:return>
```

Handle cancellations in your ERP:
1. Look up original order by `canceledOrderNumber`
2. Process as credit/cancellation in your ERP
3. Mark new order as processed
{% endtab %}
{% endtabs %}
