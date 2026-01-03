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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqNLWFdAHcQIyYWdjCLKxs7BydHVw9vX38aAD5AzVA5SiQAITBPAE9srUqcYAMyBkkYADF3DAwABQkS904GACVMCFcwEgqqsZqofT1JgGkYctKwdwkAWgMVgGt54DNJ6f058pyxrQmpgDMIfQwAeTgAVTgNuDAQuA6unpBMhgl3GB2tUu1zuj2er3enU83QY3zUxxO1QAhCsVjgALKDbj6OBSHAgGAMJA4ADKdH0EgYOAA1Dg2hA4LCaThCpgqajRojcmEkFAoTD+hhBvphpyuZVgDzwMtODAPtCeo99ABHf4ASU8mRJAAk1b0ACorAAMRoAjDspUsJLL5TClaqYBqxeLuaRpda5fyegBhMBYCS+7yZQoAGXuFrdVptXoYvv9gZgzpdktIDAg0kJtsVcBV6s1bQAggA5fUk41miNINMZhhZhj2vNJ8UpqvpqSZmNxyQJzJFgBqAE1K9X27XO37uyVEwjkzyGKVYJl0QX9d7dUWAOIAfRJKH1w4X05d1VYPL5nz6AyGcCbVRb54Vl6F19viJb7ujF4bjs1Or1hpNc1T0jGVPS-HMHSdGdm0tUC6y7AMp2DMNKw-MDHwQntoK5FsRw7cDcx-YMUBDACK2A1sazrb8oOPCU5zbfCMInRCg0KXoi2HRixwvTCkOwt850PJcVzXNVNx3PcD0XATTgoh8BSvEUb3hY9gBRNESW6WUQHACRiRuDBPHxec7BwQAkwhwIsYBCEzSjMjlZNdXkY0FYVRScu9YI9aiIMbUtdQNFY+yA7zP0fGjNU8+iQJ88d4yQ0MZlQqN0JhPig2i04GKomNIsyAL-2CgAmLjcoIyCoro7LU24+CWJ7JKytHeqEsy6rnPnRcSXub1vRQEkSRuXppKPZN5NcpSPLfXYphAeoJCaFp2km59lLhCVZAoRAihKI4dlEAo0AQJRFyAA)

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqNLWFdAHcQIyYWdjCLKxs7BydHVw9vX38aAD5AzVA5SiQAITBPAE9srUqcYAMyBkkYAAUJEvdOBgAlTAhXMBIKqsGaqH09EYBpGHLSsHcJAFoDeYBrKeAzEbH9SfKcwa1h0YAzCH0MAHk4AFU4ZbgwELhm1vaQTIYJdxh12pOzy5udweTxanjaDDee321TCSCgoPBXQwPX0fQG0KGsPAc04TQR7Ru+gAjl8AJKeTIAKQAggBhcYoAAq8wADCyAIzrLGzCS455gglwYlkilQjG5bk4vEvBi0sBYCRy7yZQoAGXGXNI2N50oFsvlkiVMHR4oOsIYEGkMAY-PBhJJMHJmUa1IAcoyAMqslkAJk1SAtVpt+IY9pFJtNMNIgak1tt7TlCqNKvV-pjcZDicNJWNYvFwHNpVgmQAstTGbSABKk10AcQA+h6mWmi7nI-74TKkSi0XnqhtRiB6hJdYjur1+nt1qICsUytlp-lEKh0Fhi0A)

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqNLWFdAHcQIyYWdjCLKxs7BydHVw9vX38aAD5AzVA5SiQAITBPAE9srUqcYAMkbzsGGAAFCRL3TgYAJUwIVzASCqqhmqh9PVGAaRhy0rB3CQBaAwWAa2ngM1Hx-SnynKGtYDCkKFbPdq6evrhBg4Oj0nB5zmazi4BVOH0AR3cYAElPJkAFIAQQAwhMUAAVBYABjhAEYNscnhIXi02h1Pj8-oDbncqg8yHN0a8sQxwWAsBIqd5MoUADITFGPUkYt4dKk0ukwAmEw7HBgQaQwBiY87Yr6-AFApqggBy0IAyvC4QAmVlIYWi8Wchg4mX4-YC3JCkVSMUSi7cyS8hnMrU6y16im22klPkm03EhilWCZACyoOh4IAEv8FQBxAD6yphTv9XtN1VYx1OFO6GF6+n6-MOmzG9TF5Mll2z1xABI2ogKxTK2Rr+UQqHQWADQA)

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAhME8ATyCtBJxgAyRvOwYYAEEMDAAFCWj3TgYAJUwIVzASeMTa5Kh9PQaAaRg4mLB3CQBaA26AazbgMwam-Va44LqRxrSYDOy8gs8i0vLK6qmk2QpESOjJ4dFwtAQlWACgA)

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
