# Making Your First Call

This guide walks you through making your first API call to the Colect SOAP API.

***

## Prerequisites

* Your API key (contact Colect support)
* A SOAP client (SoapUI, Postman, or your programming language's SOAP library)

***

## Step 1: Download the WSDL

Most SOAP clients can auto-generate request templates from the WSDL:

```
https://connector.colect.services/services/api/3.0?wsdl
```

***

## Step 2: Test Connection with getInfo

The simplest operation to verify your setup:

{% tabs %}
{% tab title="Request" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:getInfo>
            <api:apiKey>your-api-key</api:apiKey>
        </api:getInfo>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwfgHgtgNgBAbgUwE4GcCWB7AdgXgEQCMAdAAx4wJYDGGAJmlgOb4CqAKgGIC0AHHiAD4AUMBQYAhgAdKcAFwBRLIigZpQmBpiQoWFLLFSZ+ABYAXU5NkB6KyirGEEcSiLaDkohiSNbEyVZkEFWkrPHVNbV1ZKTQTc0sbGKIqKiIUcSgEdMkPKUlTNFMAV1oEIiwoUOFNGFE-GVkACQRxUqQras06wyVZACE6AE9Omo1gGNlGBFMASSwAMwwR0ZrxyTRo9YBpBGHBjCKkLhiuAGtd4CsJmJ3h8JXLianZhaX72t8euQHaO8v3BqKZSqBACIA)
{% endtab %}

{% tab title="cURL" %}
```bash
curl -X POST \
  'https://connector.colect.services:443/services/api/3.0' \
  -H 'Content-Type: text/xml; charset=utf-8' \
  -H 'SOAPAction: ""' \
  -d '<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:getInfo>
            <api:apiKey>your-api-key</api:apiKey>
        </api:getInfo>
    </soapenv:Body>
</soapenv:Envelope>'
```
{% endtab %}
{% endtabs %}

***

## Step 3: Store Your First Product

{% tabs %}
{% tab title="Request" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeProducts>
            <api:apiKey>your-api-key</api:apiKey>
            <api:product>
                <api:uniqueId>STYLE-001</api:uniqueId>
                <api:name>Classic T-Shirt</api:name>
                <api:description>Premium cotton t-shirt</api:description>
                <api:brandName>Your Brand</api:brandName>
                <api:colorCode>BLK</api:colorCode>
                <api:colorDesc>Black</api:colorDesc>
                <api:price>
                    <api:currencyCode>EUR</api:currencyCode>
                    <api:retailPrice>49.95</api:retailPrice>
                    <api:wholesalePrice>25.00</api:wholesalePrice>
                    <api:net>false</api:net>
                </api:price>
                <api:size>
                    <api:name>S</api:name>
                    <api:sortCode>1</api:sortCode>
                    <api:stockLevel>
                        <api:quantity>100</api:quantity>
                    </api:stockLevel>
                </api:size>
                <api:size>
                    <api:name>M</api:name>
                    <api:sortCode>2</api:sortCode>
                    <api:stockLevel>
                        <api:quantity>150</api:quantity>
                    </api:stockLevel>
                </api:size>
                <api:size>
                    <api:name>L</api:name>
                    <api:sortCode>3</api:sortCode>
                    <api:stockLevel>
                        <api:quantity>120</api:quantity>
                    </api:stockLevel>
                </api:size>
                <api:medium>
                    <api:type>IMAGE_PRIMARY</api:type>
                    <api:url>https://cdn.example.com/products/style-001-blk.jpg</api:url>
                </api:medium>
            </api:product>
        </api:storeProducts>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwfgHgtgNgBAbgUwE4GcCWB7AdgXgEQCMAdAAx4wJYDGGAJmlgOb4CqAKgGIC0AHHiAD4AUMBQYAhgAdKcAFwBRLIigZpQmBpiQoWFLLFSZ+ABYAXU5NkB6KyirGEEcSiLaDkohiSNbEyVZkEFWkrPHVNbV1ZKTQTc0sbGKIqKiIUcSgEdMkPKUlTNFMAV1oEIiwoUOFNGFE-GVkACQRxUqQras06wyVZACE6AE9Omo1gGP1TLwQABSQ6IqpTFBHRmvHJNGjNgGkEYcGMIqQuGK4Aa33gKwmYveHwtfWJyXnaRdNVp9GNraKsNAARyKCAAkrQBABlNgATQAMvIuCQSARrhN-kCQeCvt8uhMsOIIAgBABhKDOdBUGBsLiQ4xoJCmNGbWQEok43G1CalOxIND5TBYARzRxoIoQGA0czYGCmLgoemM5lbHlUPkC7Ac3G-WQAIyQ4iwtAAcoTiTCjkgYH0DUblXrbSazVrvjqaCokCS6MS+nCdvb3V4vaUXU83RgPQARLJUAR9clUc4BiNeaN2UNrHWvNBUYmPTlhiZUY5IShUQbB4nyFgAJQDJbLFe9Ge1E1LpnEaCgcxzxIALABOIgDgCs9vbne7fNzLddEwA7sYI1kMrNp8SAEwj0gke2L5fpTI9mf5gs-fEIT4AMwyKAQ9qwl9nXRuLOzJ7PzxZ6AAXnnP4WLJssSkIPs6p4AVy35eKYlYCKir5bGIjJwRBAE6igUyJnCCDKM+nI6sChoFKYwwEMi9pEVgJEPJBPyIZMGDYbhQT4bUDG-v+kEYWgf5sV+WzAQIACyYHsmhn4YTBcEbvayGwc2ElnhhWHnDheFKdxExUTR8EjruDE6YUtF0S+EyYUxaksVAbFybxXHoeZ9n8XiQHOnCYkOaZUkoc2ADMcnSYppmuUhqnqaxmmOSyRmkfBG4GdpRTEcZLljBx4XWbZHHOVF54skS9DimlUFbKR0gCKCwkAIIAOLyAA+jMNZVdVNYwva5VeXROrHDZZgWHoNhULQWBEAgYCEpImTJBgEBWK8CxLCgtikZkSIolwupQOcRAAFaSIw9p9dlEyFWKEAtvai3vEsWpyVMpZzEtyyrNc7gNAMtAPO99S9IoyiqHmQA)
{% endtab %}

{% tab title="Response" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <ns2:storeProductsResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
    </soap:Body>
</soap:Envelope>
```

An empty response body indicates success.
{% endtab %}
{% endtabs %}

***

## Step 4: Store Your First Customer

{% tabs %}
{% tab title="Request" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:storeCustomers>
            <api:apiKey>your-api-key</api:apiKey>
            <api:customer>
                <api:customerNo>CUST-001</api:customerNo>
                <api:name>Fashion Boutique Amsterdam</api:name>
                <api:email>orders@fashionboutique.nl</api:email>
                <api:currencyCode>EUR</api:currencyCode>
                <api:retailCurrencyCode>EUR</api:retailCurrencyCode>
            </api:customer>
        </api:storeCustomers>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwfgHgtgNgBAbgUwE4GcCWB7AdgXgEQCMAdAAx4wJYDGGAJmlgOb4CqAKgGIC0AHHiAD4AUMBQYAhgAdKcAFwBRLIigZpQmBpiQoWFLLFSZ+ABYAXU5NkB6KyirGEEcSiLaDkohiSNbEyVZkEFWkrPHVNbV1ZKTQTc0sbGKIqKiIUcSgEdMkPKUlTNFMAV1oEIiwoUOFNGFE-GVkACQRxUqQras06wyVZACE6AE9Omo1gGP1TLwQAYSKUKYhkFBHRmvHJNGjNgGkEYcGMIqQuGK4Aa33gKwmYveHwtfWJqnnF5FWn0Y2t14WMJZIAByGAEMxYAGU2FwSCQCNcXm8AcgQZ8vl0JlhxEsBBxnMZMFgYAMigUAI5FBAwACCEAWyFo2IRm1kWJxj3RzxZjnEaCgAi8bRQAAEAGb4wkAIyO5Mp5SgzK2PL5aM5P1kryQSEoVEGMzoCAE8hYACVFRrjtrqHqDar0ertaZeVA5lqdTbSkbTebHc7XVbdfrPRyvua-u8kHbzf9tXN-oCVhzru4GgNaA9k-VeoplKpDUA)
{% endtab %}

{% tab title="Response" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <ns2:storeCustomersResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/"/>
    </soap:Body>
</soap:Envelope>
```

An empty response body indicates success.
{% endtab %}
{% endtabs %}

***

## Step 5: Check for Orders

After your customers place orders in the Colect app or B2B webstore:

{% tabs %}
{% tab title="Request" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:api="http://api.cc.salesapp.apptitude.nl/">
    <soapenv:Header/>
    <soapenv:Body>
        <api:getUnprocessedOrders>
            <api:apiKey>your-api-key</api:apiKey>
        </api:getUnprocessedOrders>
    </soapenv:Body>
</soapenv:Envelope>
```

[Try in Colect Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/workbench#lz=DwfgHgtgNgBAbgUwE4GcCWB7AdgXgEQCMAdAAx4wJYDGGAJmlgOb4CqAKgGIC0AHHiAD4AUMBQYAhgAdKcAFwBRLIigZpQmBpiQoWFLLFSZ+ABYAXU5NkB6KyirGEEcSiLaDkohiSNbEyVZkEFWkrPHVNbV1ZKTQTc0sbGKIqKiIUcSgEdMkPKUlTNFMAV1oEIiwoUOFNGFE-GVkACQRxUqQras06wyVZACE6AE9Omo1gGNlGBFMWLEkkDCoslARaAHkkNpQR0ZrxyTRog4BpBGHBjCKkLhiuAGsz4CsJmNPh8N2niamZuYWllArdabZDbD61Xw9OQDWjvJ7uBqKZSqBACIA)
{% endtab %}

{% tab title="Response" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <ns2:getUnprocessedOrdersResponse xmlns:ns2="http://api.cc.salesapp.apptitude.nl/">
            <ns2:return>
                <ns2:orderNumber>ORD123456789</ns2:orderNumber>
                <ns2:customerNo>CUST-001</ns2:customerNo>
                <ns2:timestamp>2025-01-15T10:30:00</ns2:timestamp>
                <ns2:currency>EUR</ns2:currency>
                <ns2:totalAmount>250.00</ns2:totalAmount>
                <ns2:totalCount>10</ns2:totalCount>
                <!-- ... order lines and additional fields ... -->
            </ns2:return>
        </ns2:getUnprocessedOrdersResponse>
    </soap:Body>
</soap:Envelope>
```

Returns all orders that have not yet been marked as processed.
{% endtab %}
{% endtabs %}

***

## Common Integration Pattern

```
┌──────────────────────────────────────────────────────────────┐
│                    Daily Sync Schedule                        │
├──────────────────────────────────────────────────────────────┤
│  06:00  storeFullProductSet     Full product catalog sync     │
│  06:30  storeFullCustomerSet    Full customer sync            │
│  Every  updateStock             Real-time stock updates       │
│  hour   updatePrices            Price changes                 │
│  Every  getUnprocessedOrders    Poll for new orders           │
│  5 min  markOrdersAsProcessed   Confirm order processing      │
└──────────────────────────────────────────────────────────────┘
```

{% hint style="info" %}
**Best Practice:** Use `storeFullProductSet` and `storeFullCustomerSet` for nightly full syncs, and the partial update operations (`updateStock`, `updatePrices`) for real-time changes during the day.
{% endhint %}

***

## Next Steps

* [Product Operations](../operations/products.md) - Complete product sync guide
* [Customer Operations](../operations/customers.md) - Customer management
* [Order Operations](../operations/orders.md) - Order retrieval and processing
