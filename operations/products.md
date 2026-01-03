# Products

Manage your product catalog in Colect. Products are the core of your B2B e-commerce platform, containing all information your customers need to place orders.

{% hint style="success" %}
**Full Example:** [Download complete storeProduct.xml](https://assets.apptitude.nl/KnowledgeBase/Webservice_3_0/External/storeProduct.xml) - A comprehensive example showing all available product fields.
{% endhint %}

***

## Operations Overview

| Operation                                                | Type           | Description                     |
| -------------------------------------------------------- | -------------- | ------------------------------- |
| [`storeProducts`](products.md#storeproducts)             | Partial Sync   | Add or update specific products |
| [`storeFullProductSet`](products.md#storefullproductset) | Full Sync      | Replace entire product catalog  |
| [`deleteProducts`](products.md#deleteproducts)           | Delete         | Remove specific products        |
| [`deleteAllProducts`](products.md#deleteallproducts)     | Delete         | Clear all products              |
| [`getAllProducts`](products.md#getallproducts)           | Read           | Retrieve all products           |
| [`updateStock`](products.md#updatestock)                 | Partial Update | Update stock levels only        |
| [`updatePrices`](products.md#updateprices)               | Partial Update | Update prices only              |
| [`updateExtraFields`](products.md#updateextrafields)     | Partial Update | Update extra fields only        |

***

## storeProducts

Add new products or update existing ones. Products not included in the request remain unchanged.

{% hint style="info" %}
Products are uniquely identified by the combination of `uniqueId` + `colorCode`. Sending a product with an existing combination will update that product.
{% endhint %}

### Request

againa a test

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgzIGSRgABQlM904GEFy8vOACgwBpGByssHcJAFoDLoBrVuAzKH09EZac4LrEhpGkKHLPSoZa6brgAEIurpwyiqqcAEYcbdW1mYL3OH0AR3cYAElPAIBlABUATQAZFC6ABj+hyGl2ud0ezym5wuc04YCwEgAwpkYAE0l8msCYXDJEjvGcoUkCnAIAQUQiMPwQPpODg3l0XnR9BIGJjRsTSfiobNRt4uBJ9C59GA4AEyrx9O4CDhYUxhTgGF0QIzmaykLzOPzBcLOeduUhYfCACL2TioimcPqqg2SY1cHVrPUAIwkEDgngAciSUR8OhIcGkXW7Vc7XR6vfbpk7A54nqiAEoAQXdhsBwejschBMJMIgDBgUkkWVxKLeLwiDzjpatufzheLEfWBU4NYLEiyttNdIZTOq1bzrfbJob9QKIGi4Dg9ZeLwATABWVVj-jC+uZgl6pcTjuvBb6OAyF6S0l+md-eeL8fC7drrmjhi59w1BMIt4PABqKEX948NRvuq2OxlNSe5SCcXTDtCoy7pwKJ-lmeqcJ0EjwJwRbIgEKAAKpxlaSEoWheJweuBQAO50HC9i2KU-IwQE84CACqpkRRNh2EBtFEbeczIfe+gYOxKIACwAJwCMJC7DKMPEQHxAkQZGRIwCsABmthjqqcBKfJmiqtBsFZjMAE4C8+gAF72DgJFuHQOAgEUFpgdp2ajFS5lOZBSDsiiLwaeGnG6qOkgMPWQKSWQQWrgZClzNEk7oQAHAA7IcM4AMyCXOABsiXxcJqZhbFkVRSOcx2WAFpfDACCYO50WjHcrquAwOSHIxYUNXATWTMV9RhWVFVVTV-kzH1Zn6cVG5jbVOlEuGACyvkcsNdXhcy9YzouEXoctjYxa69ZJSl6VZTleUbQV+3bT1Hn9X0lXVRg00lfV7iNW4LVzn8qodV1T1fuVd2DY9O1JKNbkg5s2w4A8fBSBZpwQwUpKeBKBB-QUzWwAEDxzQmADiKAAPolHGOMJnGHyqpj43Xc5SCdI9xhQKQ7CcJ4cACDAeAklAdgWGABAcM1dj-ICXSOhgfRdMp5SdQIABWUBSKqDNOaqyOo2rSMwCjkro3M1PY7jBOE2kz4YmFhsgx5qtMyzrBsxzXM83zsKC3ZWQiwChzi5L4sQBaCtKyrEjA7TM2lVteKhYFa1XRNYUa3r-m6YsyzDnqCz7CsENGXsSwHDOjmI3MVy3PcsbvN8vzeyroIVxCPUIdiiLoQA6hEbxWi3RVRXqXkBOSlLUrS9LKiyYUDyXozWhI25t4yebd0aQ651DAgbzgMQo64wq2Dgyn6JgnggDgG8CMX8FhVnBc5w6fVFMh+fLL+I2iOEGTZEEQzv6kaAPcoIIQA)

hi there - this is a test

### Response

```xml
<ns2:storeProductsResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
```

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgzIGSRgAMXcMDAAFCUz3TgYAZRgGXLy84AKDAGkYHKywdwkAWgMhgGte4DMofT0ZnpzgtsSOmaQoGs86lqXltuAAQiGhnABJOE4MLxgcAEEAGXucDdr6kBx+9xwAdwg4BhwRRwExgUBwx1ae3aBXccH0AEd3DBTp4Ag0ACoATXuKCGAAY8QBGKYwuGI5Go3ZQ-JrThgLASADCmRgATS9y6JNp9MkzO8kOpmkOxxwAjFOAAZuUMM9NtscJ4IAwIKLxRCqcsubMXlt6gLNQUddt9dTVrNYQikSi0Vicfi8QAmLVIC3k60a00FOkMvmsgByADVMc7vbyWSaocKTmKBJLpbLXgDFcrVbH1YKktNtXK9R72kdo+LbDKwAweBIE7qGO8Y+ChhHnSAihJStLqommjsVrIUsgMtkglNROE0AglLAAkA)

### Use Cases

* Initial data load when setting up your Colect integration
* Nightly full synchronization to ensure data consistency
* Bulk product catalog refresh

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgyRvOwYYAAUJTPdOBhBcvLzgAoMAaRgcrLB3CQBaA26AazbgMyh9PVHWnOD6xMbRpCgKzyqGOpn6ubH3OH0AR3cYAElPAIBlABUATQAZFG6ABnuARmGC7b2D47X12YLOMCwEgAwpkYAE0tdmq95v9ASDvN8ZtCxotKtVERsCqjlujpj9fvN3vsjicLjc7o8AEzIpBEz4nPH4zSbJCwyTwsEAOQAapcaWzgaCMQ0RiilithTSijASuU0TU1sNROEMtkgkqwqk0AglLAAkA)

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgyRvOwYYAEEMDAAFCUz3TgYQXLy84AKDAGkYHKywdwkAWgN+gGsu4DMofT1Jzpzg5vGCopgS8qqazzqGpqTZFOQM7KDx0XC0BCVYAKA)

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgyQpGAYAQQwMAAUJTPdOBhBcvLzgAoMAaRgcrLB3CQBaAz6Aa07gMyh9PQmOnOCmsYKi0vKqmrqGuaTZFOQM7KCx0XC0BCVYAKA)

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

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgyR3KE8IBhgAZQYwTgBrXLy84AKDAGkYHKywdwkAWgMemvbgMyh9PVG2nOCGxKbRpAAzCH0MAHk4AFU4GrgwAHc4AAUJTPdOBhAAhgl3GGGCpZX1rZ39o5PPM4v6mc05sZAVVqAEk4AswCg7AR4Awfr9ZgUoB8vlt9ABHW7AzwBcoAFQAmgAZFA9AAMpIAjPd5kjTudURiYFi4fC-ojkecAMJgLASbneAJpQktaljWmfLk8yT8mAs1n-Mj6ABeMAAchBoQEALKixUq9Wa6as-LzQHVGqEmAITBy41s+YYiBwVwMHIAdgArLrHc63FM7bMRgCgRarTajb9dWaQWCIVCYbaEaaQ6DwZDeAmI8aFeKUXB0ZjsXiiSTyVSg0hc-T84zmVn5ey6QxubyZYLhbqq82pXzMrL6-CFSBlWqNbLCVGRwb+wGTcHzZbrRhE9mCj6XTkPaTve4nRuVw0oyHF+G7Ufzam4xnnbbdUUSmVKuafsNROEMtkgq+wqk0EvlEEQA)

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

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgyR3KE8IBhgABQl9TntcvLzgAoMAaRgcrLB3CQBaA26AazbgMyh9PVHWnOD6xMbRpAAzCH0MAHk4AFU4frgwAHc4Ssz3TgYQAIYJdxhhgqWV9a2d-cOJY9Pz6ZnNObGoKpqAEk4AswCg7AR4Aw6t8GgV-u8GFt9ABHa6AzwBADKABUAJoAGRQ3QADCSAIy3eYIzwnJFwVHozFfWH5alvWmnADCYCwEh53gCaQJzSpfw5dJ5fIFMBhrJ+BU4XQk8E4WRlARQGwASmKkEqJCq4GqNSz5b8kP9qjAcVlYAEAOoRVZErEAQSJeqtNVt9rNrIt3tlACYAOwCACsJK9ANl-oaI3F1uBoPBvChcpmgdjKbBEIz8e+2cRyLRMAx2PxRNJFJjJYZZYrhaz8Il3N5kg1wtFictbYYUs7mTj8rh8wNRpNw81Or1E9V6unzfq2etvtl2pQOLdgIJMbXdpHo7ZSZqAQjABYBABOCP7s-LpK9oO5tOQuDQ+N6oolMqVa2fLMsgpMgGTZEEwyiOEaAIEofpAA)

### Price Types

| Type                 | Description                      |
| -------------------- | -------------------------------- |
| `WHOLESALE`          | B2B wholesale price              |
| `RETAIL`             | Suggested retail price           |
| `ORIGINAL_WHOLESALE` | Original price (before discount) |
| `PURCHASE`           | Purchase/cost price              |

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

***

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

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwZw9ghgDgpgdgNwFwFFEwDZlgAgB4C2GcIS408CAvAEQAWALg1EgPSsgDGdMBEIAOkIZyUAWABOAcw6QorSpmwxWNAFA5N+IiSTQAlrUbM2rAwM6cBICBhg2oY6FAb6GAVwAmMAXAyqAPg0tUDlKJAAJGAhvCVYgrRxQikQkACEwTwBPBMSQgyR3KE8IBhgUPAYJCAAxfUxPEFy8vOACgwBpGByssHcJAFoDAYBrbuAzKH09Ka6c4JbEtqmkADMIfQwAeTgAVTgRuDAAdzgABQlM904GJqr3GAmC9c2d-cOT88vPa9vmxc0y2mMEq1TqDSaCwBrQK7jg+gAjg8AJKeAIAZQAKgBNAAyKAGAAZCQBGJ4rOGIlFoqHQ-IrThgLASADCmRgATSuI65OmjOZbO8-zpgIKIKqtXqGBpIuhQKQcAgBA5nAgEhgAH19CR7jd9GASLyFUqObTZfTpt4uBJ9C59XAAiy1TAcMidRJfvaQEarZwbXaDcLzaKVghbA8AgBZCDcbUu478Og4fmeAA0OA8BAARnYcJ4JFkcFhjkawxgI2bg-KEPoQPocxz7o9JtMa3WG0G6UbxWCpTLg0kxaDJQ1O+b5YrlQFGXCqlkNWBVgubVJtUbJ6aBzCVr7-a5A2zZwWcIucFsV2uW0hd7b9w7K+OCmWI2dJB4pLZS+HN1uLUg2-WdgBE2pa1oBP5VlePYjtKY5LFBw7gtKkIAkaRQlGUFQSkhjT-BMojhBk2RBPhYSpGgCBKLAQRAA)

***

## Product Data Structure

### Mandatory Fields

| Field       | Type   | Description                                              |
| ----------- | ------ | -------------------------------------------------------- |
| `uniqueId`  | String | Product style identifier                                 |
| `colorCode` | String | Color code (together with uniqueId forms the unique key) |

### Recommended Fields

| Field         | Type   | Description                   |
| ------------- | ------ | ----------------------------- |
| `name`        | String | Product name displayed in app |
| `description` | String | Product description           |
| `colorDesc`   | String | Human-readable color name     |
| `brandName`   | String | Brand name                    |
| `price`       | List   | At least one price entry      |
| `size`        | List   | Available sizes with stock    |
| `medium`      | List   | At least one IMAGE\_PRIMARY   |

See [Product Types](../data-types/products.md) for the complete field reference.

***

## Best Practices

{% tabs %}
{% tab title="Performance" %}
**Batch your updates**

* Send multiple products in a single request
* Avoid sending one product per request

**Use partial updates**

* Use `updateStock` instead of `storeProducts` for stock-only changes
* Use `updatePrices` instead of `storeProducts` for price-only changes
{% endtab %}

{% tab title="Data Quality" %}
**Images**

* Always include `IMAGE_PRIMARY` for each product
* Use high-resolution images (min 1000x1000px recommended)
* Provide thumbnail URLs to speed up app loading

**Stock accuracy**

* Sync stock in real-time or at minimum every hour
* Set accurate stock to prevent overselling

**Sizing**

* Use consistent size names across products
* Set `sortCode` for proper size ordering (1, 2, 3, etc.)
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
