# Invoice Operations

Push invoice data to Colect so your customers can view their outstanding invoices and payment status in the app and B2B webstore.

{% hint style="success" %}
**Full Example:** [Download complete storeFullInvoiceSetForCustomer.xml](https://assets.apptitude.nl/KnowledgeBase/Webservice_3_0/External/storeFullInvoiceSetForCustomer.xml) - A comprehensive example showing all available invoice fields.
{% endhint %}

---

## Operations Overview

| Operation | Type | Description |
|-----------|------|-------------|
| [`storeInvoices`](#storeinvoices) | Partial Sync | Add or update invoices |
| [`storeFullInvoiceSetForCustomer`](#storefullinvoicesetforcustomer) | Full Sync | Replace all invoices for a customer |
| [`deleteInvoices`](#deleteinvoices) | Delete | Remove specific invoices |
| [`deleteAllInvoices`](#deleteallinvoices) | Delete | Clear all invoices |

---

## storeInvoices

Add new invoices or update existing ones. Invoices not included remain unchanged.

{% hint style="info" %}
Invoices are uniquely identified by `customerNo` + `invoiceNumber`. Sending an invoice with an existing combination will update that invoice.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeInvoices>
            <api:apiKey>your-api-key</api:apiKey>
            <api:invoice>
                <api:customerNo>CUST-001</api:customerNo>
                <api:invoiceNumber>INV-2025-00042</api:invoiceNumber>
                <api:date>2025-01-15T00:00:00</api:date>
                <api:dueDate>2025-02-14T00:00:00</api:dueDate>
                <api:currency>EUR</api:currency>
                <api:amountExcludingTax>1250.00</api:amountExcludingTax>
                <api:taxAmount>262.50</api:taxAmount>
                <api:outstandingAmount>1512.50</api:outstandingAmount>
                <api:status>OPEN</api:status>
                <api:externalUrl>https://your-erp.com/invoices/INV-2025-00042.pdf</api:externalUrl>
                <api:customField1>Order: ORD-2025-00123</api:customField1>
            </api:invoice>
            <api:invoice>
                <api:customerNo>CUST-001</api:customerNo>
                <api:invoiceNumber>INV-2025-00036</api:invoiceNumber>
                <api:date>2025-01-01T00:00:00</api:date>
                <api:dueDate>2025-01-31T00:00:00</api:dueDate>
                <api:currency>EUR</api:currency>
                <api:amountExcludingTax>800.00</api:amountExcludingTax>
                <api:taxAmount>168.00</api:taxAmount>
                <api:outstandingAmount>0</api:outstandingAmount>
                <api:status>PAID</api:status>
            </api:invoice>
        </api:storeInvoices>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOnN0b3JlSW52b2ljZXM+CiAgICAgICAgICAgIDxhcGk6YXBpS2V5PnlvdXItYXBpLWtleTwvYXBpOmFwaUtleT4KICAgICAgICAgICAgPGFwaTppbnZvaWNlPgogICAgICAgICAgICAgICAgPGFwaTpjdXN0b21lck5vPkNVU1QtMDAxPC9hcGk6Y3VzdG9tZXJObz4KICAgICAgICAgICAgICAgIDxhcGk6aW52b2ljZU51bWJlcj5JTlYtMjAyNS0wMDA0MjwvYXBpOmludm9pY2VOdW1iZXI+CiAgICAgICAgICAgICAgICA8YXBpOmRhdGU+MjAyNS0wMS0xNVQwMDowMDowMDwvYXBpOmRhdGU+CiAgICAgICAgICAgICAgICA8YXBpOmR1ZURhdGU+MjAyNS0wMi0xNFQwMDowMDowMDwvYXBpOmR1ZURhdGU+CiAgICAgICAgICAgICAgICA8YXBpOmN1cnJlbmN5PkVVUjwvYXBpOmN1cnJlbmN5PgogICAgICAgICAgICAgICAgPGFwaTphbW91bnRFeGNsdWRpbmdUYXg+MTI1MC4wMDwvYXBpOmFtb3VudEV4Y2x1ZGluZ1RheD4KICAgICAgICAgICAgICAgIDxhcGk6dGF4QW1vdW50PjI2Mi41MDwvYXBpOnRheEFtb3VudD4KICAgICAgICAgICAgICAgIDxhcGk6b3V0c3RhbmRpbmdBbW91bnQ+MTUxMi41MDwvYXBpOm91dHN0YW5kaW5nQW1vdW50PgogICAgICAgICAgICAgICAgPGFwaTpzdGF0dXM+T1BFTjwvYXBpOnN0YXR1cz4KICAgICAgICAgICAgICAgIDxhcGk6ZXh0ZXJuYWxVcmw+aHR0cHM6Ly95b3VyLWVycC5jb20vaW52b2ljZXMvSU5WLTIwMjUtMDAwNDIucGRmPC9hcGk6ZXh0ZXJuYWxVcmw+CiAgICAgICAgICAgICAgICA8YXBpOmN1c3RvbUZpZWxkMT5PcmRlcjogT1JELTIwMjUtMDAxMjM8L2FwaTpjdXN0b21GaWVsZDE+CiAgICAgICAgICAgIDwvYXBpOmludm9pY2U+CiAgICAgICAgICAgIDxhcGk6aW52b2ljZT4KICAgICAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJObz5DVVNULTAwMTwvYXBpOmN1c3RvbWVyTm8+CiAgICAgICAgICAgICAgICA8YXBpOmludm9pY2VOdW1iZXI+SU5WLTIwMjUtMDAwMzY8L2FwaTppbnZvaWNlTnVtYmVyPgogICAgICAgICAgICAgICAgPGFwaTpkYXRlPjIwMjUtMDEtMDFUMDA6MDA6MDA8L2FwaTpkYXRlPgogICAgICAgICAgICAgICAgPGFwaTpkdWVEYXRlPjIwMjUtMDEtMzFUMDA6MDA6MDA8L2FwaTpkdWVEYXRlPgogICAgICAgICAgICAgICAgPGFwaTpjdXJyZW5jeT5FVVI8L2FwaTpjdXJyZW5jeT4KICAgICAgICAgICAgICAgIDxhcGk6YW1vdW50RXhjbHVkaW5nVGF4PjgwMC4wMDwvYXBpOmFtb3VudEV4Y2x1ZGluZ1RheD4KICAgICAgICAgICAgICAgIDxhcGk6dGF4QW1vdW50PjE2OC4wMDwvYXBpOnRheEFtb3VudD4KICAgICAgICAgICAgICAgIDxhcGk6b3V0c3RhbmRpbmdBbW91bnQ+MDwvYXBpOm91dHN0YW5kaW5nQW1vdW50PgogICAgICAgICAgICAgICAgPGFwaTpzdGF0dXM+UEFJRDwvYXBpOnN0YXR1cz4KICAgICAgICAgICAgPC9hcGk6aW52b2ljZT4KICAgICAgICA8L2FwaTpzdG9yZUludm9pY2VzPgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)

### Response

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Body>
        <ns2:storeInvoicesResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
    </soapenv:Body>
</soapenv:Envelope>
```

---

## storeFullInvoiceSetForCustomer

Replace all invoices for a specific customer. Invoices not included will be **deleted** for that customer.

{% hint style="warning" %}
Only affects invoices for the specified customer. Other customers' invoices remain unchanged.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeFullInvoiceSetForCustomer>
            <api:apiKey>your-api-key</api:apiKey>
            <api:customerNo>CUST-001</api:customerNo>
            <api:invoice>
                <api:customerNo>CUST-001</api:customerNo>
                <api:invoiceNumber>INV-2025-00042</api:invoiceNumber>
                <api:date>2025-01-15T00:00:00</api:date>
                <api:dueDate>2025-02-14T00:00:00</api:dueDate>
                <api:currency>EUR</api:currency>
                <api:outstandingAmount>1512.50</api:outstandingAmount>
                <!-- ... -->
            </api:invoice>
            <api:invoice>
                <api:customerNo>CUST-001</api:customerNo>
                <api:invoiceNumber>INV-2025-00043</api:invoiceNumber>
                <!-- ... -->
            </api:invoice>
        </api:storeFullInvoiceSetForCustomer>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOnN0b3JlRnVsbEludm9pY2VTZXRGb3JDdXN0b21lcj4KICAgICAgICAgICAgPGFwaTphcGlLZXk+eW91ci1hcGkta2V5PC9hcGk6YXBpS2V5PgogICAgICAgICAgICA8YXBpOmN1c3RvbWVyTm8+Q1VTVC0wMDE8L2FwaTpjdXN0b21lck5vPgogICAgICAgICAgICA8YXBpOmludm9pY2U+CiAgICAgICAgICAgICAgICA8YXBpOmN1c3RvbWVyTm8+Q1VTVC0wMDE8L2FwaTpjdXN0b21lck5vPgogICAgICAgICAgICAgICAgPGFwaTppbnZvaWNlTnVtYmVyPklOVi0yMDI1LTAwMDQyPC9hcGk6aW52b2ljZU51bWJlcj4KICAgICAgICAgICAgICAgIDxhcGk6ZGF0ZT4yMDI1LTAxLTE1VDAwOjAwOjAwPC9hcGk6ZGF0ZT4KICAgICAgICAgICAgICAgIDxhcGk6ZHVlRGF0ZT4yMDI1LTAyLTE0VDAwOjAwOjAwPC9hcGk6ZHVlRGF0ZT4KICAgICAgICAgICAgICAgIDxhcGk6Y3VycmVuY3k+RVVSPC9hcGk6Y3VycmVuY3k+CiAgICAgICAgICAgICAgICA8YXBpOm91dHN0YW5kaW5nQW1vdW50PjE1MTIuNTA8L2FwaTpvdXRzdGFuZGluZ0Ftb3VudD4KICAgICAgICAgICAgPC9hcGk6aW52b2ljZT4KICAgICAgICAgICAgPGFwaTppbnZvaWNlPgogICAgICAgICAgICAgICAgPGFwaTpjdXN0b21lck5vPkNVU1QtMDAxPC9hcGk6Y3VzdG9tZXJObz4KICAgICAgICAgICAgICAgIDxhcGk6aW52b2ljZU51bWJlcj5JTlYtMjAyNS0wMDA0MzwvYXBpOmludm9pY2VOdW1iZXI+CiAgICAgICAgICAgICAgICA8YXBpOmRhdGU+MjAyNS0wMS0yMFQwMDowMDowMDwvYXBpOmRhdGU+CiAgICAgICAgICAgICAgICA8YXBpOmR1ZURhdGU+MjAyNS0wMi0xOVQwMDowMDowMDwvYXBpOmR1ZURhdGU+CiAgICAgICAgICAgICAgICA8YXBpOmN1cnJlbmN5PkVVUjwvYXBpOmN1cnJlbmN5PgogICAgICAgICAgICAgICAgPGFwaTpvdXRzdGFuZGluZ0Ftb3VudD44NTAuMDA8L2FwaTpvdXRzdGFuZGluZ0Ftb3VudD4KICAgICAgICAgICAgPC9hcGk6aW52b2ljZT4KICAgICAgICA8L2FwaTpzdG9yZUZ1bGxJbnZvaWNlU2V0Rm9yQ3VzdG9tZXI+CiAgICA8L3NvYXBlbnY6Qm9keT4KPC9zb2FwZW52OkVudmVsb3BlPg==)

---

## deleteInvoices

Remove specific invoices.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:deleteInvoices>
            <api:apiKey>your-api-key</api:apiKey>
            <api:invoice>
                <api:customerNo>CUST-001</api:customerNo>
                <api:invoiceNumber>INV-2025-00042</api:invoiceNumber>
                <api:date>2025-01-15T00:00:00</api:date>
                <api:currency>EUR</api:currency>
                <api:outstandingAmount>0</api:outstandingAmount>
            </api:invoice>
        </api:deleteInvoices>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOmRlbGV0ZUludm9pY2VzPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgICAgIDxhcGk6aW52b2ljZT4KICAgICAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJObz5DVVNULTAwMTwvYXBpOmN1c3RvbWVyTm8+CiAgICAgICAgICAgICAgICA8YXBpOmludm9pY2VOdW1iZXI+SU5WLTIwMjUtMDAwNDI8L2FwaTppbnZvaWNlTnVtYmVyPgogICAgICAgICAgICAgICAgPGFwaTpkYXRlPjIwMjUtMDEtMTVUMDA6MDA6MDA8L2FwaTpkYXRlPgogICAgICAgICAgICAgICAgPGFwaTpjdXJyZW5jeT5FVVI8L2FwaTpjdXJyZW5jeT4KICAgICAgICAgICAgICAgIDxhcGk6b3V0c3RhbmRpbmdBbW91bnQ+MDwvYXBpOm91dHN0YW5kaW5nQW1vdW50PgogICAgICAgICAgICA8L2FwaTppbnZvaWNlPgogICAgICAgIDwvYXBpOmRlbGV0ZUludm9pY2VzPgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)

---

## deleteAllInvoices

Remove all invoices from your collection.

{% hint style="danger" %}
**Warning:** This removes ALL invoices for ALL customers.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:deleteAllInvoices>
            <api:apiKey>your-api-key</api:apiKey>
        </api:deleteAllInvoices>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOmRlbGV0ZUFsbEludm9pY2VzPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgPC9hcGk6ZGVsZXRlQWxsSW52b2ljZXM+CiAgICA8L3NvYXBlbnY6Qm9keT4KPC9zb2FwZW52OkVudmVsb3BlPg==)

---

## Invoice Data Structure

### Mandatory Fields

| Field | Type | Description |
|-------|------|-------------|
| `customerNo` | String | Customer identifier |
| `invoiceNumber` | String | Unique invoice number |
| `date` | DateTime | Invoice date |
| `currency` | String | Currency code |
| `outstandingAmount` | Float | Amount still owed |

### Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `dueDate` | DateTime | Payment due date |
| `amountExcludingTax` | Float | Net amount before tax |
| `taxAmount` | Float | Tax amount |
| `status` | String | Status code (e.g., OPEN, PAID, OVERDUE) |
| `externalUrl` | String | Link to invoice PDF or detail page |
| `customField1` | String | Custom field for additional data |
| `customField2` | String | Custom field for additional data |
| `customField3` | String | Custom field for additional data |
| `customField4` | String | Custom field for additional data |

---

## Invoice Status

The `status` field is a free-text field. Common values:

| Status | Description |
|--------|-------------|
| `OPEN` | Invoice awaiting payment |
| `PAID` | Invoice fully paid |
| `OVERDUE` | Payment past due date |
| `PARTIAL` | Partially paid |
| `CREDIT` | Credit note |

{% hint style="info" %}
Use consistent status values across your invoices for proper display in the app.
{% endhint %}

---

## External URL

Provide a link to the full invoice document:

```xml
<api:invoice>
    <!-- ... -->
    <api:externalUrl>https://your-portal.com/invoices/INV-2025-00042.pdf</api:externalUrl>
</api:invoice>
```

Customers can click this link to view or download the complete invoice.

---

## Custom Fields

Use custom fields for additional information:

```xml
<api:invoice>
    <!-- ... -->
    <api:customField1>Order Reference: ORD-2025-00123</api:customField1>
    <api:customField2>Payment Terms: Net 30</api:customField2>
    <api:customField3>Sales Rep: John Smith</api:customField3>
    <api:customField4>Region: EMEA</api:customField4>
</api:invoice>
```

---

## Best Practices

{% tabs %}
{% tab title="Sync Strategy" %}
```
Recommended sync schedule:
├── Daily: Full sync for each customer with changes
├── Immediately: Update outstandingAmount on payments
└── Monthly: Full sync of all customers (cleanup)

Use storeFullInvoiceSetForCustomer when:
├── Customer makes a payment
├── New invoice is created
└── Invoice status changes
```
{% endtab %}

{% tab title="Outstanding Amount" %}
Always update `outstandingAmount` when:
- Payment is received (partial or full)
- Invoice is credited
- Payment is reversed

```xml
<!-- Fully paid invoice -->
<api:invoice>
    <api:customerNo>CUST-001</api:customerNo>
    <api:invoiceNumber>INV-2025-00042</api:invoiceNumber>
    <!-- ... -->
    <api:outstandingAmount>0</api:outstandingAmount>
    <api:status>PAID</api:status>
</api:invoice>

<!-- Partially paid invoice -->
<api:invoice>
    <api:customerNo>CUST-001</api:customerNo>
    <api:invoiceNumber>INV-2025-00042</api:invoiceNumber>
    <!-- ... -->
    <api:outstandingAmount>512.50</api:outstandingAmount>
    <api:status>PARTIAL</api:status>
</api:invoice>
```
{% endtab %}

{% tab title="Data Quality" %}
- Include `dueDate` to help customers prioritize payments
- Use meaningful `status` values
- Provide `externalUrl` for full invoice access
- Keep invoice data synchronized with your ERP
{% endtab %}
{% endtabs %}
