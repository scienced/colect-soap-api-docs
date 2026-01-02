# Access Rules Operations

Control which products are visible to which customers, and which customers are accessible to which sales agents. Access rules enable personalized catalogs and territory management.

{% hint style="success" %}
**Full Examples:**
- [Download storeProductAccessRules.xml](https://assets.apptitude.nl/KnowledgeBase/Webservice_3_0/External/storeProductAccessRules.xml) - Product access rules example
- [Download storeFullCustomerAccessSetForAgent.xml](https://assets.apptitude.nl/KnowledgeBase/Webservice_3_0/External/storeFullCustomerAccessSetForAgent.xml) - Agent customer access example
{% endhint %}

---

## Operations Overview

### Product Access Rules
| Operation | Description |
|-----------|-------------|
| [`storeFullProductAccessSet`](#storefullproductaccessset) | Replace all product access rules (full sync) |
| [`storeProductAccessRules`](#storeproductaccessrules) | Add or update product access rules |
| [`deleteProductAccessRules`](#deleteproductaccessrules) | Remove specific access rules |
| [`deleteProductAccessRulesForCustomer`](#deleteproductaccessrulesforcustomer) | Remove all rules for a customer |
| [`deleteAllProductAccessRules`](#deleteallproductaccessrules) | Clear all access rules |

### Customer Access Rules
| Operation | Description |
|-----------|-------------|
| [`storeFullCustomerAccessSet`](#storefullcustomeraccessset) | Define which customers each agent can access |
| [`storeFullCustomerAccessSetForAgent`](#storefullcustomeraccesssetforagent) | Define access for a specific agent |

---

## Product Access Fundamentals

By default, all customers can see all products. Product access rules allow you to:

- Restrict products to specific customers
- Exclude products from specific customers
- Filter by brand, category, season, or individual product

{% hint style="info" %}
**Rule Order Matters:** Rules are evaluated sequentially. The order of rules determines the final access result.
{% endhint %}

---

## storeFullProductAccessSet

Replace all product access rules in one operation.

{% hint style="warning" %}
**Caution:** This clears all existing rules and replaces them with the provided set.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
   <soapenv:Header/>
   <soapenv:Body>
      <api:storeFullProductAccessSet>
         <api:apiKey>your-api-key</api:apiKey>
         <!-- Customer CUST-001 can only see BRAND-A products -->
         <api:productAccessRule>
            <api:customerNo>CUST-001</api:customerNo>
            <api:type>REMOVE</api:type>
            <api:operation>CONTAINS</api:operation>
            <api:sortCode>1</api:sortCode>
         </api:productAccessRule>
         <api:productAccessRule>
            <api:customerNo>CUST-001</api:customerNo>
            <api:type>ADD</api:type>
            <api:operation>EQUALS</api:operation>
            <api:brandId>BRAND-A</api:brandId>
            <api:sortCode>2</api:sortCode>
         </api:productAccessRule>
         <!-- Customer CUST-002 can see everything except PREMIUM category -->
         <api:productAccessRule>
            <api:customerNo>CUST-002</api:customerNo>
            <api:type>REMOVE</api:type>
            <api:operation>EQUALS</api:operation>
            <api:categoryCode>PREMIUM</api:categoryCode>
            <api:sortCode>1</api:sortCode>
         </api:productAccessRule>
      </api:storeFullProductAccessSet>
   </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iIHhtbG5zOmFwaT0iaHR0cDovL2FwaS5jYy5zYWxlc2FwcC5hcHB0aXR1ZGUubmwvIj4KICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgPHNvYXBlbnY6Qm9keT4KICAgICAgPGFwaTpzdG9yZUZ1bGxQcm9kdWN0QWNjZXNzU2V0PgogICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgIDwhLS0gQ3VzdG9tZXIgQ1VTVC0wMDEgY2FuIG9ubHkgc2VlIEJSQU5ELUEgcHJvZHVjdHMgLS0+CiAgICAgICAgIDxhcGk6cHJvZHVjdEFjY2Vzc1J1bGU+CiAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJObz5DVVNULTAwMTwvYXBpOmN1c3RvbWVyTm8+CiAgICAgICAgICAgIDxhcGk6dHlwZT5SRU1PVkU8L2FwaTp0eXBlPgogICAgICAgICAgICA8YXBpOm9wZXJhdGlvbj5DT05UQUlOUzwvYXBpOm9wZXJhdGlvbj4KICAgICAgICAgICAgPGFwaTpzb3J0Q29kZT4xPC9hcGk6c29ydENvZGU+CiAgICAgICAgIDwvYXBpOnByb2R1Y3RBY2Nlc3NSdWxlPgogICAgICAgICA8YXBpOnByb2R1Y3RBY2Nlc3NSdWxlPgogICAgICAgICAgICA8YXBpOmN1c3RvbWVyTm8+Q1VTVC0wMDE8L2FwaTpjdXN0b21lck5vPgogICAgICAgICAgICA8YXBpOnR5cGU+QUREPC9hcGk6dHlwZT4KICAgICAgICAgICAgPGFwaTpvcGVyYXRpb24+RVFVQUxTPC9hcGk6b3BlcmF0aW9uPgogICAgICAgICAgICA8YXBpOmJyYW5kSWQ+QlJBTkQtQTwvYXBpOmJyYW5kSWQ+CiAgICAgICAgICAgIDxhcGk6c29ydENvZGU+MjwvYXBpOnNvcnRDb2RlPgogICAgICAgICA8L2FwaTpwcm9kdWN0QWNjZXNzUnVsZT4KICAgICAgICAgPCEtLSBDdXN0b21lciBDVVNULTAwMiBjYW4gc2VlIGV2ZXJ5dGhpbmcgZXhjZXB0IFBSRU1JVU0gY2F0ZWdvcnkgLS0+CiAgICAgICAgIDxhcGk6cHJvZHVjdEFjY2Vzc1J1bGU+CiAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJObz5DVVNULTAwMjwvYXBpOmN1c3RvbWVyTm8+CiAgICAgICAgICAgIDxhcGk6dHlwZT5SRU1PVkU8L2FwaTp0eXBlPgogICAgICAgICAgICA8YXBpOm9wZXJhdGlvbj5FUVVBTFM8L2FwaTpvcGVyYXRpb24+CiAgICAgICAgICAgIDxhcGk6Y2F0ZWdvcnlDb2RlPlBSRU1JVU08L2FwaTpjYXRlZ29yeUNvZGU+CiAgICAgICAgICAgIDxhcGk6c29ydENvZGU+MTwvYXBpOnNvcnRDb2RlPgogICAgICAgICA8L2FwaTpwcm9kdWN0QWNjZXNzUnVsZT4KICAgICAgPC9hcGk6c3RvcmVGdWxsUHJvZHVjdEFjY2Vzc1NldD4KICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)

---

## storeProductAccessRules

Add or update specific access rules without affecting other rules.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
   <soapenv:Header/>
   <soapenv:Body>
      <api:storeProductAccessRules>
         <api:apiKey>your-api-key</api:apiKey>
         <api:productAccessRule>
            <api:customerNo>CUST-003</api:customerNo>
            <api:type>ADD</api:type>
            <api:operation>EQUALS</api:operation>
            <api:brandId>EXCLUSIVE-BRAND</api:brandId>
            <api:sortCode>1</api:sortCode>
         </api:productAccessRule>
      </api:storeProductAccessRules>
   </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iIHhtbG5zOmFwaT0iaHR0cDovL2FwaS5jYy5zYWxlc2FwcC5hcHB0aXR1ZGUubmwvIj4KICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgPHNvYXBlbnY6Qm9keT4KICAgICAgPGFwaTpzdG9yZVByb2R1Y3RBY2Nlc3NSdWxlcz4KICAgICAgICAgPGFwaTphcGlLZXk+eW91ci1hcGkta2V5PC9hcGk6YXBpS2V5PgogICAgICAgICA8YXBpOnByb2R1Y3RBY2Nlc3NSdWxlPgogICAgICAgICAgICA8YXBpOmN1c3RvbWVyTm8+Q1VTVC0wMDM8L2FwaTpjdXN0b21lck5vPgogICAgICAgICAgICA8YXBpOnR5cGU+QUREPC9hcGk6dHlwZT4KICAgICAgICAgICAgPGFwaTpvcGVyYXRpb24+RVFVQUxTPC9hcGk6b3BlcmF0aW9uPgogICAgICAgICAgICA8YXBpOmJyYW5kSWQ+RVhDTFVTSVZFLUJSQU5EPC9hcGk6YnJhbmRJZD4KICAgICAgICAgICAgPGFwaTpzb3J0Q29kZT4xPC9hcGk6c29ydENvZGU+CiAgICAgICAgIDwvYXBpOnByb2R1Y3RBY2Nlc3NSdWxlPgogICAgICA8L2FwaTpzdG9yZVByb2R1Y3RBY2Nlc3NSdWxlcz4KICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)

---

## deleteProductAccessRules

Remove specific access rules.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
   <soapenv:Header/>
   <soapenv:Body>
      <api:deleteProductAccessRules>
         <api:apiKey>your-api-key</api:apiKey>
         <api:productAccessRule>
            <api:customerNo>CUST-003</api:customerNo>
            <api:brandId>EXCLUSIVE-BRAND</api:brandId>
         </api:productAccessRule>
      </api:deleteProductAccessRules>
   </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iIHhtbG5zOmFwaT0iaHR0cDovL2FwaS5jYy5zYWxlc2FwcC5hcHB0aXR1ZGUubmwvIj4KICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgPHNvYXBlbnY6Qm9keT4KICAgICAgPGFwaTpkZWxldGVQcm9kdWN0QWNjZXNzUnVsZXM+CiAgICAgICAgIDxhcGk6YXBpS2V5PnlvdXItYXBpLWtleTwvYXBpOmFwaUtleT4KICAgICAgICAgPGFwaTpwcm9kdWN0QWNjZXNzUnVsZT4KICAgICAgICAgICAgPGFwaTpjdXN0b21lck5vPkNVU1QtMDAzPC9hcGk6Y3VzdG9tZXJObz4KICAgICAgICAgICAgPGFwaTpicmFuZElkPkVYQ0xVU0lWRS1CUkFORDwvYXBpOmJyYW5kSWQ+CiAgICAgICAgIDwvYXBpOnByb2R1Y3RBY2Nlc3NSdWxlPgogICAgICA8L2FwaTpkZWxldGVQcm9kdWN0QWNjZXNzUnVsZXM+CiAgIDwvc29hcGVudjpCb2R5Pgo8L3NvYXBlbnY6RW52ZWxvcGU+)

---

## deleteProductAccessRulesForCustomer

Remove all access rules for a specific customer, returning them to default access (all products).

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
   <soapenv:Header/>
   <soapenv:Body>
      <api:deleteProductAccessRulesForCustomer>
         <api:apiKey>your-api-key</api:apiKey>
         <api:customerNo>CUST-001</api:customerNo>
      </api:deleteProductAccessRulesForCustomer>
   </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iIHhtbG5zOmFwaT0iaHR0cDovL2FwaS5jYy5zYWxlc2FwcC5hcHB0aXR1ZGUubmwvIj4KICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgPHNvYXBlbnY6Qm9keT4KICAgICAgPGFwaTpkZWxldGVQcm9kdWN0QWNjZXNzUnVsZXNGb3JDdXN0b21lcj4KICAgICAgICAgPGFwaTphcGlLZXk+eW91ci1hcGkta2V5PC9hcGk6YXBpS2V5PgogICAgICAgICA8YXBpOmN1c3RvbWVyTm8+Q1VTVC0wMDE8L2FwaTpjdXN0b21lck5vPgogICAgICA8L2FwaTpkZWxldGVQcm9kdWN0QWNjZXNzUnVsZXNGb3JDdXN0b21lcj4KICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)

---

## deleteAllProductAccessRules

Remove all product access rules, giving all customers access to all products.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
   <soapenv:Header/>
   <soapenv:Body>
      <api:deleteAllProductAccessRules>
         <api:apiKey>your-api-key</api:apiKey>
      </api:deleteAllProductAccessRules>
   </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iIHhtbG5zOmFwaT0iaHR0cDovL2FwaS5jYy5zYWxlc2FwcC5hcHB0aXR1ZGUubmwvIj4KICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgPHNvYXBlbnY6Qm9keT4KICAgICAgPGFwaTpkZWxldGVBbGxQcm9kdWN0QWNjZXNzUnVsZXM+CiAgICAgICAgIDxhcGk6YXBpS2V5PnlvdXItYXBpLWtleTwvYXBpOmFwaUtleT4KICAgICAgPC9hcGk6ZGVsZXRlQWxsUHJvZHVjdEFjY2Vzc1J1bGVzPgogICA8L3NvYXBlbnY6Qm9keT4KPC9zb2FwZW52OkVudmVsb3BlPg==)

---

## Product Access Rule Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `customerNo` | String | Yes | Customer to apply rule to |
| `type` | Enum | No | `ADD` (default) or `REMOVE` |
| `operation` | Enum | No | `EQUALS` (default) or `CONTAINS` |
| `environment` | Enum | No | `APP`, `B2B`, or omit for both |
| `uniqueId` | String | No | Specific product ID |
| `colorCode` | String | No | Specific color (requires uniqueId) |
| `categoryCode` | String | No | Product category |
| `productGroupCode` | String | No | Product group |
| `brandId` | String | No | Brand identifier |
| `seasonCode` | String | No | Season code |
| `sortCode` | Integer | No | Rule evaluation order |

### Rule Type

| Type | Description |
|------|-------------|
| `ADD` | Grant access to matching products |
| `REMOVE` | Deny access to matching products |

### Match Operation

| Operation | Description |
|-----------|-------------|
| `EQUALS` | Exact match (default) |
| `CONTAINS` | Substring match |

---

## Rule Evaluation Examples

{% tabs %}
{% tab title="Example 1: Brand Exclusive" %}
**Goal:** Customer only sees Brand X products

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
   <soapenv:Header/>
   <soapenv:Body>
      <api:storeProductAccessRules>
         <api:apiKey>your-api-key</api:apiKey>
         <!-- First, remove access to everything -->
         <api:productAccessRule>
            <api:customerNo>CUST-001</api:customerNo>
            <api:type>REMOVE</api:type>
            <api:sortCode>1</api:sortCode>
         </api:productAccessRule>
         <!-- Then, add access to Brand X only -->
         <api:productAccessRule>
            <api:customerNo>CUST-001</api:customerNo>
            <api:type>ADD</api:type>
            <api:brandId>BRAND-X</api:brandId>
            <api:sortCode>2</api:sortCode>
         </api:productAccessRule>
      </api:storeProductAccessRules>
   </soapenv:Body>
</soapenv:Envelope>
```
{% endtab %}

{% tab title="Example 2: Exclude Category" %}
**Goal:** Customer sees everything except Premium category

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
   <soapenv:Header/>
   <soapenv:Body>
      <api:storeProductAccessRules>
         <api:apiKey>your-api-key</api:apiKey>
         <api:productAccessRule>
            <api:customerNo>CUST-002</api:customerNo>
            <api:type>REMOVE</api:type>
            <api:operation>EQUALS</api:operation>
            <api:categoryCode>PREMIUM</api:categoryCode>
            <api:sortCode>1</api:sortCode>
         </api:productAccessRule>
      </api:storeProductAccessRules>
   </soapenv:Body>
</soapenv:Envelope>
```
{% endtab %}

{% tab title="Example 3: Season Restriction" %}
**Goal:** Customer only sees current and next season

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
   <soapenv:Header/>
   <soapenv:Body>
      <api:storeProductAccessRules>
         <api:apiKey>your-api-key</api:apiKey>
         <!-- Remove all -->
         <api:productAccessRule>
            <api:customerNo>CUST-003</api:customerNo>
            <api:type>REMOVE</api:type>
            <api:sortCode>1</api:sortCode>
         </api:productAccessRule>
         <!-- Add current season -->
         <api:productAccessRule>
            <api:customerNo>CUST-003</api:customerNo>
            <api:type>ADD</api:type>
            <api:seasonCode>FW24</api:seasonCode>
            <api:sortCode>2</api:sortCode>
         </api:productAccessRule>
         <!-- Add next season -->
         <api:productAccessRule>
            <api:customerNo>CUST-003</api:customerNo>
            <api:type>ADD</api:type>
            <api:seasonCode>SS25</api:seasonCode>
            <api:sortCode>3</api:sortCode>
         </api:productAccessRule>
      </api:storeProductAccessRules>
   </soapenv:Body>
</soapenv:Envelope>
```
{% endtab %}

{% tab title="Example 4: APP vs B2B" %}
**Goal:** Show premium only in APP, not B2B webstore

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
   <soapenv:Header/>
   <soapenv:Body>
      <api:storeProductAccessRules>
         <api:apiKey>your-api-key</api:apiKey>
         <!-- Remove premium from B2B -->
         <api:productAccessRule>
            <api:customerNo>CUST-004</api:customerNo>
            <api:type>REMOVE</api:type>
            <api:environment>B2B</api:environment>
            <api:categoryCode>PREMIUM</api:categoryCode>
            <api:sortCode>1</api:sortCode>
         </api:productAccessRule>
      </api:storeProductAccessRules>
   </soapenv:Body>
</soapenv:Envelope>
```
{% endtab %}
{% endtabs %}

---

## Customer Access for Agents

Define which customers a sales agent can access.

## storeFullCustomerAccessSet

Set customer access for all agents at once.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
   <soapenv:Header/>
   <soapenv:Body>
      <api:storeFullCustomerAccessSet>
         <api:apiKey>your-api-key</api:apiKey>
         <api:agentCustomerAccessRecord>
            <api:agentEmail>agent1@yourcompany.com</api:agentEmail>
            <api:agentDefaultCurrency>EUR</api:agentDefaultCurrency>
            <api:agentDefaultPriceGroup>RETAIL</api:agentDefaultPriceGroup>
            <api:customerAccessRecords>
               <api:customerNo>CUST-001</api:customerNo>
            </api:customerAccessRecords>
            <api:customerAccessRecords>
               <api:customerNo>CUST-002</api:customerNo>
            </api:customerAccessRecords>
            <api:customerAccessRecords>
               <api:customerNo>CUST-003</api:customerNo>
               <api:shippingLocationCode>STORE-A</api:shippingLocationCode>
            </api:customerAccessRecords>
         </api:agentCustomerAccessRecord>
         <api:agentCustomerAccessRecord>
            <api:agentEmail>agent2@yourcompany.com</api:agentEmail>
            <api:agentDefaultCurrency>USD</api:agentDefaultCurrency>
            <api:customerAccessRecords>
               <api:customerNo>CUST-004</api:customerNo>
            </api:customerAccessRecords>
            <api:customerAccessRecords>
               <api:customerNo>CUST-005</api:customerNo>
            </api:customerAccessRecords>
         </api:agentCustomerAccessRecord>
      </api:storeFullCustomerAccessSet>
   </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iIHhtbG5zOmFwaT0iaHR0cDovL2FwaS5jYy5zYWxlc2FwcC5hcHB0aXR1ZGUubmwvIj4KICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgPHNvYXBlbnY6Qm9keT4KICAgICAgPGFwaTpzdG9yZUZ1bGxDdXN0b21lckFjY2Vzc1NldD4KICAgICAgICAgPGFwaTphcGlLZXk+eW91ci1hcGkta2V5PC9hcGk6YXBpS2V5PgogICAgICAgICA8YXBpOmFnZW50Q3VzdG9tZXJBY2Nlc3NSZWNvcmQ+CiAgICAgICAgICAgIDxhcGk6YWdlbnRFbWFpbD5hZ2VudDFAeW91cmNvbXBhbnkuY29tPC9hcGk6YWdlbnRFbWFpbD4KICAgICAgICAgICAgPGFwaTphZ2VudERlZmF1bHRDdXJyZW5jeT5FVVI8L2FwaTphZ2VudERlZmF1bHRDdXJyZW5jeT4KICAgICAgICAgICAgPGFwaTphZ2VudERlZmF1bHRQcmljZUdyb3VwPlJFVEFJTDwvYXBpOmFnZW50RGVmYXVsdFByaWNlR3JvdXA+CiAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJBY2Nlc3NSZWNvcmRzPgogICAgICAgICAgICAgICA8YXBpOmN1c3RvbWVyTm8+Q1VTVC0wMDE8L2FwaTpjdXN0b21lck5vPgogICAgICAgICAgICA8L2FwaTpjdXN0b21lckFjY2Vzc1JlY29yZHM+CiAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJBY2Nlc3NSZWNvcmRzPgogICAgICAgICAgICAgICA8YXBpOmN1c3RvbWVyTm8+Q1VTVC0wMDI8L2FwaTpjdXN0b21lck5vPgogICAgICAgICAgICA8L2FwaTpjdXN0b21lckFjY2Vzc1JlY29yZHM+CiAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJBY2Nlc3NSZWNvcmRzPgogICAgICAgICAgICAgICA8YXBpOmN1c3RvbWVyTm8+Q1VTVC0wMDM8L2FwaTpjdXN0b21lck5vPgogICAgICAgICAgICAgICA8YXBpOnNoaXBwaW5nTG9jYXRpb25Db2RlPlNUT1JFLUE8L2FwaTpzaGlwcGluZ0xvY2F0aW9uQ29kZT4KICAgICAgICAgICAgPC9hcGk6Y3VzdG9tZXJBY2Nlc3NSZWNvcmRzPgogICAgICAgICA8L2FwaTphZ2VudEN1c3RvbWVyQWNjZXNzUmVjb3JkPgogICAgICAgICA8YXBpOmFnZW50Q3VzdG9tZXJBY2Nlc3NSZWNvcmQ+CiAgICAgICAgICAgIDxhcGk6YWdlbnRFbWFpbD5hZ2VudDJAeW91cmNvbXBhbnkuY29tPC9hcGk6YWdlbnRFbWFpbD4KICAgICAgICAgICAgPGFwaTphZ2VudERlZmF1bHRDdXJyZW5jeT5VU0Q8L2FwaTphZ2VudERlZmF1bHRDdXJyZW5jeT4KICAgICAgICAgICAgPGFwaTpjdXN0b21lckFjY2Vzc1JlY29yZHM+CiAgICAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJObz5DVVNULTAwNDwvYXBpOmN1c3RvbWVyTm8+CiAgICAgICAgICAgIDwvYXBpOmN1c3RvbWVyQWNjZXNzUmVjb3Jkcz4KICAgICAgICAgICAgPGFwaTpjdXN0b21lckFjY2Vzc1JlY29yZHM+CiAgICAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJObz5DVVNULTAwNTwvYXBpOmN1c3RvbWVyTm8+CiAgICAgICAgICAgIDwvYXBpOmN1c3RvbWVyQWNjZXNzUmVjb3Jkcz4KICAgICAgICAgPC9hcGk6YWdlbnRDdXN0b21lckFjY2Vzc1JlY29yZD4KICAgICAgPC9hcGk6c3RvcmVGdWxsQ3VzdG9tZXJBY2Nlc3NTZXQ+CiAgIDwvc29hcGVudjpCb2R5Pgo8L3NvYXBlbnY6RW52ZWxvcGU+)

---

## storeFullCustomerAccessSetForAgent

Set customer access for a specific agent.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:api="http://api.cc.salesapp.apptitude.nl/">
   <soapenv:Header/>
   <soapenv:Body>
      <api:storeFullCustomerAccessSetForAgent>
         <api:apiKey>your-api-key</api:apiKey>
         <api:agentEmail>agent1@yourcompany.com</api:agentEmail>
         <api:agentDefaultPriceGroup>RETAIL</api:agentDefaultPriceGroup>
         <api:agentDefaultCurrency>EUR</api:agentDefaultCurrency>
         <api:customerAccessRecord>
            <api:customerNo>CUST-001</api:customerNo>
         </api:customerAccessRecord>
         <api:customerAccessRecord>
            <api:customerNo>CUST-002</api:customerNo>
         </api:customerAccessRecord>
         <api:customerAccessRecord>
            <api:customerNo>CUST-003</api:customerNo>
            <api:shippingLocationCode>MAIN</api:shippingLocationCode>
         </api:customerAccessRecord>
      </api:storeFullCustomerAccessSetForAgent>
   </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iIHhtbG5zOmFwaT0iaHR0cDovL2FwaS5jYy5zYWxlc2FwcC5hcHB0aXR1ZGUubmwvIj4KICAgPHNvYXBlbnY6SGVhZGVyLz4KICAgPHNvYXBlbnY6Qm9keT4KICAgICAgPGFwaTpzdG9yZUZ1bGxDdXN0b21lckFjY2Vzc1NldEZvckFnZW50PgogICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgIDxhcGk6YWdlbnRFbWFpbD5hZ2VudDFAeW91cmNvbXBhbnkuY29tPC9hcGk6YWdlbnRFbWFpbD4KICAgICAgICAgPGFwaTphZ2VudERlZmF1bHRQcmljZUdyb3VwPlJFVEFJTDwvYXBpOmFnZW50RGVmYXVsdFByaWNlR3JvdXA+CiAgICAgICAgIDxhcGk6YWdlbnREZWZhdWx0Q3VycmVuY3k+RVVSPC9hcGk6YWdlbnREZWZhdWx0Q3VycmVuY3k+CiAgICAgICAgIDxhcGk6Y3VzdG9tZXJBY2Nlc3NSZWNvcmQ+CiAgICAgICAgICAgIDxhcGk6Y3VzdG9tZXJObz5DVVNULTAwMTwvYXBpOmN1c3RvbWVyTm8+CiAgICAgICAgIDwvYXBpOmN1c3RvbWVyQWNjZXNzUmVjb3JkPgogICAgICAgICA8YXBpOmN1c3RvbWVyQWNjZXNzUmVjb3JkPgogICAgICAgICAgICA8YXBpOmN1c3RvbWVyTm8+Q1VTVC0wMDI8L2FwaTpjdXN0b21lck5vPgogICAgICAgICA8L2FwaTpjdXN0b21lckFjY2Vzc1JlY29yZD4KICAgICAgICAgPGFwaTpjdXN0b21lckFjY2Vzc1JlY29yZD4KICAgICAgICAgICAgPGFwaTpjdXN0b21lck5vPkNVU1QtMDAzPC9hcGk6Y3VzdG9tZXJObz4KICAgICAgICAgICAgPGFwaTpzaGlwcGluZ0xvY2F0aW9uQ29kZT5NQUlOPC9hcGk6c2hpcHBpbmdMb2NhdGlvbkNvZGU+CiAgICAgICAgIDwvYXBpOmN1c3RvbWVyQWNjZXNzUmVjb3JkPgogICAgICA8L2FwaTpzdG9yZUZ1bGxDdXN0b21lckFjY2Vzc1NldEZvckFnZW50PgogICA8L3NvYXBlbnY6Qm9keT4KPC9zb2FwZW52OkVudmVsb3BlPg==)

---

## Customer Access Record Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `customerNo` | String | Yes | Customer identifier |
| `shippingLocationCode` | String | No | Restrict to specific shipping location |

---

## Best Practices

{% tabs %}
{% tab title="Product Access" %}
- Use `storeFullProductAccessSet` for nightly sync
- Always set `sortCode` for predictable rule evaluation
- Test complex rule combinations before production
- Document your rule logic for maintenance
{% endtab %}

{% tab title="Agent Access" %}
- Define territories clearly using customer access
- Use `agentDefaultPriceGroup` for browsing without customer
- Restrict shipping locations for agents with limited territory
{% endtab %}

{% tab title="Performance" %}
- Minimize the number of rules when possible
- Use broader rules (brand/category) over individual product rules
- Batch rule updates rather than single-rule calls
{% endtab %}
{% endtabs %}
