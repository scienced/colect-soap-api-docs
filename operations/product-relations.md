# Product Relations

Define relationships between products to enable cross-selling features like "matching items" and product succession chains for carry-over styles.

***

## Operations Overview

| Operation                                                                     | Type         | Description                      |
| ----------------------------------------------------------------------------- | ------------ | -------------------------------- |
| [`storeFullProductRelations`](product-relations.md#storefullproductrelations) | Full Sync    | Replace all product relations    |
| [`storeProductRelations`](product-relations.md#storeproductrelations)         | Partial Sync | Add or update specific relations |
| [`deleteProductRelations`](product-relations.md#deleteproductrelations)       | Delete       | Remove specific relations        |
| [`deleteAllProductRelations`](product-relations.md#deleteallproductrelations) | Delete       | Clear all relations              |

***

## Relation Types

| Type           | Description                    | Use Case                                |
| -------------- | ------------------------------ | --------------------------------------- |
| `MATCHING_SET` | Products that go well together | Show matching items (shirt + pants)     |
| `SUCCESSOR`    | New version of an old product  | Show replacement for discontinued items |

***

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
                <api:sourceProductUniqueId>SHIRT-001</api:sourceProductUniqueId>
                <api:sourceProductColorCode>BLU</api:sourceProductColorCode>
                <api:targetProductUniqueId>PANTS-001</api:targetProductUniqueId>
                <api:targetProductColorCode>NVY</api:targetProductColorCode>
                <api:type>MATCHING_SET</api:type>
                <api:groupCode>BLUE-1</api:groupCode>
                <api:groupDescription>Blue Set</api:groupDescription>
            </ws:productRelation>
            <ws:productRelation>
                <api:sourceProductUniqueId>SHIRT-001</api:sourceProductUniqueId>
                <api:sourceProductColorCode>BLU</api:sourceProductColorCode>
                <api:targetProductUniqueId>BELT-001</api:targetProductUniqueId>
                <api:targetProductColorCode>BRN</api:targetProductColorCode>
                <api:type>MATCHING_SET</api:type>
                <api:groupCode>BLUE-1</api:groupCode>
                <api:groupDescription>Blue Set</api:groupDescription>
            </ws:productRelation>

            <!-- Successor: Old style → New style -->
            <ws:productRelation>
                <api:sourceProductUniqueId>TSHIRT-V1</api:sourceProductUniqueId>
                <api:sourceProductColorCode>BLK</api:sourceProductColorCode>
                <api:targetProductUniqueId>TSHIRT-V2</api:targetProductUniqueId>
                <api:targetProductColorCode>BLK</api:targetProductColorCode>
                <api:type>SUCCESSOR</api:type>
                <api:groupCode>NEW26</api:groupCode>
                <api:groupDescription>New for 2026</api:groupDescription>
            </ws:productRelation>
        </api:storeFullProductRelations>
    </soapenv:Body>
```

### Response

```xml
<ns2:storeFullProductRelationsResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
```

{% hint style="warning" %}
Note the different namespace for `productRelation`: `xmlns:ws="http://ws.cc.salesapp.apptitude.nl/"`
{% endhint %}

***

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
                <api:sourceProductUniqueId>JACKET-001</api:sourceProductUniqueId>
                <api:sourceProductColorCode>BLK</api:sourceProductColorCode>
                <api:targetProductUniqueId>PANTS-002</api:targetProductUniqueId>
                <api:targetProductColorCode>BLK</api:targetProductColorCode>
                <api:type>MATCHING_SET</api:type>
                <api:groupCode>BLACL-1</api:groupCode>
                <api:groupDescription>Black Set</api:groupDescription>
            </ws:productRelation>
        </api:storeProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

***

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
                <api:sourceProductUniqueId>JACKET-001</api:sourceProductUniqueId>
                <api:sourceProductColorCode>BLK</api:sourceProductColorCode>
                <api:targetProductUniqueId>PANTS-002</api:targetProductUniqueId>
                <api:targetProductColorCode>BLK</api:targetProductColorCode>
                <api:type>MATCHING_SET</api:type>
            </ws:productRelation>
        </api:deleteProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

***

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

***

## Product Relation Fields

<table><thead><tr><th width="237.13671875">Field</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody><tr><td><code>sourceProductUniqueId</code></td><td>String</td><td>Yes</td><td>Source product style ID</td></tr><tr><td><code>sourceProductColorCode</code></td><td>String</td><td>No</td><td>Source product color</td></tr><tr><td><code>targetProductUniqueId</code></td><td>String</td><td>No</td><td>Related product style ID</td></tr><tr><td><code>targetProductColorCode</code></td><td>String</td><td>No</td><td>Related product color</td></tr><tr><td><code>type</code></td><td>Enum</td><td>No</td><td><code>MATCHING_SET</code> or <code>SUCCESSOR</code></td></tr><tr><td><code>groupCode</code></td><td>String</td><td>Yes</td><td>Code of the group this relation belongs to</td></tr><tr><td><code>groupDescription</code></td><td>String</td><td>No</td><td>Description of the group this relation belongs to</td></tr></tbody></table>



***

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
                <api:sourceProductUniqueId>DRESS-SHIRT</api:sourceProductUniqueId>
                <api:sourceProductColorCode>BLU</api:sourceProductColorCode>
                <api:targetProductUniqueId>CHINOS</api:targetProductUniqueId>
                <api:targetProductColorCode>KHK</api:targetProductColorCode>
                <api:type>MATCHING_SET</api:type>
                <api:groupCode>BLUE-1</api:groupCode>
                <api:groupDescription>Blue Set</api:groupDescription>
            </ws:productRelation>

            <ws:productRelation>
                <api:sourceProductUniqueId>DRESS-SHIRT</api:sourceProductUniqueId>
                <api:sourceProductColorCode>BLU</api:sourceProductColorCode>
                <api:targetProductUniqueId>BLAZER</api:targetProductUniqueId>
                <api:targetProductColorCode>NVY</api:targetProductColorCode>
                <api:type>MATCHING_SET</api:type>
                <api:groupCode>BLUE-1</api:groupCode>
                <api:groupDescription>Blue Set</api:groupDescription>
            </ws:productRelation>

            <ws:productRelation>
                <api:sourceProductUniqueId>DRESS-SHIRT</api:sourceProductUniqueId>
                <api:sourceProductColorCode>BLU</api:sourceProductColorCode>
                <api:targetProductUniqueId>LEATHER-BELT</api:targetProductUniqueId>
                <api:targetProductColorCode>TAN</api:targetProductColorCode>
                <api:type>MATCHING_SET</api:type>
                <api:groupCode>BLUE-1</api:groupCode>
                <api:groupDescription>Blue Set</api:groupDescription>
            </ws:productRelation>
        </api:storeProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

In the app, when viewing the blue dress shirt, customers see:

* "Complete the look" or "Matching items" section
* Khaki chinos, navy blazer, tan belt suggestions

***

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
                <api:sourceProductUniqueId>CLASSIC-POLO</api:sourceProductUniqueId>
                <api:sourceProductColorCode>WHT</api:sourceProductColorCode>
                <api:targetProductUniqueId>PREMIUM-POLO</api:targetProductUniqueId>
                <api:targetProductColorCode>WHT</api:targetProductColorCode>
                <api:type>SUCCESSOR</api:type>
                <api:groupCode>NEW26</api:groupCode>
                <api:groupDescription>New for 2026</api:groupDescription>
            </ws:productRelation>

            <ws:productRelation>
                <api:sourceProductUniqueId>CLASSIC-POLO</api:sourceProductUniqueId>
                <api:sourceProductColorCode>NVY</api:sourceProductColorCode>
                <api:targetProductUniqueId>PREMIUM-POLO</api:targetProductUniqueId>
                <api:targetProductColorCode>NVY</api:targetProductColorCode>
                <api:type>SUCCESSOR</api:type>
                <api:groupCode>NEW26</api:groupCode>
                <api:groupDescription>New for 2026</api:groupDescription>
            </ws:productRelation>
        </api:storeProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

Use cases:

* Reordering discontinued items → show successor
* Historical orders → link to current equivalent
* Season carry-over with style updates

***

## Handling Unknown Products

The `failOnUnknownProducts` parameter controls behavior when referenced products don't exist:

| Value   | Behavior                                  |
| ------- | ----------------------------------------- |
| `true`  | Operation fails if any product is unknown |
| `false` | Unknown products are silently ignored     |

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

***

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
                <api:sourceProductUniqueId>SHIRT-001</api:sourceProductUniqueId>
                <api:sourceProductColorCode>BLU</api:sourceProductColorCode>
                <api:targetProductUniqueId>PANTS-001</api:targetProductUniqueId>
                <api:targetProductColorCode>NVY</api:targetProductColorCode>
                <api:type>MATCHING_SET</api:type>
                <api:groupCode>BLUE-1</api:groupCode>
                <api:groupDescription>Blue Set</api:groupDescription>
            </ws:productRelation>

            <!-- Pants shows Shirt as matching -->
            <ws:productRelation>
                <api:sourceProductUniqueId>PANTS-001</api:sourceProductUniqueId>
                <api:sourceProductColorCode>NVY</api:sourceProductColorCode>
                <api:targetProductUniqueId>SHIRT-001</api:targetProductUniqueId>
                <api:targetProductColorCode>BLU</api:targetProductColorCode>
                <api:type>MATCHING_SET</api:type>
                <api:groupCode>BLUE-1</api:groupCode>
                <api:groupDescription>Blue Set</api:groupDescription>
            </ws:productRelation>
        </api:storeProductRelations>
    </soapenv:Body>
</soapenv:Envelope>
```

***

## Best Practices

{% tabs %}
{% tab title="Matching Sets" %}
* Create bidirectional relations for mutual matching
* Limit to 3-6 matching items per product
* Match by style theme and color coordination
* Update when new collections launch
{% endtab %}

{% tab title="Successors" %}
* Only create when old product is discontinued
* Point to exact color equivalent when possible
* Maintain chain: V1 → V2 → V3 (each points to next)
* Use for carry-over styles between seasons
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
