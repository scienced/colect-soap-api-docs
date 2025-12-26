# Colect SOAP API Documentation

## Version 3.0

Welcome to the Colect SOAP API documentation. This API enables fashion brands to synchronize their ERP systems with the Colect platform for seamless B2B e-commerce operations.

---

## Overview

The Colect API provides a comprehensive SOAP-based interface for:

| Domain | Description |
|--------|-------------|
| **Products** | Sync your product catalog including sizes, colors, prices, and stock levels |
| **Customers** | Manage B2B customer accounts with pricing groups and access controls |
| **Orders** | Retrieve orders placed through Colect apps and webstores |
| **Invoices** | Push invoice data for customer visibility |
| **Access Rules** | Control which products are visible to which customers |

---

## Quick Start

{% tabs %}
{% tab title="Endpoint" %}
```
https://connector.colect.services:443/services/api/3.0
```
{% endtab %}

{% tab title="WSDL" %}
```
https://connector.colect.services:443/services/api/3.0?wsdl
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
All API operations require an `apiKey` parameter. Contact Colect support to obtain your collection-specific API key.
{% endhint %}

---

## Data Flow Architecture

```
                    ┌─────────────────────────────────┐
                    │         Colect Platform         │
                    │  (Sales App & B2B Webstore)     │
                    └─────────────────────────────────┘
                                    ▲
                                    │
                    ┌───────────────┴───────────────┐
                    │          SOAP API             │
                    └───────────────┬───────────────┘
                                    │
            ┌───────────────────────┼───────────────────────┐
            │                       │                       │
            ▼                       │                       ▼
    ┌───────────────┐               │               ┌───────────────┐
    │   PUSH DATA   │               │               │   PULL DATA   │
    │───────────────│               │               │───────────────│
    │ • Products    │               │               │ • Orders      │
    │ • Customers   │               │               │               │
    │ • Stock       │               │               │               │
    │ • Prices      │               │               │               │
    │ • Invoices    │               │               │               │
    │ • Access Rules│               │               │               │
    └───────────────┘               │               └───────────────┘
            │                       │                       ▲
            │                       │                       │
            └───────────────────────┼───────────────────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    │        Your ERP System        │
                    └───────────────────────────────┘
```

| Direction | Data | Description |
|-----------|------|-------------|
| **ERP → Colect** | Products, Customers, Stock, Prices, Invoices, Access Rules | You push master data to Colect |
| **Colect → ERP** | Orders | You pull orders placed by customers and sales reps |

---

## API Operations at a Glance

### Product Management
| Operation | Description |
|-----------|-------------|
| `storeProducts` | Add or update products (partial sync) |
| `storeFullProductSet` | Replace entire product catalog (full sync) |
| `deleteProducts` | Remove specific products |
| `deleteAllProducts` | Clear all products |
| `getAllProducts` | Retrieve all products |

### Customer Management
| Operation | Description |
|-----------|-------------|
| `storeCustomers` | Add or update customers (partial sync) |
| `storeFullCustomerSet` | Replace entire customer base (full sync) |
| `deleteCustomers` | Remove specific customers |
| `deleteAllCustomers` | Clear all customers |

### Order Management
| Operation | Description |
|-----------|-------------|
| `getUnprocessedOrders` | Get orders ready for ERP processing |
| `getOrders` | Get orders by order number |
| `markOrdersAsProcessed` | Mark orders as processed in ERP |

### Real-time Updates
| Operation | Description |
|-----------|-------------|
| `updateStock` | Update stock levels without full product sync |
| `updatePrices` | Update prices without full product sync |
| `updateExtraFields` | Update extra fields without full product sync |

---

## Understanding Sync Patterns

{% tabs %}
{% tab title="Full Sync" %}
**Operations:** `storeFullProductSet`, `storeFullCustomerSet`

Replaces all existing data with the provided set. Items not included in the request will be deleted.

```
Before: [A, B, C, D]
Request: [A, B, E]
After: [A, B, E]  ← C and D are removed
```

**Use case:** Initial data load, daily full synchronization
{% endtab %}

{% tab title="Partial Sync" %}
**Operations:** `storeProducts`, `storeCustomers`

Adds new items or updates existing ones. Items not included in the request remain unchanged.

```
Before: [A, B, C, D]
Request: [A, E]
After: [A, B, C, D, E]  ← A is updated, E is added
```

**Use case:** Real-time updates, incremental changes
{% endtab %}
{% endtabs %}

---

## Next Steps

<table data-view="cards">
<thead>
<tr>
<th></th>
<th></th>
<th data-hidden data-card-target data-type="content-ref"></th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Getting Started</strong></td>
<td>Authentication and first API call</td>
<td><a href="getting-started/authentication.md">authentication.md</a></td>
</tr>
<tr>
<td><strong>Product Operations</strong></td>
<td>Sync your product catalog</td>
<td><a href="operations/products.md">products.md</a></td>
</tr>
<tr>
<td><strong>Order Operations</strong></td>
<td>Retrieve and process orders</td>
<td><a href="operations/orders.md">orders.md</a></td>
</tr>
<tr>
<td><strong>Data Types Reference</strong></td>
<td>Complete type documentation</td>
<td><a href="data-types/overview.md">overview.md</a></td>
</tr>
</tbody>
</table>
