# Product Operations

Manage your product catalog in Colect. Products are the core of your B2B e-commerce platform, containing all information your customers need to place orders.

{% hint style="success" %}
**Full Example:** [Download complete storeProduct.xml](https://assets.apptitude.nl/KnowledgeBase/Webservice_3_0/External/storeProduct.xml) - A comprehensive example showing all available product fields.
{% endhint %}

---

## Operations Overview

| Operation | Type | Description |
|-----------|------|-------------|
| [`storeProducts`](#storeproducts) | Partial Sync | Add or update specific products |
| [`storeFullProductSet`](#storefullproductset) | Full Sync | Replace entire product catalog |
| [`deleteProducts`](#deleteproducts) | Delete | Remove specific products |
| [`deleteAllProducts`](#deleteallproducts) | Delete | Clear all products |
| [`getAllProducts`](#getallproducts) | Read | Retrieve all products |
| [`updateStock`](#updatestock) | Partial Update | Update stock levels only |
| [`updatePrices`](#updateprices) | Partial Update | Update prices only |
| [`updateExtraFields`](#updateextrafields) | Partial Update | Update extra fields only |

---

## storeProducts

Add new products or update existing ones. Products not included in the request remain unchanged.

{% hint style="info" %}
Products are uniquely identified by the combination of `uniqueId` + `colorCode`. Sending a product with an existing combination will update that product.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeProducts>
            <api:apiKey>your-api-key</api:apiKey>
            <api:product>
                <!-- Product 1 -->
                <api:uniqueId>STYLE-001</api:uniqueId>
                <api:colorCode>BLK</api:colorCode>
                <api:name>Classic T-Shirt</api:name>
                <api:description>Premium cotton t-shirt</api:description>
                <api:colorDesc>Black</api:colorDesc>
                <api:brandName>Your Brand</api:brandName>
                <api:brandId>BRAND01</api:brandId>
                <api:categoryCode>TSHIRTS</api:categoryCode>
                <api:categoryDesc>T-Shirts</api:categoryDesc>
                <api:seasonCode>SS25</api:seasonCode>
                <api:seasonDesc>Spring/Summer 2025</api:seasonDesc>
                <api:status>ACTIVE</api:status>
                <!-- Pricing -->
                <api:price>
                    <api:currencyCode>EUR</api:currencyCode>
                    <api:wholesalePrice>25.00</api:wholesalePrice>
                    <api:retailPrice>49.95</api:retailPrice>
                    <api:net>false</api:net>
                </api:price>
                <!-- Sizes with stock -->
                <api:size>
                    <api:name>S</api:name>
                    <api:sortCode>1</api:sortCode>
                    <api:eanCode>8712345678901</api:eanCode>
                    <api:stockLevel>
                        <api:quantity>100</api:quantity>
                    </api:stockLevel>
                </api:size>
                <api:size>
                    <api:name>M</api:name>
                    <api:sortCode>2</api:sortCode>
                    <api:eanCode>8712345678902</api:eanCode>
                    <api:stockLevel>
                        <api:quantity>150</api:quantity>
                    </api:stockLevel>
                </api:size>
                <!-- Images -->
                <api:medium>
                    <api:type>IMAGE_PRIMARY</api:type>
                    <api:url>https://cdn.example.com/style-001-blk-front.jpg</api:url>
                </api:medium>
                <api:medium>
                    <api:type>IMAGE_BACK</api:type>
                    <api:url>https://cdn.example.com/style-001-blk-back.jpg</api:url>
                    <api:sortCode>1</api:sortCode>
                </api:medium>
            </api:product>
            <api:product>
                <!-- Product 2 -->
                <api:uniqueId>STYLE-001</api:uniqueId>
                <api:colorCode>WHT</api:colorCode>
                <api:name>Classic T-Shirt</api:name>
                <api:colorDesc>White</api:colorDesc>
                <!-- ... additional fields ... -->
            </api:product>
        </api:storeProducts>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOnN0b3JlUHJvZHVjdHM+CiAgICAgICAgICAgIDxhcGk6YXBpS2V5PnlvdXItYXBpLWtleTwvYXBpOmFwaUtleT4KICAgICAgICAgICAgPGFwaTpwcm9kdWN0PgogICAgICAgICAgICAgICAgPCEtLSBQcm9kdWN0IDEgLS0+CiAgICAgICAgICAgICAgICA8YXBpOnVuaXF1ZUlkPlNUWUxFLTAwMTwvYXBpOnVuaXF1ZUlkPgogICAgICAgICAgICAgICAgPGFwaTpjb2xvckNvZGU+QkxLPC9hcGk6Y29sb3JDb2RlPgogICAgICAgICAgICAgICAgPGFwaTpuYW1lPkNsYXNzaWMgVC1TaGlydDwvYXBpOm5hbWU+CiAgICAgICAgICAgICAgICA8YXBpOmRlc2NyaXB0aW9uPlByZW1pdW0gY290dG9uIHQtc2hpcnQ8L2FwaTpkZXNjcmlwdGlvbj4KICAgICAgICAgICAgICAgIDxhcGk6Y29sb3JEZXNjPkJsYWNrPC9hcGk6Y29sb3JEZXNjPgogICAgICAgICAgICAgICAgPGFwaTpicmFuZE5hbWU+WW91ciBCcmFuZDwvYXBpOmJyYW5kTmFtZT4KICAgICAgICAgICAgICAgIDxhcGk6YnJhbmRJZD5CUkFORDAxPC9hcGk6YnJhbmRJZD4KICAgICAgICAgICAgICAgIDxhcGk6Y2F0ZWdvcnlDb2RlPlRTSElSVFM8L2FwaTpjYXRlZ29yeUNvZGU+CiAgICAgICAgICAgICAgICA8YXBpOmNhdGVnb3J5RGVzYz5ULVNoaXJ0czwvYXBpOmNhdGVnb3J5RGVzYz4KICAgICAgICAgICAgICAgIDxhcGk6c2Vhc29uQ29kZT5TUzI1PC9hcGk6c2Vhc29uQ29kZT4KICAgICAgICAgICAgICAgIDxhcGk6c2Vhc29uRGVzYz5TcHJpbmcvU3VtbWVyIDIwMjU8L2FwaTpzZWFzb25EZXNjPgogICAgICAgICAgICAgICAgPGFwaTpzdGF0dXM+QUNUSVZFPC9hcGk6c3RhdHVzPgogICAgICAgICAgICAgICAgPCEtLSBQcmljaW5nIC0tPgogICAgICAgICAgICAgICAgPGFwaTpwcmljZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOmN1cnJlbmN5Q29kZT5FVVI8L2FwaTpjdXJyZW5jeUNvZGU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTp3aG9sZXNhbGVQcmljZT4yNS4wMDwvYXBpOndob2xlc2FsZVByaWNlPgogICAgICAgICAgICAgICAgICAgIDxhcGk6cmV0YWlsUHJpY2U+NDkuOTU8L2FwaTpyZXRhaWxQcmljZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOm5ldD5mYWxzZTwvYXBpOm5ldD4KICAgICAgICAgICAgICAgIDwvYXBpOnByaWNlPgogICAgICAgICAgICAgICAgPCEtLSBTaXplcyB3aXRoIHN0b2NrIC0tPgogICAgICAgICAgICAgICAgPGFwaTpzaXplPgogICAgICAgICAgICAgICAgICAgIDxhcGk6bmFtZT5TPC9hcGk6bmFtZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnNvcnRDb2RlPjE8L2FwaTpzb3J0Q29kZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOmVhbkNvZGU+ODcxMjM0NTY3ODkwMTwvYXBpOmVhbkNvZGU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpzdG9ja0xldmVsPgogICAgICAgICAgICAgICAgICAgICAgICA8YXBpOnF1YW50aXR5PjEwMDwvYXBpOnF1YW50aXR5PgogICAgICAgICAgICAgICAgICAgIDwvYXBpOnN0b2NrTGV2ZWw+CiAgICAgICAgICAgICAgICA8L2FwaTpzaXplPgogICAgICAgICAgICAgICAgPGFwaTpzaXplPgogICAgICAgICAgICAgICAgICAgIDxhcGk6bmFtZT5NPC9hcGk6bmFtZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnNvcnRDb2RlPjI8L2FwaTpzb3J0Q29kZT4KICAgICAgICAgICAgICAgICAgICA8YXBpOmVhbkNvZGU+ODcxMjM0NTY3ODkwMjwvYXBpOmVhbkNvZGU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpzdG9ja0xldmVsPgogICAgICAgICAgICAgICAgICAgICAgICA8YXBpOnF1YW50aXR5PjE1MDwvYXBpOnF1YW50aXR5PgogICAgICAgICAgICAgICAgICAgIDwvYXBpOnN0b2NrTGV2ZWw+CiAgICAgICAgICAgICAgICA8L2FwaTpzaXplPgogICAgICAgICAgICAgICAgPCEtLSBJbWFnZXMgLS0+CiAgICAgICAgICAgICAgICA8YXBpOm1lZGl1bT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnR5cGU+SU1BR0VfUFJJTUFSWTwvYXBpOnR5cGU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTp1cmw+aHR0cHM6Ly9jZG4uZXhhbXBsZS5jb20vc3R5bGUtMDAxLWJsay1mcm9udC5qcGc8L2FwaTp1cmw+CiAgICAgICAgICAgICAgICA8L2FwaTptZWRpdW0+CiAgICAgICAgICAgICAgICA8YXBpOm1lZGl1bT4KICAgICAgICAgICAgICAgICAgICA8YXBpOnR5cGU+SU1BR0VfQkFDSzwvYXBpOnR5cGU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTp1cmw+aHR0cHM6Ly9jZG4uZXhhbXBsZS5jb20vc3R5bGUtMDAxLWJsay1iYWNrLmpwZzwvYXBpOnVybD4KICAgICAgICAgICAgICAgICAgICA8YXBpOnNvcnRDb2RlPjE8L2FwaTpzb3J0Q29kZT4KICAgICAgICAgICAgICAgIDwvYXBpOm1lZGl1bT4KICAgICAgICAgICAgPC9hcGk6cHJvZHVjdD4KICAgICAgICAgICAgPGFwaTpwcm9kdWN0PgogICAgICAgICAgICAgICAgPCEtLSBQcm9kdWN0IDIgLS0+CiAgICAgICAgICAgICAgICA8YXBpOnVuaXF1ZUlkPlNUWUxFLTAwMTwvYXBpOnVuaXF1ZUlkPgogICAgICAgICAgICAgICAgPGFwaTpjb2xvckNvZGU+V0hUPC9hcGk6Y29sb3JDb2RlPgogICAgICAgICAgICAgICAgPGFwaTpuYW1lPkNsYXNzaWMgVC1TaGlydDwvYXBpOm5hbWU+CiAgICAgICAgICAgICAgICA8YXBpOmNvbG9yRGVzYz5XaGl0ZTwvYXBpOmNvbG9yRGVzYz4KICAgICAgICAgICAgICAgIDwhLS0gLi4uIGFkZGl0aW9uYWwgZmllbGRzIC4uLiAtLT4KICAgICAgICAgICAgPC9hcGk6cHJvZHVjdD4KICAgICAgICA8L2FwaTpzdG9yZVByb2R1Y3RzPgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4K)

### Response

```xml
<ns2:storeProductsResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
```

---

## storeFullProductSet

Replace your entire product catalog. Products not included in this request will be **deleted**.

{% hint style="warning" %}
**Caution:** This is a destructive operation. All existing products not in the request will be removed.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeFullProductSet>
            <api:apiKey>your-api-key</api:apiKey>
            <api:product>
                <!-- Include ALL products you want to keep -->
                <api:uniqueId>STYLE-001</api:uniqueId>
                <api:colorCode>BLK</api:colorCode>
                <!-- ... full product data ... -->
            </api:product>
            <api:product>
                <api:uniqueId>STYLE-002</api:uniqueId>
                <api:colorCode>NVY</api:colorCode>
                <!-- ... full product data ... -->
            </api:product>
            <!-- ... all other products ... -->
        </api:storeFullProductSet>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOnN0b3JlRnVsbFByb2R1Y3RTZXQ+CiAgICAgICAgICAgIDxhcGk6YXBpS2V5PnlvdXItYXBpLWtleTwvYXBpOmFwaUtleT4KICAgICAgICAgICAgPGFwaTpwcm9kdWN0PgogICAgICAgICAgICAgICAgPCEtLSBJbmNsdWRlIEFMTCBwcm9kdWN0cyB5b3Ugd2FudCB0byBrZWVwIC0tPgogICAgICAgICAgICAgICAgPGFwaTp1bmlxdWVJZD5TVFlMRS0wMDE8L2FwaTp1bmlxdWVJZD4KICAgICAgICAgICAgICAgIDxhcGk6Y29sb3JDb2RlPkJMSzwvYXBpOmNvbG9yQ29kZT4KICAgICAgICAgICAgICAgIDwhLS0gLi4uIGZ1bGwgcHJvZHVjdCBkYXRhIC4uLiAtLT4KICAgICAgICAgICAgPC9hcGk6cHJvZHVjdD4KICAgICAgICAgICAgPGFwaTpwcm9kdWN0PgogICAgICAgICAgICAgICAgPGFwaTp1bmlxdWVJZD5TVFlMRS0wMDI8L2FwaTp1bmlxdWVJZD4KICAgICAgICAgICAgICAgIDxhcGk6Y29sb3JDb2RlPk5WWTwvYXBpOmNvbG9yQ29kZT4KICAgICAgICAgICAgICAgIDwhLS0gLi4uIGZ1bGwgcHJvZHVjdCBkYXRhIC4uLiAtLT4KICAgICAgICAgICAgPC9hcGk6cHJvZHVjdD4KICAgICAgICAgICAgPCEtLSAuLi4gYWxsIG90aGVyIHByb2R1Y3RzIC4uLiAtLT4KICAgICAgICA8L2FwaTpzdG9yZUZ1bGxQcm9kdWN0U2V0PgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4=)

### Use Cases

- Initial data load when setting up your Colect integration
- Nightly full synchronization to ensure data consistency
- Bulk product catalog refresh

---

## deleteProducts

Remove specific products from the catalog.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:deleteProducts>
            <api:apiKey>your-api-key</api:apiKey>
            <api:product>
                <api:uniqueId>STYLE-001</api:uniqueId>
                <api:colorCode>BLK</api:colorCode>
            </api:product>
            <api:product>
                <api:uniqueId>STYLE-002</api:uniqueId>
                <api:colorCode>NVY</api:colorCode>
            </api:product>
        </api:deleteProducts>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOmRlbGV0ZVByb2R1Y3RzPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgICAgIDxhcGk6cHJvZHVjdD4KICAgICAgICAgICAgICAgIDxhcGk6dW5pcXVlSWQ+U1RZTEUtMDAxPC9hcGk6dW5pcXVlSWQ+CiAgICAgICAgICAgICAgICA8YXBpOmNvbG9yQ29kZT5CTEs8L2FwaTpjb2xvckNvZGU+CiAgICAgICAgICAgIDwvYXBpOnByb2R1Y3Q+CiAgICAgICAgICAgIDxhcGk6cHJvZHVjdD4KICAgICAgICAgICAgICAgIDxhcGk6dW5pcXVlSWQ+U1RZTEUtMDAyPC9hcGk6dW5pcXVlSWQ+CiAgICAgICAgICAgICAgICA8YXBpOmNvbG9yQ29kZT5OVlk8L2FwaTpjb2xvckNvZGU+CiAgICAgICAgICAgIDwvYXBpOnByb2R1Y3Q+CiAgICAgICAgPC9hcGk6ZGVsZXRlUHJvZHVjdHM+CiAgICA8L3NvYXBlbnY6Qm9keT4KPC9zb2FwZW52OkVudmVsb3BlPg==)

---

## deleteAllProducts

Remove all products from the catalog.

{% hint style="danger" %}
**Warning:** This removes ALL products. Use with extreme caution.
{% endhint %}

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:deleteAllProducts>
            <api:apiKey>your-api-key</api:apiKey>
        </api:deleteAllProducts>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOmRlbGV0ZUFsbFByb2R1Y3RzPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgPC9hcGk6ZGVsZXRlQWxsUHJvZHVjdHM+CiAgICA8L3NvYXBlbnY6Qm9keT4KPC9zb2FwZW52OkVudmVsb3BlPg==)

---

## getAllProducts

Retrieve all products currently in your collection.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:getAllProducts>
            <api:apiKey>your-api-key</api:apiKey>
        </api:getAllProducts>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOmdldEFsbFByb2R1Y3RzPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgPC9hcGk6Z2V0QWxsUHJvZHVjdHM+CiAgICA8L3NvYXBlbnY6Qm9keT4KPC9zb2FwZW52OkVudmVsb3BlPg==)

### Response

```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <ns2:getAllProductsResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/">
            <ns2:return>
                <ns2:uniqueId>STYLE-001</ns2:uniqueId>
                <ns2:colorCode>BLK</ns2:colorCode>
                <ns2:name>Classic T-Shirt</ns2:name>
                <!-- ... all product fields ... -->
            </ns2:return>
            <ns2:return>
                <ns2:uniqueId>STYLE-002</ns2:uniqueId>
                <ns2:colorCode>NVY</ns2:colorCode>
                <!-- ... all product fields ... -->
            </ns2:return>
        </ns2:getAllProductsResponse>
    </soap:Body>
</soap:Envelope>
```

---

## updateStock

Update stock levels without sending full product data. Ideal for real-time inventory synchronization.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:updateStock>
            <api:apiKey>your-api-key</api:apiKey>
            <api:failOnUnknownProducts>true</api:failOnUnknownProducts>
            <api:stockInfoElement>
                <api:productUniqueId>STYLE-001</api:productUniqueId>
                <api:productColorCode>BLK</api:productColorCode>
                <api:sizeName>M</api:sizeName>
                <api:stockLevel>
                    <api:quantity>75</api:quantity>
                </api:stockLevel>
            </api:stockInfoElement>
            <api:stockInfoElement>
                <api:productUniqueId>STYLE-001</api:productUniqueId>
                <api:productColorCode>BLK</api:productColorCode>
                <api:sizeName>L</api:sizeName>
                <api:stockLevel>
                    <api:quantity>50</api:quantity>
                </api:stockLevel>
            </api:stockInfoElement>
        </api:updateStock>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOnVwZGF0ZVN0b2NrPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgICAgIDxhcGk6ZmFpbE9uVW5rbm93blByb2R1Y3RzPnRydWU8L2FwaTpmYWlsT25Vbmtub3duUHJvZHVjdHM+CiAgICAgICAgICAgIDxhcGk6c3RvY2tJbmZvRWxlbWVudD4KICAgICAgICAgICAgICAgIDxhcGk6cHJvZHVjdFVuaXF1ZUlkPlNUWUxFLTAwMTwvYXBpOnByb2R1Y3RVbmlxdWVJZD4KICAgICAgICAgICAgICAgIDxhcGk6cHJvZHVjdENvbG9yQ29kZT5CTEs8L2FwaTpwcm9kdWN0Q29sb3JDb2RlPgogICAgICAgICAgICAgICAgPGFwaTpzaXplTmFtZT5NPC9hcGk6c2l6ZU5hbWU+CiAgICAgICAgICAgICAgICA8YXBpOnN0b2NrTGV2ZWw+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpxdWFudGl0eT43NTwvYXBpOnF1YW50aXR5PgogICAgICAgICAgICAgICAgPC9hcGk6c3RvY2tMZXZlbD4KICAgICAgICAgICAgPC9hcGk6c3RvY2tJbmZvRWxlbWVudD4KICAgICAgICAgICAgPGFwaTpzdG9ja0luZm9FbGVtZW50PgogICAgICAgICAgICAgICAgPGFwaTpwcm9kdWN0VW5pcXVlSWQ+U1RZTEUtMDAxPC9hcGk6cHJvZHVjdFVuaXF1ZUlkPgogICAgICAgICAgICAgICAgPGFwaTpwcm9kdWN0Q29sb3JDb2RlPkJMSzwvYXBpOnByb2R1Y3RDb2xvckNvZGU+CiAgICAgICAgICAgICAgICA8YXBpOnNpemVOYW1lPkw8L2FwaTpzaXplTmFtZT4KICAgICAgICAgICAgICAgIDxhcGk6c3RvY2tMZXZlbD4KICAgICAgICAgICAgICAgICAgICA8YXBpOnF1YW50aXR5PjUwPC9hcGk6cXVhbnRpdHk+CiAgICAgICAgICAgICAgICA8L2FwaTpzdG9ja0xldmVsPgogICAgICAgICAgICA8L2FwaTpzdG9ja0luZm9FbGVtZW50PgogICAgICAgIDwvYXBpOnVwZGF0ZVN0b2NrPgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4K)

### Alternative: Using EAN Code

```xml
<api:stockInfoElement>
    <api:eanCode>8712345678902</api:eanCode>
    <api:stockLevel>
        <api:quantity>75</api:quantity>
    </api:stockLevel>
</api:stockInfoElement>
```

### Date-Based Stock Levels

For future deliveries, specify multiple stock levels with dates:

```xml
<api:stockInfoElement>
    <api:productUniqueId>STYLE-001</api:productUniqueId>
    <api:productColorCode>BLK</api:productColorCode>
    <api:sizeName>M</api:sizeName>
    <api:stockLevel>
        <api:quantity>50</api:quantity>
        <!-- Current stock -->
    </api:stockLevel>
    <api:stockLevel>
        <api:startDate>2025-02-01T00:00:00</api:startDate>
        <api:quantity>150</api:quantity>
        <!-- Expected stock from Feb 1 -->
    </api:stockLevel>
</api:stockInfoElement>
```

---

## updatePrices

Update prices without sending full product data.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:updatePrices>
            <api:apiKey>your-api-key</api:apiKey>
            <api:failOnUnknownProducts>true</api:failOnUnknownProducts>
            <api:priceInfoElement>
                <api:productUniqueId>STYLE-001</api:productUniqueId>
                <api:productColorCode>BLK</api:productColorCode>
                <api:currencyCode>EUR</api:currencyCode>
                <api:priceType>WHOLESALE</api:priceType>
                <api:price>27.50</api:price>
            </api:priceInfoElement>
            <api:priceInfoElement>
                <api:productUniqueId>STYLE-001</api:productUniqueId>
                <api:productColorCode>BLK</api:productColorCode>
                <api:currencyCode>EUR</api:currencyCode>
                <api:priceType>RETAIL</api:priceType>
                <api:price>54.95</api:price>
            </api:priceInfoElement>
        </api:updatePrices>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOnVwZGF0ZVByaWNlcz4KICAgICAgICAgICAgPGFwaTphcGlLZXk+eW91ci1hcGkta2V5PC9hcGk6YXBpS2V5PgogICAgICAgICAgICA8YXBpOmZhaWxPblVua25vd25Qcm9kdWN0cz50cnVlPC9hcGk6ZmFpbE9uVW5rbm93blByb2R1Y3RzPgogICAgICAgICAgICA8YXBpOnByaWNlSW5mb0VsZW1lbnQ+CiAgICAgICAgICAgICAgICA8YXBpOnByb2R1Y3RVbmlxdWVJZD5TVFlMRS0wMDE8L2FwaTpwcm9kdWN0VW5pcXVlSWQ+CiAgICAgICAgICAgICAgICA8YXBpOnByb2R1Y3RDb2xvckNvZGU+QkxLPC9hcGk6cHJvZHVjdENvbG9yQ29kZT4KICAgICAgICAgICAgICAgIDxhcGk6Y3VycmVuY3lDb2RlPkVVUjwvYXBpOmN1cnJlbmN5Q29kZT4KICAgICAgICAgICAgICAgIDxhcGk6cHJpY2VUeXBlPldIT0xFU0FMRTwvYXBpOnByaWNlVHlwZT4KICAgICAgICAgICAgICAgIDxhcGk6cHJpY2U+MjcuNTA8L2FwaTpwcmljZT4KICAgICAgICAgICAgPC9hcGk6cHJpY2VJbmZvRWxlbWVudD4KICAgICAgICAgICAgPGFwaTpwcmljZUluZm9FbGVtZW50PgogICAgICAgICAgICAgICAgPGFwaTpwcm9kdWN0VW5pcXVlSWQ+U1RZTEUtMDAxPC9hcGk6cHJvZHVjdFVuaXF1ZUlkPgogICAgICAgICAgICAgICAgPGFwaTpwcm9kdWN0Q29sb3JDb2RlPkJMSzwvYXBpOnByb2R1Y3RDb2xvckNvZGU+CiAgICAgICAgICAgICAgICA8YXBpOmN1cnJlbmN5Q29kZT5FVVI8L2FwaTpjdXJyZW5jeUNvZGU+CiAgICAgICAgICAgICAgICA8YXBpOnByaWNlVHlwZT5SRVRBSUw8L2FwaTpwcmljZVR5cGU+CiAgICAgICAgICAgICAgICA8YXBpOnByaWNlPjU0Ljk1PC9hcGk6cHJpY2U+CiAgICAgICAgICAgIDwvYXBpOnByaWNlSW5mb0VsZW1lbnQ+CiAgICAgICAgPC9hcGk6dXBkYXRlUHJpY2VzPgogICAgPC9zb2FwZW52OkJvZHk+Cjwvc29hcGVudjpFbnZlbG9wZT4K)

### Price Types

| Type | Description |
|------|-------------|
| `WHOLESALE` | B2B wholesale price |
| `RETAIL` | Suggested retail price |
| `ORIGINAL_WHOLESALE` | Original price (before discount) |
| `PURCHASE` | Purchase/cost price |

### Price Groups

Apply different prices to different customer groups:

```xml
<api:priceInfoElement>
    <api:productUniqueId>STYLE-001</api:productUniqueId>
    <api:productColorCode>BLK</api:productColorCode>
    <api:currencyCode>EUR</api:currencyCode>
    <api:priceGroup>VIP</api:priceGroup>
    <api:priceType>WHOLESALE</api:priceType>
    <api:price>22.00</api:price>
</api:priceInfoElement>
```

---

## updateExtraFields

Update custom extra fields without full product sync.

### Request

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:updateExtraFields>
            <api:apiKey>your-api-key</api:apiKey>
            <api:failOnUnknownProducts>true</api:failOnUnknownProducts>
            <api:extraFields>
                <api:uniqueId>STYLE-001</api:uniqueId>
                <api:colorCode>BLK</api:colorCode>
                <api:extraField>
                    <api:name>care_instructions</api:name>
                    <api:description>Care Instructions</api:description>
                    <api:value>Machine wash cold, tumble dry low</api:value>
                    <api:visible>true</api:visible>
                </api:extraField>
                <api:extraField>
                    <api:name>country_of_origin</api:name>
                    <api:description>Country of Origin</api:description>
                    <api:value>Portugal</api:value>
                    <api:visible>true</api:visible>
                </api:extraField>
            </api:extraFields>
        </api:updateExtraFields>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#xml=PHNvYXBlbnY6RW52ZWxvcGUgeG1sbnM6c29hcGVudj0iaHR0cDovL3NjaGVtYXMueG1sc29hcC5vcmcvc29hcC9lbnZlbG9wZS8iCiAgICB4bWxuczphcGk9Imh0dHA6Ly9hcGkuY2Muc2FsZXNhcHAuYXBwdGl0dWRlLm5sLyI+CiAgICA8c29hcGVudjpIZWFkZXIvPgogICAgPHNvYXBlbnY6Qm9keT4KICAgICAgICA8YXBpOnVwZGF0ZUV4dHJhRmllbGRzPgogICAgICAgICAgICA8YXBpOmFwaUtleT55b3VyLWFwaS1rZXk8L2FwaTphcGlLZXk+CiAgICAgICAgICAgIDxhcGk6ZmFpbE9uVW5rbm93blByb2R1Y3RzPnRydWU8L2FwaTpmYWlsT25Vbmtub3duUHJvZHVjdHM+CiAgICAgICAgICAgIDxhcGk6ZXh0cmFGaWVsZHM+CiAgICAgICAgICAgICAgICA8YXBpOnVuaXF1ZUlkPlNUWUxFLTAwMTwvYXBpOnVuaXF1ZUlkPgogICAgICAgICAgICAgICAgPGFwaTpjb2xvckNvZGU+QkxLPC9hcGk6Y29sb3JDb2RlPgogICAgICAgICAgICAgICAgPGFwaTpleHRyYUZpZWxkPgogICAgICAgICAgICAgICAgICAgIDxhcGk6bmFtZT5jYXJlX2luc3RydWN0aW9uczwvYXBpOm5hbWU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTpkZXNjcmlwdGlvbj5DYXJlIEluc3RydWN0aW9uczwvYXBpOmRlc2NyaXB0aW9uPgogICAgICAgICAgICAgICAgICAgIDxhcGk6dmFsdWU+TWFjaGluZSB3YXNoIGNvbGQsIHR1bWJsZSBkcnkgbG93PC9hcGk6dmFsdWU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTp2aXNpYmxlPnRydWU8L2FwaTp2aXNpYmxlPgogICAgICAgICAgICAgICAgPC9hcGk6ZXh0cmFGaWVsZD4KICAgICAgICAgICAgICAgIDxhcGk6ZXh0cmFGaWVsZD4KICAgICAgICAgICAgICAgICAgICA8YXBpOm5hbWU+Y291bnRyeV9vZl9vcmlnaW48L2FwaTpuYW1lPgogICAgICAgICAgICAgICAgICAgIDxhcGk6ZGVzY3JpcHRpb24+Q291bnRyeSBvZiBPcmlnaW48L2FwaTpkZXNjcmlwdGlvbj4KICAgICAgICAgICAgICAgICAgICA8YXBpOnZhbHVlPlBvcnR1Z2FsPC9hcGk6dmFsdWU+CiAgICAgICAgICAgICAgICAgICAgPGFwaTp2aXNpYmxlPnRydWU8L2FwaTp2aXNpYmxlPgogICAgICAgICAgICAgICAgPC9hcGk6ZXh0cmFGaWVsZD4KICAgICAgICAgICAgPC9hcGk6ZXh0cmFGaWVsZHM+CiAgICAgICAgPC9hcGk6dXBkYXRlRXh0cmFGaWVsZHM+CiAgICA8L3NvYXBlbnY6Qm9keT4KPC9zb2FwZW52OkVudmVsb3BlPgo=)

---

## Product Data Structure

### Mandatory Fields

| Field | Type | Description |
|-------|------|-------------|
| `uniqueId` | String | Product style identifier |
| `colorCode` | String | Color code (together with uniqueId forms the unique key) |

### Recommended Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | String | Product name displayed in app |
| `description` | String | Product description |
| `colorDesc` | String | Human-readable color name |
| `brandName` | String | Brand name |
| `price` | List | At least one price entry |
| `size` | List | Available sizes with stock |
| `medium` | List | At least one IMAGE_PRIMARY |

See [Product Types](../data-types/products.md) for the complete field reference.

---

## Best Practices

{% tabs %}
{% tab title="Performance" %}
**Batch your updates**
- Send multiple products in a single request
- Avoid sending one product per request

**Use partial updates**
- Use `updateStock` instead of `storeProducts` for stock-only changes
- Use `updatePrices` instead of `storeProducts` for price-only changes
{% endtab %}

{% tab title="Data Quality" %}
**Images**
- Always include `IMAGE_PRIMARY` for each product
- Use high-resolution images (min 1000x1000px recommended)
- Provide thumbnail URLs to speed up app loading

**Stock accuracy**
- Sync stock in real-time or at minimum every hour
- Set accurate stock to prevent overselling

**Sizing**
- Use consistent size names across products
- Set `sortCode` for proper size ordering (1, 2, 3, etc.)
{% endtab %}

{% tab title="Scheduling" %}
```
Recommended sync schedule:
├── 06:00  storeFullProductSet (full catalog)
├── Every hour  updateStock (stock levels)
├── As needed   updatePrices (price changes)
└── As needed   storeProducts (new products)
```
{% endtab %}
{% endtabs %}
