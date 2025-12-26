# Customer Operations

Manage your B2B customer accounts in Colect. Customers represent the retail stores, boutiques, and distributors who can place orders through your Colect app or B2B webstore.

---

## Operations Overview

| Operation | Type | Description |
|-----------|------|-------------|
| [`storeCustomers`](#storecustomers) | Partial Sync | Add or update specific customers |
| [`storeFullCustomerSet`](#storefullcustomerset) | Full Sync | Replace entire customer base |
| [`deleteCustomers`](#deletecustomers) | Delete | Remove specific customers |
| [`deleteAllCustomers`](#deleteallcustomers) | Delete | Clear all customers |

---

## storeCustomers

Add new customers or update existing ones. Customers not included in the request remain unchanged.

{% hint style="info" %}
Customers are uniquely identified by `customerNo`. Sending a customer with an existing number will update that customer.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeCustomers>
            <api:apiKey>your-api-key</api:apiKey>
            <api:customer>
                <!-- Mandatory fields -->
                <api:customerNo>CUST-001</api:customerNo>
                <api:name>Fashion Boutique Amsterdam</api:name>
                <api:email>orders@fashionboutique.nl</api:email>
                <api:currencyCode>EUR</api:currencyCode>
                <!-- Address -->
                <api:street>Kalverstraat</api:street>
                <api:houseNumber>123</api:houseNumber>
                <api:postalCode>1012 PA</api:postalCode>
                <api:city>Amsterdam</api:city>
                <api:country>NL</api:country>
                <api:phone>+31 20 123 4567</api:phone>
                <!-- Pricing configuration -->
                <api:activePriceGroup>RETAIL</api:activePriceGroup>
                <api:status>ACTIVE</api:status>
                <api:language>nl</api:language>
                <api:channel>Boutique</api:channel>
                <!-- Shipping locations -->
                <api:shippingLocation>
                    <api:code>MAIN</api:code>
                    <api:name>Main Store</api:name>
                    <api:defaultLocation>true</api:defaultLocation>
                    <api:street>Kalverstraat</api:street>
                    <api:houseNumber>123</api:houseNumber>
                    <api:postalCode>1012 PA</api:postalCode>
                    <api:city>Amsterdam</api:city>
                    <api:country>NL</api:country>
                </api:shippingLocation>
                <api:shippingLocation>
                    <api:code>WAREHOUSE</api:code>
                    <api:name>Warehouse</api:name>
                    <api:defaultLocation>false</api:defaultLocation>
                    <api:street>Industrieweg</api:street>
                    <api:houseNumber>45</api:houseNumber>
                    <api:postalCode>1099 AB</api:postalCode>
                    <api:city>Amsterdam</api:city>
                    <api:country>NL</api:country>
                </api:shippingLocation>
                <!-- Contacts -->
                <api:contact>
                    <api:code>OWNER</api:code>
                    <api:firstName>Jan</api:firstName>
                    <api:lastName>de Vries</api:lastName>
                    <api:email>jan@fashionboutique.nl</api:email>
                    <api:phone>+31 6 12345678</api:phone>
                    <api:role>Owner</api:role>
                    <api:accessType>FULL</api:accessType>
                </api:contact>
                <!-- Discount configuration -->
                <api:discountGroup>
                    <api:code>SEASONAL</api:code>
                    <api:percentage>10</api:percentage>
                    <api:description>Seasonal Discount</api:description>
                </api:discountGroup>
                <!-- Order settings -->
                <api:minOrderValue>250.00</api:minOrderValue>
                <api:maxOrderValue>50000.00</api:maxOrderValue>
                <api:orderFooter>Thank you for your order!</api:orderFooter>
            </api:customer>
        </api:storeCustomers>
    </soapenv:Body>
</soapenv:Envelope>
```

### Response

```xml
<ns2:storeCustomersResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
```

---

## storeFullCustomerSet

Replace your entire customer base. Customers not included in this request will be **deleted**.

{% hint style="warning" %}
**Caution:** This is a destructive operation. All existing customers not in the request will be removed.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeFullCustomerSet>
            <api:apiKey>your-api-key</api:apiKey>
            <api:customer>
                <api:customerNo>CUST-001</api:customerNo>
                <api:name>Fashion Boutique Amsterdam</api:name>
                <api:email>orders@fashionboutique.nl</api:email>
                <api:currencyCode>EUR</api:currencyCode>
                <!-- ... full customer data ... -->
            </api:customer>
            <api:customer>
                <api:customerNo>CUST-002</api:customerNo>
                <api:name>Style Corner Berlin</api:name>
                <api:email>info@stylecorner.de</api:email>
                <api:currencyCode>EUR</api:currencyCode>
                <!-- ... full customer data ... -->
            </api:customer>
            <!-- ... all other customers ... -->
        </api:storeFullCustomerSet>
    </soapenv:Body>
</soapenv:Envelope>
```

---

## deleteCustomers

Remove specific customers from your collection.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:deleteCustomers>
            <api:apiKey>your-api-key</api:apiKey>
            <api:customer>
                <api:customerNo>CUST-001</api:customerNo>
                <api:name>Fashion Boutique Amsterdam</api:name>
                <api:email>orders@fashionboutique.nl</api:email>
                <api:currencyCode>EUR</api:currencyCode>
            </api:customer>
        </api:deleteCustomers>
    </soapenv:Body>
</soapenv:Envelope>
```

---

## deleteAllCustomers

Remove all customers from your collection.

{% hint style="danger" %}
**Warning:** This removes ALL customers. Use with extreme caution.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:deleteAllCustomers>
            <api:apiKey>your-api-key</api:apiKey>
        </api:deleteAllCustomers>
    </soapenv:Body>
</soapenv:Envelope>
```

---

## Customer Data Structure

### Mandatory Fields

| Field | Type | Description |
|-------|------|-------------|
| `customerNo` | String | Unique customer identifier (from your ERP) |
| `name` | String | Customer/company name |
| `email` | String | Primary email address |
| `currencyCode` | String | Currency code (EUR, USD, GBP, etc.) |

### Address Fields

| Field | Type | Description |
|-------|------|-------------|
| `address` | String | Full address (use only when structured fields unavailable) |
| `street` | String | Street name |
| `houseNumber` | String | House/building number |
| `houseNumberSuffix` | String | Suffix (apartment, suite, etc.) |
| `postalCode` | String | Postal/ZIP code |
| `city` | String | City name |
| `country` | String | Country name |
| `phone` | String | Phone number |

### Pricing Configuration

| Field | Type | Description |
|-------|------|-------------|
| `activePriceGroup` | String | Current price group |
| `nextPriceGroup` | String | Upcoming price group |
| `nextPriceGroupDate` | DateTime | Date when next price group becomes active |
| `retailCurrencyCode` | String | Currency for displaying retail prices |

### Status Options

| Status | Description |
|--------|-------------|
| `ACTIVE` | Normal active customer (default) |
| `WARNING` | Shows `statusMessage` in a popup when selected |
| `BLOCKED` | Cannot be selected, no orders allowed |
| `BLOCKED_FOR_ORDERING` | Can be selected but cannot place orders |

```xml
<api:customer>
    <api:customerNo>CUST-003</api:customerNo>
    <!-- ... -->
    <api:status>WARNING</api:status>
    <api:statusMessage>Payment overdue - please contact accounting</api:statusMessage>
</api:customer>
```

---

## Shipping Locations

Customers can have multiple shipping addresses:

```xml
<api:shippingLocation>
    <api:code>STORE-001</api:code>
    <api:name>Downtown Store</api:name>
    <api:defaultLocation>true</api:defaultLocation>
    <api:street>Main Street</api:street>
    <api:houseNumber>100</api:houseNumber>
    <api:postalCode>10001</api:postalCode>
    <api:city>New York</api:city>
    <api:country>US</api:country>
    <api:email>downtown@example.com</api:email>
    <api:telephone>+1 212 555 0100</api:telephone>
    <api:contactFirstName>John</api:contactFirstName>
    <api:contactLastName>Smith</api:contactLastName>
    <api:externalReference>ERP-LOC-001</api:externalReference>
</api:shippingLocation>
```

---

## Contacts

Add multiple contacts per customer for order notifications:

```xml
<api:contact>
    <api:code>BUYER</api:code>
    <api:firstName>Sarah</api:firstName>
    <api:lastName>Johnson</api:lastName>
    <api:email>sarah@customer.com</api:email>
    <api:phone>+1 555 123 4567</api:phone>
    <api:role>Head Buyer</api:role>
    <api:accessType>FULL</api:accessType>
</api:contact>
```

| Access Type | Description |
|-------------|-------------|
| `FULL` | Full access to B2B webshop |
| `NONE` | No access to B2B webshop |

---

## Discount Groups

Apply automatic discounts to customer orders:

```xml
<api:discountGroup>
    <api:code>VIP</api:code>
    <api:percentage>15</api:percentage>
    <api:minimumQuantity>10</api:minimumQuantity>
    <api:startDate>2025-01-01T00:00:00</api:startDate>
    <api:endDate>2025-12-31T23:59:59</api:endDate>
    <api:description>VIP Customer Discount</api:description>
    <api:identifier>VIP-2025</api:identifier>
</api:discountGroup>
```

{% hint style="info" %}
The `identifier` is returned on orders so you can track which discount was applied.
{% endhint %}

---

## Margin Groups

Calculate wholesale prices based on retail prices:

```xml
<api:marginGroup>
    <api:code>*</api:code>
    <api:amount>2.3</api:amount>
</api:marginGroup>
```

This means: `wholesalePrice = retailPrice / 2.3`

---

## Agreements

Customer-specific agreements with pricing and terms:

```xml
<api:agreement>
    <api:code>AGR-2025-001</api:code>
    <api:description>2025 Season Agreement</api:description>
    <api:channel>Wholesale</api:channel>
    <api:activePriceGroup>VIP</api:activePriceGroup>
    <api:currencyCode>EUR</api:currencyCode>
    <api:orderFooter>Terms: Net 30</api:orderFooter>
    <api:discountGroup>
        <api:code>AGR</api:code>
        <api:percentage>5</api:percentage>
    </api:discountGroup>
</api:agreement>
```

---

## Order Rules

### Minimum/Maximum Order Value

```xml
<api:customer>
    <!-- ... -->
    <api:minOrderValue>500.00</api:minOrderValue>
    <api:maxOrderValue>25000.00</api:maxOrderValue>
</api:customer>
```

### Order Discount Rules

Apply automatic discounts based on order amount:

```xml
<api:orderDiscountRule>
    <api:identification>BULK-DISCOUNT</api:identification>
    <api:description>10% off orders over €1000</api:description>
    <api:minimumOrderAmount>1000.00</api:minimumOrderAmount>
    <api:discountPercentage>10</api:discountPercentage>
    <api:exclusive>false</api:exclusive>
    <api:evaluationMethod>EVALUATE_BASED_ON_TODAY</api:evaluationMethod>
</api:orderDiscountRule>
```

### Delivery Cost Rules

Apply shipping charges based on order amount:

```xml
<api:orderDeliveryCostRule>
    <api:identification>SHIPPING</api:identification>
    <api:description>Free shipping over €500</api:description>
    <api:minimumOrderAmount>0</api:minimumOrderAmount>
    <api:amount>15.00</api:amount>
    <api:exclusive>true</api:exclusive>
    <api:evaluationMethod>EVALUATE_BASED_ON_TODAY</api:evaluationMethod>
</api:orderDeliveryCostRule>
<api:orderDeliveryCostRule>
    <api:identification>FREE-SHIPPING</api:identification>
    <api:description>Free shipping</api:description>
    <api:minimumOrderAmount>500.00</api:minimumOrderAmount>
    <api:amount>0</api:amount>
    <api:exclusive>true</api:exclusive>
    <api:evaluationMethod>EVALUATE_BASED_ON_TODAY</api:evaluationMethod>
</api:orderDeliveryCostRule>
```

---

## Payment Status

| Status | Description |
|--------|-------------|
| `PAYMENT_AFTERWARDS` | Standard payment terms (default) |
| `PREPAYMENT` | Payment required before order processing |
| `PREPAYMENT_REORDER_ONLY` | Prepayment only for reorders |

```xml
<api:customer>
    <!-- ... -->
    <api:paymentStatus>PREPAYMENT</api:paymentStatus>
    <api:taxPercentage>21</api:taxPercentage>
</api:customer>
```

---

## Best Practices

{% tabs %}
{% tab title="Data Quality" %}
- Always provide complete address data with structured fields
- Use consistent `customerNo` from your ERP
- Keep email addresses up to date for order confirmations
- Set appropriate `status` for customers with payment issues
{% endtab %}

{% tab title="Performance" %}
- Use `storeFullCustomerSet` for nightly sync
- Use `storeCustomers` for real-time updates during the day
- Batch multiple customer updates in single requests
{% endtab %}

{% tab title="Pricing" %}
- Define clear price groups in your catalog
- Use `activePriceGroup` to assign customers to price tiers
- Use `agreements` for complex customer-specific pricing
{% endtab %}
{% endtabs %}
