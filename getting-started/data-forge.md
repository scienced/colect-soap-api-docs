# Colect Data Forge

Colect Data Forge is an AI-powered tool for generating and validating SOAP XML requests. Use it to learn the API, create test data, and debug your integration.

{% hint style="info" %}
**Access the tool:** [Colect Data Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/)
{% endhint %}

{% hint style="warning" %}
**Access Restriction:** This tool is only available to users with a **Company Admin** role (users who have backend access to Colect).
{% endhint %}

---

## Features

| Feature | Description |
|---------|-------------|
| **XML Generator** | Generate valid SOAP XML examples using natural language prompts |
| **URL Extraction** | Include product URLs in prompts to auto-fetch names, details, and images |
| **Generation Settings** | Select specific features (sizes, pricing, media) before generating |
| **XML Validator** | Validate your XML against the official XSD schema |
| **AI Error Fixing** | Automatically fix validation errors with AI assistance |
| **AI Error Summaries** | Get human-readable explanations of cryptic XSD errors |
| **Analysis Panel** | See how complete your XML is compared to the full schema |
| **Send to API** | Send validated requests directly to the Colect API endpoint |
| **History** | View your previous generations and validations (IP-based) |

---

## XML Generator (Prompt Tab)

Generate SOAP XML examples by describing what you need in plain English. The AI understands all Colect SOAP API operations.

<figure><img src="../.gitbook/assets/colect-data-forge-prompt.jpg" alt="Colect Data Forge prompt interface"><figcaption>XML Generator showing the prompt interface with operation selector and generated output</figcaption></figure>

### How to Use

1. Select the **Prompt** tab
2. Choose the **Operation** (e.g., `storeProducts`, `storeCustomers`)
3. Describe what you want to generate in the prompt field
4. Click **Generate XML**
5. Copy or download the generated XML

### Examples of Prompts

```
Generate 5 products for the Brand Ivy Beau.
It is a women Brand
https://ivybeau.com/en/collections/tops-blouses-1
Add extra fields
Add translations for DE and NL as the xsd contains
```

```
Create a storeCustomers request for a Dutch fashion boutique
with 2 shipping locations and VIP discount group
```

```
Create storeHistoricalOrders with 2 orders containing
3 order lines each, including shipment tracking URLs
```

### Quick Examples

The tool provides quick example buttons for common scenarios:
- Products for a bodywear brand with sub sizes and stock levels
- Historical orders
- Customers
- Products with sizes S, M, L and color variations

### URL Extraction

Include product page URLs in your prompt, and the AI will automatically fetch and extract:
- Product names and descriptions
- Image URLs
- Other product details from the page

When a URL is detected, you'll see a notification: **"URL detected - will fetch and extract product data"**

### Generation Settings

Click the settings icon to open the Generation Settings modal, where you can select exactly which features to include in your generated XML.

<figure><img src="../.gitbook/assets/colect-data-forge-generation-settings.jpg" alt="Generation Settings modal showing feature selection"><figcaption>Generation Settings modal with operation selector and feature checkboxes</figcaption></figure>

**Available feature categories:**

| Category | Options |
|----------|---------|
| **Sizes** | Size Names, Sub-sizes, EAN/Barcode Codes, Stock Levels, Replenishment Dates, Prepacks, Customer Size Naming |
| **Pricing** | Retail Price, Wholesale Price, Original Prices, Purchase Price, Price Groups, Customer-specific Prices, Size-specific Prices, Price Validity Periods |
| **Media** | Primary Image, Swatch Image, Back/Model Images, Additional Images |

Use **All** or **None** buttons to quickly select or deselect all options in a category.

### Limitations

{% hint style="warning" %}
**Generation Limits:** The generator can create 1-5 items per request, depending on the complexity and number of attributes requested. For larger datasets, generate multiple batches or use the output as a template.
{% endhint %}

---

## XML Validator (Validate Tab)

Validate your SOAP XML against the official Colect API XSD schema. This helps catch errors before sending requests to the API.

### How to Use

1. Select the **Validate** tab
2. Paste your XML in the left panel, or click **Upload XML File**
3. Click **Validate XML**
4. View validation results - green checkmark means valid

<figure><img src="../.gitbook/assets/colect-data-forge-validate.jpg" alt="Colect Data Forge validation interface showing valid XML"><figcaption>XML Validator showing a successful validation with syntax-highlighted output</figcaption></figure>

### Features

- **Syntax highlighting** for easy reading
- **Error indicators** pointing to specific issues
- **Load from Output** to validate previously generated XML
- **Supports large files** - validate complete product catalogs or customer sets

---

## AI-Powered Error Fixing

When validation fails, Colect Data Forge provides powerful AI assistance to understand and fix errors.

<figure><img src="../.gitbook/assets/colect-data-forge-error-fix.jpg" alt="Colect Data Forge AI error summary and fix"><figcaption>AI Summary explaining validation errors in plain English with "Fix All with AI" option</figcaption></figure>

### AI Error Summary

XSD validation errors are often cryptic and hard to understand. The **AI Summary** translates these technical messages into clear explanations:

- **Root Cause** - What's actually wrong (e.g., "10 element ordering/sequence errors")
- **What's happening** - Detailed explanation of the issue
- **Where it occurs** - Which elements and products are affected
- **Expected structure** - What the XSD schema expects

### Fix All with AI

Click the **Fix All with AI** button to automatically correct validation errors. The AI will:
- Reorder elements to match the XSD sequence
- Fix namespace issues
- Correct common formatting problems

{% hint style="info" %}
**Tip:** After AI fixes your XML, review the changes to understand what was wrong. This helps you avoid similar errors in your integration code.
{% endhint %}

---

## Analysis Panel

The Analysis panel provides a real-time overview of how complete your XML is compared to the full schema capabilities.

<figure><img src="../.gitbook/assets/colect-data-forge-analysis.jpg" alt="Analysis panel showing XML completeness"><figcaption>Analysis panel showing which features are present in your XML</figcaption></figure>

### What it Shows

For each operation, the panel displays categories with checkmarks indicating which features are present:

| Category | Tracked Features |
|----------|-----------------|
| **Sizes** | Size Names, Sub-sizes, EAN/Barcode Codes, Stock Levels, Replenishment Dates, Prepacks |
| **Pricing** | Retail Price, Wholesale Price, Original Prices, Purchase Price, Price Groups, Customer-specific Prices |
| **Media** | Primary Image, Swatch Image, Back/Model Images, Additional Images |

- ✓ **Green checkmark** - Feature is present in your XML
- ✗ **Red X** - Feature is not present

The counters (e.g., "10/27", "3/7") show how many items include each feature category.

---

## Send to SOAP Endpoint

Test your XML directly against the Colect API without needing a separate SOAP client like SoapUI or Postman.

<figure><img src="../.gitbook/assets/colect-data-forge-send-to-api.jpg" alt="Send to SOAP Endpoint dialog"><figcaption>Send to SOAP Endpoint dialog with API key field and response preview</figcaption></figure>

### How to Use

1. Generate or validate your XML
2. Click the **Send** button (paper airplane icon) in the toolbar
3. Enter your **API Key** (saved automatically for future use)
4. Click **Send Request**
5. View the response in the dialog

### Response Handling

- **Success** - Empty response body indicates the operation completed successfully
- **Error** - The response will show the SOAP fault with error details

{% hint style="warning" %}
**API Key Security:** Your API key is stored locally in your browser and sent directly to the Colect API. It is never stored on the Forge server.
{% endhint %}

---

## Common Validation Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Element not expected | Wrong element name or order | Check spelling and element sequence |
| Missing required element | Mandatory field omitted | Add the required element |
| Invalid value | Wrong data type or format | Check date formats, numbers, enum values |
| Namespace error | Wrong or missing namespace | Verify `xmlns` declarations |
| Sequence error | Elements in wrong order | Use AI fix or reorder to match XSD |

---

## Use Cases

{% tabs %}
{% tab title="Learning" %}
**New to the API?**
- Generate example requests to understand the data structure
- See how different fields relate to each other
- Experiment with different operations
{% endtab %}

{% tab title="Development" %}
**Building your integration?**
- Generate test data quickly
- Validate your XML before sending to production
- Debug failing requests by comparing with valid examples
{% endtab %}

{% tab title="Debugging" %}
**Something not working?**
- Paste your failing XML into the validator
- Get AI-generated explanations of cryptic errors
- Use "Fix All with AI" to automatically correct issues
- Compare your XML with a generated valid example
{% endtab %}
{% endtabs %}

---

## Tips

1. **Start simple** - Generate a basic example first, then add complexity
2. **Use as templates** - Copy generated XML and modify for your needs
3. **Validate often** - Check your XML before each API call during development
4. **Read AI summaries** - Learn from error explanations to improve your code
5. **Check the History** - Revisit previous generations without re-prompting

