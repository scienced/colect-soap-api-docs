# Access Rules

Control which products are visible to which customers, and which customers are accessible to which sales agents. Access rules enable personalized catalogs and territory management.

{% hint style="success" %}
**Full Examples:**

* [Download storeProductAccessRules.xml](https://assets.apptitude.nl/KnowledgeBase/Webservice_3_0/External/storeProductAccessRules.xml) - Product access rules example
* [Download storeFullCustomerAccessSetForAgent.xml](https://assets.apptitude.nl/KnowledgeBase/Webservice_3_0/External/storeFullCustomerAccessSetForAgent.xml) - Agent customer access example
{% endhint %}

***

## Operations Overview

### Product Access Rules

| Operation                                                                                    | Description                                  |
| -------------------------------------------------------------------------------------------- | -------------------------------------------- |
| [`storeFullProductAccessSet`](access-rules.md#storefullproductaccessset)                     | Replace all product access rules (full sync) |
| [`storeProductAccessRules`](access-rules.md#storeproductaccessrules)                         | Add or update product access rules           |
| [`deleteProductAccessRules`](access-rules.md#deleteproductaccessrules)                       | Remove specific access rules                 |
| [`deleteProductAccessRulesForCustomer`](access-rules.md#deleteproductaccessrulesforcustomer) | Remove all rules for a customer              |
| [`deleteAllProductAccessRules`](access-rules.md#deleteallproductaccessrules)                 | Clear all access rules                       |

### Customer Access Rules

| Operation                                                                                  | Description                                  |
| ------------------------------------------------------------------------------------------ | -------------------------------------------- |
| [`storeFullCustomerAccessSet`](access-rules.md#storefullcustomeraccessset)                 | Define which customers each agent can access |
| [`storeFullCustomerAccessSetForAgent`](access-rules.md#storefullcustomeraccesssetforagent) | Define access for a specific agent           |

***

## Product Access Fundamentals

By default, all customers can see all products. Product access rules allow you to:

* Restrict products to specific customers
* Exclude products from specific customers
* Filter by brand, category, season, or individual product

{% hint style="info" %}
**Rule Order Matters:** Rules are evaluated sequentially. The order of rules determines the final access result.
{% endhint %}

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNfERJJoAS1qNmbVjoGdOAkBAwwLUMdCgNtDAK4ATGALgZVAPgBQOIGgcpRIABIwEO4SrP6BOMEUiEgAQmCuAJ5x8UE6ZAySMABizhgYAAoS6c6cDACCptYgAMowDNk58cB5OgDSMFkZYM4SALQ6owDWA8BGUNpa8-1ZAZ1dAISjozgAws4gBQQwErsAqs0AKqMADNcAjDicEHA4YF4ZOCAwMDgpAEp1AByABFRnUcFAqq4agwQDgth01t15khIdVag1OE0-qUYIi1rkUZx9odjoCwD4ducrrc7rM8sSDmAjhJyfiCciFgwMrAfH8UABZADyADUUPSUdzeasCV08soJBBHG9KULARc6gBJQHNCULBVK7QqmWyhJ5cASBg7dJ4ulzBYWq029lde2oqEwzHY3EuwkLNHQjGNEAgHFWX05TlIRmk1kUqmXG73PXRknMskUk2yqNSvF1YHAlO5iNylEG5VwHwoACKpzqABldW7y0bK1mOXkAEaKuCuTWuHz-IGguop7vPPsD9tI82SJ3uHwAJhTjutC+nQTdAc9wdDPo3CU22z2TJZZ0Tt0Xj2en2+OBgCGOGQYdG0cCk97wWIcOHK-IFmqnAK14MDAUiSB8CIHlG25BliIZhniB6lgsMbpnGlLUkmy5umhLJsshfpIMW-6iuKbrFoRZplrAioVlWtYNk28q0Yaxqmih0ZKmBEFrnif6CoBAopk8oHgRIGR8SWRGrs6dqzpaUnQVuHpwd64btiuBQSMUpQVKp9S7q07QyrMoihGkmT+GZITJGgj5YLyQA)

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNfERJJoAS1qNmbVjoGdOAkBAwwLUMdCgNtDAK4ATGALgZVAPgBQOIGgcpRIABIwEO4SrP6BOMEUiEgAQmCuAJ5x8UE6ZAySMAAKEunOnAwAgqbWIABKzlYg2TnxwHk6ANIwWRlgzhIAtDqDANY9wEZQ2lrT3VkBrW15UKWu5VU1IPWNMC1Ly9NInM4gBQQwEgByYD4AwgCqAMoAKoMADO8AzJN5J2dgC7XW6LA6HGYMDKwHyVAAisN+R0h0NBYISeWUEggjjAcB8KAAig9KgAZJ6ImaY7HaXH7MHtI4AIyxcFcAElXPiABp3EnPNkANRQgxSdUqVwRUxmzIgrI5dIODJm4AkDDu6T2AEYKWRJGqNQq2lKkKsyhVqpxag0rAqdQCJMU1hsLVbds1QZNRKE0pl-J6Qsk0AglNCgA)

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNfERJJoAS1qNmbVjoGdOAkBAwwLUMdCgNtDAK4ATGALgZVAPgBQOIGgcpRIABIwEO4SrP6BOMEUiEgAQmCuAJ5x8UE6SO5WDDAAChLpzpwMAIKm1iAASs5WINk58cB5OgDSMFkZYM4SALQ6QwDWvcBGUNpaMz1ZAW3teVBlrhXVtSANTTCtyyszSJzOIAxgBDASAHJgPgDCAKoAygAqQwAMnwDMU3mnc6Xa53A6HBJ5ABGEggcFcAElXD4UAANB4AGVe8IAaighil6lUbgARf7HaGwhFIpaHMmzNblSo1Th1RpWMEJaazAowIqlRlbFk7NnWVpTUShNKZfzikLJNAIJSwHxAA)

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNfERJJoAS1qNmbVjoGdOAkBAwwLUMdCgNtDAK4ATGALgZVAPgBQOIGgcpRIABIwEO4SrP6BOMEUiEgAQmCuAJ5x8UE6SO5WDDAAChLpzpwMAIKm1iAASs5WIABikgDCziAMYAQwEtk58cB5OgDSMFkZYM4SALQ6cwDWk8BGUNpaGxNZAUPDeZxdPX0SAHJgPu0AqgDKACpzAAxPAIxrh8e9-ReDw+ubAowIqlcqVGqcOqNZptCSdbrfAZ7BKyJLINKZfxrUShNAIJSwHxAA)

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNfERJJoAS1qNmbVjoGdOAkBAwwLUMdCgNtDAK4ATGALgZVAPgBQOIGgcpRIABIwEO4SrP6BOMEUiEgAQmCuAJ5x8UE6SO5WDDAAghgYAAoS6c6cDMWm1iAASs5WINk58cB5OgDSMFkZYM4SALQ6owDWA8BGUNpa8-1ZAZ2zeQUwRaUVVa41dQ0gza3WHbOioWmZ-hchyWgISrA+QA)

***

## Product Access Rule Fields

| Field              | Type    | Required | Description                        |
| ------------------ | ------- | -------- | ---------------------------------- |
| `customerNo`       | String  | Yes      | Customer to apply rule to          |
| `type`             | Enum    | No       | `ADD` (default) or `REMOVE`        |
| `operation`        | Enum    | No       | `EQUALS` (default) or `CONTAINS`   |
| `environment`      | Enum    | No       | `APP`, `B2B`, or omit for both     |
| `uniqueId`         | String  | No       | Specific product ID                |
| `colorCode`        | String  | No       | Specific color (requires uniqueId) |
| `categoryCode`     | String  | No       | Product category                   |
| `productGroupCode` | String  | No       | Product group                      |
| `brandId`          | String  | No       | Brand identifier                   |
| `seasonCode`       | String  | No       | Season code                        |
| `sortCode`         | Integer | No       | Rule evaluation order              |

### Rule Type

| Type     | Description                       |
| -------- | --------------------------------- |
| `ADD`    | Grant access to matching products |
| `REMOVE` | Deny access to matching products  |

### Match Operation

| Operation  | Description           |
| ---------- | --------------------- |
| `EQUALS`   | Exact match (default) |
| `CONTAINS` | Substring match       |

***

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

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNfERJJoAS1qNmbVjoGdOAkBAwwLUMdCgNtDAK4ATGALgZVAPgBQOIGgcpRIABIwEO4SrP6BOMEUiEgAQmCuAJ5x8UE6ZAySMABizhgYAMLOIAUEMBIAgqbWIADKMAzZOfHAeToA0jBZGWDOEgC0OmMA1oPARlDaWgsDWQFd3b1S8AyV1WC1DU0gIABKMJySrp3rGwtaW3AMKHzaGD4QDwwAjAACw6MXAhQCBwDImfZzTbbZ4QV7XG4JKGPAAiMAAZhBSjtRhJ4JwsigAKonSF3D7bVEYrGVCS4uD4+E3Hpkz6UzEYBgABQk2k4MAA4hIRlAfCcUAAVeoASQAMqTFuSUej2VyeXzBcLGetmYtOFUanVGnzjmcLhJXCAtQidUg9XsDgA5MA+cqElrisYABk9X3ltv1+zqTqtOT9doNh2Np3Ol0tawRuTu4cDkeaptjIe1eWTjudrvdXs9ACYwwHc5mgvNdWXDUdo2aLRXEUma6mTTHzXGE1mW-ag3m3R7vQBmUt9iTB+PdxOLEB0bS2bRwKQysCcCCOMBwcrpGA+d0AeTFY3qfrnC4Wy9X683293TbHEaNaY7janXT9iux4+f7YbIZtL9difOt03NB8kSeF43i-Is-hGCRAWBUFwQIT9PhhOF3yZSC2WpHE8SyN1kXQillXw2lCIg3sQKjMC32nUNs1bYN8yHT0ABZHxTSdGO4g5f3rDNsJ7asf1A18u0Ymd-XHVjB0LABWfj+wfKtZNol8Gykpl1KA1tBPoxkzwKXESjKYCU0EtoOnjOZRFCNJMn8eyQmSNAECUWAfCAA)

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNfERJJoAS1qNmbVjoGdOAkBAwwLUMdCgNtDAK4ATGALgZVAPgBQOIGgcpRIABIwEO4SrP6BOMEUiEgAQmCuAJ5x8UE6ZAySMABizhgYAMLOIAUEMBIAgqbWIADKMAxFkvVS8AzZOfHAeToA0jBZGWDOEgC0OjMA1uPARlDaWmtjWQEDg8M9cAwofNoYPhAHDACMAAKT05xgBFAQcBkmTyv7vccQp-27IZrLSXAAiMAAZhBSgwAAoSbScGAAcQkUygPgASigACr1ACSABkvsCLr1wVCYfDESi0c4MTtdglvocKdCMAxKhIJPBOFkUABVTEk9Zk1mQ9mc6Y8uB8gEDIHrThVGp1RpIkAgTEwR4SVzypmKpDK6pPOoAOTAPnKApaOJmAAYHVcRcaVWaJJaDYNVkr3bUGk1NdrdfrGYC8ibVYGNVqdZIw0yFZH-RarTa7Y6HQAmV1Rj1e8MK31u00B9XNEMJ725YH58tBuOhms5I31tPW232p0AZjzqc9VqLhryIDo2ls2jgUkJYE4EEcYDg5XSMB8AFkCebXWOJ2tp7P54vl6uW-2y2rG1W9QadwUeSUypULzHmm0Ol1Lv0VqJQmlMv4P4hMkaAIEosA+EAA)

***

## Customer Access Record Fields

<table><thead><tr><th width="196.0859375">Field</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody><tr><td><code>customerNo</code></td><td>String</td><td>Yes</td><td>Customer identifier</td></tr><tr><td><code>shippingLocationCode</code></td><td>String</td><td>No</td><td>Restrict to specific shipping location</td></tr></tbody></table>

***

## Best Practices

{% tabs %}
{% tab title="Product Access" %}
* Use `storeFullProductAccessSet` for nightly sync
* Always set `sortCode` for predictable rule evaluation
* Test complex rule combinations before production
* Document your rule logic for maintenance
{% endtab %}

{% tab title="Agent Access" %}
* Define territories clearly using customer access
* Use `agentDefaultPriceGroup` for browsing without customer
* Restrict shipping locations for agents with limited territory
{% endtab %}

{% tab title="Performance" %}
* Minimize the number of rules when possible
* Use broader rules (brand/category) over individual product rules
* Batch rule updates rather than single-rule calls
{% endtab %}
{% endtabs %}
