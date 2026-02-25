# Schema Snapshots

This directory stores versioned snapshots of the Colect SOAP API XSD and WSDL files. Since the schema files don't carry version numbers, we snapshot them manually to enable diffing between versions.

## Convention

Each snapshot is stored in a date-named directory: `snapshots/YYYY-MM-DD/`

Contents per snapshot:
- `api-3.0.wsdl` — The WSDL service definition
- `xsd-1.xsd` — XSD schema 1 (product relations namespace)
- `xsd-2.xsd` — XSD schema 2 (main API types and operations)

## How to take a new snapshot

```bash
DATE=$(date +%Y-%m-%d)
mkdir -p schema/snapshots/$DATE
curl -s "https://connector.colect.services/services/api/3.0?wsdl" -o schema/snapshots/$DATE/api-3.0.wsdl
curl -s "https://connector.colect.services:443/services/api/3.0?xsd=1" -o schema/snapshots/$DATE/xsd-1.xsd
curl -s "https://connector.colect.services:443/services/api/3.0?xsd=2" -o schema/snapshots/$DATE/xsd-2.xsd
```

## Diffing between snapshots

```bash
diff schema/snapshots/OLD_DATE/xsd-2.xsd schema/snapshots/NEW_DATE/xsd-2.xsd
```
