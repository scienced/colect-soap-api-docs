# Colect Data Forge

Colect Data Forge is an AI-powered tool for generating and validating SOAP XML requests. Use it to learn the API, create test data, and debug your integration.

{% hint style="info" %}
**Access the tool:** [Colect Data Forge](https://colect-xml-generator-app-xr8n8.ondigitalocean.app/)
{% endhint %}

---

## Features

| Feature | Description |
|---------|-------------|
| **XML Generator** | Generate valid SOAP XML examples using natural language prompts |
| **XML Validator** | Validate your XML against the official XSD schema |
| **History** | View your previous generations and validations |

---

## XML Generator (Prompt Tab)

Generate SOAP XML examples by describing what you need in plain English. The AI understands all Colect SOAP API operations.

### How to Use

1. Select the **Prompt** tab
2. Describe what you want to generate (e.g., "Create a storeProducts request with 2 t-shirts in different colors with sizes S, M, L and stock levels")
3. Click **Generate**
4. Copy or download the generated XML

### Examples of Prompts

```
Create a storeCustomers request for a Dutch fashion boutique
with 2 shipping locations and VIP discount group
```

```
Generate a storeProducts request for a silk blouse with
5 sizes, multiple prices for EUR and USD, and product images
```

```
Create storeHistoricalOrders with 2 orders containing
3 order lines each, including shipment tracking URLs
```

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
- **Error messages** pointing to specific issues
- **Load from Output** to validate previously generated XML
- **Supports large files** - validate complete product catalogs or customer sets

### Common Validation Errors

| Error | Cause | Solution |
|-------|-------|----------|
| Element not expected | Wrong element name or order | Check spelling and element sequence |
| Missing required element | Mandatory field omitted | Add the required element |
| Invalid value | Wrong data type or format | Check date formats, numbers, enum values |
| Namespace error | Wrong or missing namespace | Verify `xmlns` declarations |

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
- Get specific error messages
- Compare your XML with a generated valid example
{% endtab %}
{% endtabs %}

---

## Tips

1. **Start simple** - Generate a basic example first, then add complexity
2. **Use as templates** - Copy generated XML and modify for your needs
3. **Validate often** - Check your XML before each API call during development
4. **Check the History** - Revisit previous generations without re-prompting
