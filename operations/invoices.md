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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgzIGSRgASUQwfU57XLy84AKDAGkYHKywdwkAWgNOgGsW4DMofT1h5pzg2sT64aR9csqYGqnamZHOdxAighgJADkwAIBhAFUAZQAVToAGa4BGQYKNrbAd-cPJlbqC+YQKqr27gIACNdgESnsAGqdABM1xhAFYbrcACwwx6zX7-GCAkFgz5ffKzTwQBhLOGIm53Tp3BEXW5IBm3DEjElk5aEzRrJCedwwAAipPJ8KR8JpKPp10ZUuZQ1ZfMF7IJnO5GwkEngnByKBOACUWUg1Rq4FqOSqGgR2nAGCg8JwMF55lILhA8AE7ojrgJZRarTa7Q7PE6XW7lYTuQxXQBBS3ua0BGEANhhAgR1wNkbwMb9ZvDBXaDC2EDgQbgUmzcYY7oRHtT6blSALRZLTor8bDX25RY8IACAHkAAooPYG7ubXOdgowPBkiRwWwnCQYALGKCkdhtDqdXZiTivVhYxYgVgQ6EU0WolNQTwAMwN09n84wi+XHZWqs22wAYvpMJ47v2EixEgOB9rq-KwiKyIegAzAazzfr+GD-hO0wNoeVSoUSIwYUsb5TB+LxvAcxznFctwPA2CGvLsJH4asPwLACQKghI4JQpBlK3NcMGJgauG4qxWEMcSQoJlB9xUpK0rSgabJ4Zy3zEgqYnnlSnQwXc0lMvWBS8gKYn0Up6wdMapo6vqVGmZqEyKdMvqVra9qOmWIYBAAHLc3q6bMECxtaTmBsGrrCcZSCZm2VZ3Im7neRm0b+VWRn2bMTaRi2ZaRQEPkjGlxaluWiWhSlIxjr2A5RiU-KjpGPahfxTEKQRDYvBqZR-EeyyDKI4QZNkQTdWEqRoAgSiwAEQA)

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgzIGSRgAMXcMDABJRDB9ThgAZRgGEskAYXcQIoIYCVy8vOACgwBpGByssHcJAFoDGYBrceAzKH09NbGc4IHEobWkTk7u3oA5MAC2gFUGgBUZgAYHgEYVgqOusB6Jc-7dzX2630NTqMD+-z272OXzOF2ud0eLzeBw+Jx+Fx2EMhB2BCFq9VO7gIACNegFKqcAGozABMDxpAFZEQ8ACw05FAkEEomkvqYrEAgqeCAMMF0xmPZ4zZ4M25PJDyp4cpDC0XggWAlXuGAAERFYvpTPp0pZcoeCvNStW6082r1av5GqhEgk8E4ORQVwASsqji63dsBYMClMGF0IHBPMCpABBAhTOAMAIy540gQMh7K0PhyPRuMJpOOrHAACEMxmOAEVZw5fVA2VuPxYKLwZxXObQex61RMPRlxu9yer2th2h31+Lf+msboMJJLJFOp4qNTxZAGYG+257y6xDS+XK9Xa5OQiOZ-Vd8rPq6yhVqnjQU0Wu0x2TMStROEMtkgh+wqk0AQJRYACIA)

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgyRvOwYYAElEMH1Oe1y8vOACgwBpGByssHcJAFoDLoBrVuAzKH09EZac4LrEhpGkfQqqmFrputnRzncQBjACGAkAOTAAgGEAVQBlABUugAZbgEYhgs3t3f2jldWZgoWESuqB3cBAARvsAqUDgA1LoAJlusIArHd7gAWWHPOZ-AEwIGg8FTb4-OaeCAlALwpF3B5dB6Iq73JCM+6Y0ak8mEomadZITYSCTwTg5FBnABKrN5nQFcCFXy5PI6DG2EDgngWUgAggQOnAGAFbhLFcrVeqtTq9ZzphLsUs5dzhmzMDASuV-ksQCshqJwhlskEvWFUmgEEpYAEgA)

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgyRvOwYYAEEMDABJRDB9TntcvLzgAoMAaRgcrLB3CQBaAz6Aa07gMyh9PQmOnOCmsYKimBLyqpq6hrmk2RTkDOygsdFwtAQlWACgA)

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
