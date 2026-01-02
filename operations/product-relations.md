# Product Relation Operations

Define relationships between products to enable cross-selling features like "matching items" and product succession chains for carry-over styles.

---

## Operations Overview

| Operation | Type | Description |
|-----------|------|-------------|
| [`storeFullProductRelations`](#storefullproductrelations) | Full Sync | Replace all product relations |
| [`storeProductRelations`](#storeproductrelations) | Partial Sync | Add or update specific relations |
| [`deleteProductRelations`](#deleteproductrelations) | Delete | Remove specific relations |
| [`deleteAllProductRelations`](#deleteallproductrelations) | Delete | Clear all relations |

---

## Relation Types

| Type | Description | Use Case |
|------|-------------|----------|
| `MATCHING_SET` | Products that go well together | Show matching items (shirt + pants) |
| `SUCCESSOR` | New version of an old product | Show replacement for discontinued items |

---

## storeFullProductRelations

Replace all product relations. Existing relations not in the request will be **deleted**.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/"
    xmlns:ws="http://ws.cc.salesapp.apptitude.nl/">
    <soapenv:Body>
        <api:storeFullProductRelations>
            <api:apiKey>your-api-key</api:apiKey>
            <api:failOnUnknownProducts>true</api:failOnUnknownProducts>

            <!-- Matching set: Shirt + Pants + Belt -->
            <ws:productRelation>
                <ws:sourceProductUniqueId>SHIRT-001</ws:sourceProductUniqueId>
                <ws:sourceProductColorCode>BLU</ws:sourceProductColorCode>
                <ws:targetProductUniqueId>PANTS-001</ws:targetProductUniqueId>
                <ws:targetProductColorCode>NVY</ws:targetProductColorCode>
                <ws:type>MATCHING_SET</ws:type>
            </ws:productRelation>
            <ws:productRelation>
                <ws:sourceProductUniqueId>SHIRT-001</ws:sourceProductUniqueId>
                <ws:sourceProductColorCode>BLU</ws:sourceProductColorCode>
                <ws:targetProductUniqueId>BELT-001</ws:targetProductUniqueId>
                <ws:targetProductColorCode>BRN</ws:targetProductColorCode>
                <ws:type>MATCHING_SET</ws:type>
            </ws:productRelation>

            <!-- Successor: Old style → New style -->
            <ws:productRelation>
                <ws:sourceProductUniqueId>TSHIRT-V1</ws:sourceProductUniqueId>
                <ws:sourceProductColorCode>BLK</ws:sourceProductColorCode>
                <ws:targetProductUniqueId>TSHIRT-V2</ws:targetProductUniqueId>
                <ws:targetProductColorCode>BLK</ws:targetProductColorCode>
                <ws:type>SUCCESSOR</ws:type>
            </ws:productRelation>
        </api:storeFullProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyIKICAgIHhtbG5zOndzPSJodHRwOi8vd3MuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpCb2R5PgogICAgICAgIDxhcGk6c3RvcmVGdWxsUHJvZHVjdFJlbGF0aW9ucz4KICAgICAgICAgICAgPGFwaTphcGlLZXk+eW91ci1hcGkta2V5PC9hcGk6YXBpS2V5PgogICAgICAgICAgICA8YXBpOmZhaWxPblVua25vd25Qcm9kdWN0cz50cnVlPC9hcGk6ZmFpbE9uVW5rbm93blByb2R1Y3RzPgogICAgICAgICAgICA8d3M6cHJvZHVjdFJlbGF0aW9uPgogICAgICAgICAgICAgICAgPHdzOnNvdXJjZVByb2R1Y3RVbmlxdWVJZD5TSElSVC0wMDE8L3dzOnNvdXJjZVByb2R1Y3RVbmlxdWVJZD4KICAgICAgICAgICAgICAgIDx3czpzb3VyY2VQcm9kdWN0Q29sb3JDb2RlPkJMVTwvd3M6c291cmNlUHJvZHVjdENvbG9yQ29kZT4KICAgICAgICAgICAgICAgIDx3czp0YXJnZXRQcm9kdWN0VW5pcXVlSWQ+UEFOVFMtMDAxPC93czp0YXJnZXRQcm9kdWN0VW5pcXVlSWQ+CiAgICAgICAgICAgICAgICA8d3M6dGFyZ2V0UHJvZHVjdENvbG9yQ29kZT5OVlk8L3dzOnRhcmdldFByb2R1Y3RDb2xvckNvZGU+CiAgICAgICAgICAgICAgICA8d3M6dHlwZT5NQVRDSElOR19TRVQ8L3dzOnR5cGU+CiAgICAgICAgICAgIDwvd3M6cHJvZHVjdFJlbGF0aW9uPgogICAgICAgICAgICA8d3M6cHJvZHVjdFJlbGF0aW9uPgogICAgICAgICAgICAgICAgPHdzOnNvdXJjZVByb2R1Y3RVbmlxdWVJZD5TSElSVC0wMDE8L3dzOnNvdXJjZVByb2R1Y3RVbmlxdWVJZD4KICAgICAgICAgICAgICAgIDx3czpzb3VyY2VQcm9kdWN0Q29sb3JDb2RlPkJMVTwvd3M6c291cmNlUHJvZHVjdENvbG9yQ29kZT4KICAgICAgICAgICAgICAgIDx3czp0YXJnZXRQcm9kdWN0VW5pcXVlSWQ+QkVMVC0wMDE8L3dzOnRhcmdldFByb2R1Y3RVbmlxdWVJZD4KICAgICAgICAgICAgICAgIDx3czp0YXJnZXRQcm9kdWN0Q29sb3JDb2RlPkJSTjwvd3M6dGFyZ2V0UHJvZHVjdENvbG9yQ29kZT4KICAgICAgICAgICAgICAgIDx3czp0eXBlPk1BVENISU5HX1NFVDwvd3M6dHlwZT4KICAgICAgICAgICAgPC93czpwcm9kdWN0UmVsYXRpb24+CiAgICAgICAgICAgIDx3czpwcm9kdWN0UmVsYXRpb24+CiAgICAgICAgICAgICAgICA8d3M6c291cmNlUHJvZHVjdFVuaXF1ZUlkPlRTSElSVC1WMTwvd3M6c291cmNlUHJvZHVjdFVuaXF1ZUlkPgogICAgICAgICAgICAgICAgPHdzOnNvdXJjZVByb2R1Y3RDb2xvckNvZGU+QkxLPC93czpzb3VyY2VQcm9kdWN0Q29sb3JDb2RlPgogICAgICAgICAgICAgICAgPHdzOnRhcmdldFByb2R1Y3RVbmlxdWVJZD5UU0hJUlQtVjI8L3dzOnRhcmdldFByb2R1Y3RVbmlxdWVJZD4KICAgICAgICAgICAgICAgIDx3czp0YXJnZXRQcm9kdWN0Q29sb3JDb2RlPkJMSzwvd3M6dGFyZ2V0UHJvZHVjdENvbG9yQ29kZT4KICAgICAgICAgICAgICAgIDx3czp0eXBlPlNVQ0NFU1NPUjwvd3M6dHlwZT4KICAgICAgICAgICAgPC93czpwcm9kdWN0UmVsYXRpb24+CiAgICAgICAgPC9hcGk6c3RvcmVGdWxsUHJvZHVjdFJlbGF0aW9ucz4KICAgIDwvc29hcGVudjpCb2R5Pgo8L3NvYXBlbnY6RW52ZWxvcGU+)

### Response

```xml
<ns2:storeFullProductRelationsResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
```

{% hint style="warning" %}
Note the different namespace for `productRelation`: `xmlns:ws="http://ws.cc.salesapp.apptitude.nl/"`
{% endhint %}

---

## storeProductRelations

Add or update specific product relations without affecting others.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/"
    xmlns:ws="http://ws.cc.salesapp.apptitude.nl/">
    <soapenv:Body>
        <api:storeProductRelations>
            <api:apiKey>your-api-key</api:apiKey>
            <api:failOnUnknownProducts>true</api:failOnUnknownProducts>

            <ws:productRelation>
                <ws:sourceProductUniqueId>JACKET-001</ws:sourceProductUniqueId>
                <ws:sourceProductColorCode>BLK</ws:sourceProductColorCode>
                <ws:targetProductUniqueId>PANTS-002</ws:targetProductUniqueId>
                <ws:targetProductColorCode>BLK</ws:targetProductColorCode>
                <ws:type>MATCHING_SET</ws:type>
            </ws:productRelation>
        </api:storeProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyIKICAgIHhtbG5zOndzPSJodHRwOi8vd3MuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpCb2R5PgogICAgICAgIDxhcGk6c3RvcmVQcm9kdWN0UmVsYXRpb25zPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgICAgIDxhcGk6ZmFpbE9uVW5rbm93blByb2R1Y3RzPnRydWU8L2FwaTpmYWlsT25Vbmtub3duUHJvZHVjdHM+CiAgICAgICAgICAgIDx3czpwcm9kdWN0UmVsYXRpb24+CiAgICAgICAgICAgICAgICA8d3M6c291cmNlUHJvZHVjdFVuaXF1ZUlkPkpBQ0tFVC0wMDE8L3dzOnNvdXJjZVByb2R1Y3RVbmlxdWVJZD4KICAgICAgICAgICAgICAgIDx3czpzb3VyY2VQcm9kdWN0Q29sb3JDb2RlPkJMSzwvd3M6c291cmNlUHJvZHVjdENvbG9yQ29kZT4KICAgICAgICAgICAgICAgIDx3czp0YXJnZXRQcm9kdWN0VW5pcXVlSWQ+UEFOVFMtMDAyPC93czp0YXJnZXRQcm9kdWN0VW5pcXVlSWQ+CiAgICAgICAgICAgICAgICA8d3M6dGFyZ2V0UHJvZHVjdENvbG9yQ29kZT5CTEs8L3dzOnRhcmdldFByb2R1Y3RDb2xvckNvZGU+CiAgICAgICAgICAgICAgICA8d3M6dHlwZT5NQVRDSElOR19TRVQ8L3dzOnR5cGU+CiAgICAgICAgICAgIDwvd3M6cHJvZHVjdFJlbGF0aW9uPgogICAgICAgIDwvYXBpOnN0b3JlUHJvZHVjdFJlbGF0aW9ucz4KICAgIDwvc29hcGVudjpCb2R5Pgo8L3NvYXBlbnY6RW52ZWxvcGU+)

---

## deleteProductRelations

Remove specific product relations.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/"
    xmlns:ws="http://ws.cc.salesapp.apptitude.nl/">
    <soapenv:Body>
        <api:deleteProductRelations>
            <api:apiKey>your-api-key</api:apiKey>

            <ws:productRelation>
                <ws:sourceProductUniqueId>JACKET-001</ws:sourceProductUniqueId>
                <ws:sourceProductColorCode>BLK</ws:sourceProductColorCode>
                <ws:targetProductUniqueId>PANTS-002</ws:targetProductUniqueId>
                <ws:targetProductColorCode>BLK</ws:targetProductColorCode>
                <ws:type>MATCHING_SET</ws:type>
            </ws:productRelation>
        </api:deleteProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyIKICAgIHhtbG5zOndzPSJodHRwOi8vd3MuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpCb2R5PgogICAgICAgIDxhcGk6ZGVsZXRlUHJvZHVjdFJlbGF0aW9ucz4KICAgICAgICAgICAgPGFwaTphcGlLZXk+eW91ci1hcGkta2V5PC9hcGk6YXBpS2V5PgogICAgICAgICAgICA8d3M6cHJvZHVjdFJlbGF0aW9uPgogICAgICAgICAgICAgICAgPHdzOnNvdXJjZVByb2R1Y3RVbmlxdWVJZD5KQUNLRVQtMDAxPC93czpzb3VyY2VQcm9kdWN0VW5pcXVlSWQ+CiAgICAgICAgICAgICAgICA8d3M6c291cmNlUHJvZHVjdENvbG9yQ29kZT5CTEs8L3dzOnNvdXJjZVByb2R1Y3RDb2xvckNvZGU+CiAgICAgICAgICAgICAgICA8d3M6dGFyZ2V0UHJvZHVjdFVuaXF1ZUlkPlBBTlRTLTAwMjwvd3M6dGFyZ2V0UHJvZHVjdFVuaXF1ZUlkPgogICAgICAgICAgICAgICAgPHdzOnRhcmdldFByb2R1Y3RDb2xvckNvZGU+QkxLPC93czp0YXJnZXRQcm9kdWN0Q29sb3JDb2RlPgogICAgICAgICAgICAgICAgPHdzOnR5cGU+TUFUQ0hJTkdfU0VUPC93czp0eXBlPgogICAgICAgICAgICA8L3dzOnByb2R1Y3RSZWxhdGlvbj4KICAgICAgICA8L2FwaTpkZWxldGVQcm9kdWN0UmVsYXRpb25zPgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)

---

## deleteAllProductRelations

Remove all product relations.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Body>
        <api:deleteAllProductRelations>
            <api:apiKey>your-api-key</api:apiKey>
        </api:deleteAllProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpCb2R5PgogICAgICAgIDxhcGk6ZGVsZXRlQWxsUHJvZHVjdFJlbGF0aW9ucz4KICAgICAgICAgICAgPGFwaTphcGlLZXk+eW91ci1hcGkta2V5PC9hcGk6YXBpS2V5PgogICAgICAgIDwvYXBpOmRlbGV0ZUFsbFByb2R1Y3RSZWxhdGlvbnM+CiAgICA8L3NvYXBlbnY6Qm9keT4KPC9zb2FwZW52OkVudmVsb3BlPg==)

---

## Product Relation Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `sourceProductUniqueId` | String | No | Source product style ID |
| `sourceProductColorCode` | String | No | Source product color |
| `targetProductUniqueId` | String | No | Related product style ID |
| `targetProductColorCode` | String | No | Related product color |
| `type` | Enum | No | `MATCHING_SET` or `SUCCESSOR` |

---

## Matching Set Relations

Use matching sets to suggest complementary products:

```
Source Product → Target Products
   SHIRT-001/BLU → PANTS-001/NVY (matching)
   SHIRT-001/BLU → BELT-001/BRN (matching)
   SHIRT-001/BLU → TIE-001/BLU (matching)
```

### Example: Complete Outfit

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/"
    xmlns:ws="http://ws.cc.salesapp.apptitude.nl/">
    <soapenv:Body>
        <api:storeProductRelations>
            <api:apiKey>your-api-key</api:apiKey>

            <!-- Base: Blue Dress Shirt -->
            <ws:productRelation>
                <ws:sourceProductUniqueId>DRESS-SHIRT</ws:sourceProductUniqueId>
                <ws:sourceProductColorCode>BLU</ws:sourceProductColorCode>
                <ws:targetProductUniqueId>CHINOS</ws:targetProductUniqueId>
                <ws:targetProductColorCode>KHK</ws:targetProductColorCode>
                <ws:type>MATCHING_SET</ws:type>
            </ws:productRelation>

            <ws:productRelation>
                <ws:sourceProductUniqueId>DRESS-SHIRT</ws:sourceProductUniqueId>
                <ws:sourceProductColorCode>BLU</ws:sourceProductColorCode>
                <ws:targetProductUniqueId>BLAZER</ws:targetProductUniqueId>
                <ws:targetProductColorCode>NVY</ws:targetProductColorCode>
                <ws:type>MATCHING_SET</ws:type>
            </ws:productRelation>

            <ws:productRelation>
                <ws:sourceProductUniqueId>DRESS-SHIRT</ws:sourceProductUniqueId>
                <ws:sourceProductColorCode>BLU</ws:sourceProductColorCode>
                <ws:targetProductUniqueId>LEATHER-BELT</ws:targetProductUniqueId>
                <ws:targetProductColorCode>TAN</ws:targetProductColorCode>
                <ws:type>MATCHING_SET</ws:type>
            </ws:productRelation>
        </api:storeProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

In the app, when viewing the blue dress shirt, customers see:
- "Complete the look" or "Matching items" section
- Khaki chinos, navy blazer, tan belt suggestions

---

## Successor Relations

Use successors for product evolution and carry-over styles:

```
Old Product → New Product (successor)
   TSHIRT-V1/BLK → TSHIRT-V2/BLK
```

### Example: Product Evolution

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/"
    xmlns:ws="http://ws.cc.salesapp.apptitude.nl/">
    <soapenv:Body>
        <api:storeProductRelations>
            <api:apiKey>your-api-key</api:apiKey>

            <!-- Classic Polo discontinued, replaced by Premium Polo -->
            <ws:productRelation>
                <ws:sourceProductUniqueId>CLASSIC-POLO</ws:sourceProductUniqueId>
                <ws:sourceProductColorCode>WHT</ws:sourceProductColorCode>
                <ws:targetProductUniqueId>PREMIUM-POLO</ws:targetProductUniqueId>
                <ws:targetProductColorCode>WHT</ws:targetProductColorCode>
                <ws:type>SUCCESSOR</ws:type>
            </ws:productRelation>

            <ws:productRelation>
                <ws:sourceProductUniqueId>CLASSIC-POLO</ws:sourceProductUniqueId>
                <ws:sourceProductColorCode>NVY</ws:sourceProductColorCode>
                <ws:targetProductUniqueId>PREMIUM-POLO</ws:targetProductUniqueId>
                <ws:targetProductColorCode>NVY</ws:targetProductColorCode>
                <ws:type>SUCCESSOR</ws:type>
            </ws:productRelation>
        </api:storeProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

Use cases:
- Reordering discontinued items → show successor
- Historical orders → link to current equivalent
- Season carry-over with style updates

---

## Handling Unknown Products

The `failOnUnknownProducts` parameter controls behavior when referenced products don't exist:

| Value | Behavior |
|-------|----------|
| `true` | Operation fails if any product is unknown |
| `false` | Unknown products are silently ignored |

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/"
    xmlns:ws="http://ws.cc.salesapp.apptitude.nl/">
    <soapenv:Body>
        <api:storeProductRelations>
            <api:apiKey>your-api-key</api:apiKey>
            <api:failOnUnknownProducts>false</api:failOnUnknownProducts>
            <!-- Relations referencing unknown products will be skipped -->
        </api:storeProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

{% hint style="info" %}
Use `failOnUnknownProducts=true` during development to catch data issues. Use `false` in production if your product sync might be incomplete.
{% endhint %}

---

## Bidirectional Relations

{% hint style="warning" %}
**Relations are one-directional.** To show matching items from both products, create relations in both directions.
{% endhint %}

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/"
    xmlns:ws="http://ws.cc.salesapp.apptitude.nl/">
    <soapenv:Body>
        <api:storeProductRelations>
            <api:apiKey>your-api-key</api:apiKey>

            <!-- Shirt shows Pants as matching -->
            <ws:productRelation>
                <ws:sourceProductUniqueId>SHIRT-001</ws:sourceProductUniqueId>
                <ws:sourceProductColorCode>BLU</ws:sourceProductColorCode>
                <ws:targetProductUniqueId>PANTS-001</ws:targetProductUniqueId>
                <ws:targetProductColorCode>NVY</ws:targetProductColorCode>
                <ws:type>MATCHING_SET</ws:type>
            </ws:productRelation>

            <!-- Pants shows Shirt as matching -->
            <ws:productRelation>
                <ws:sourceProductUniqueId>PANTS-001</ws:sourceProductUniqueId>
                <ws:sourceProductColorCode>NVY</ws:sourceProductColorCode>
                <ws:targetProductUniqueId>SHIRT-001</ws:targetProductUniqueId>
                <ws:targetProductColorCode>BLU</ws:targetProductColorCode>
                <ws:type>MATCHING_SET</ws:type>
            </ws:productRelation>
        </api:storeProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

---

## Best Practices

{% tabs %}
{% tab title="Matching Sets" %}
- Create bidirectional relations for mutual matching
- Limit to 3-6 matching items per product
- Match by style theme and color coordination
- Update when new collections launch
{% endtab %}

{% tab title="Successors" %}
- Only create when old product is discontinued
- Point to exact color equivalent when possible
- Maintain chain: V1 → V2 → V3 (each points to next)
- Use for carry-over styles between seasons
{% endtab %}

{% tab title="Sync Strategy" %}
```
Recommended approach:

Full Sync (weekly):
└── storeFullProductRelations with all relations

Incremental (as needed):
├── storeProductRelations for new relations
└── deleteProductRelations for removed ones

After product import:
└── Sync relations that reference new products
```
{% endtab %}
{% endtabs %}
