# Wordcab-PII Examples

CLI tools for detecting and replacing PII in Word documents using Wordcab-PII models.

## Installation

```bash
# Using uv (recommended)
uv pip install gliner python-docx

# Using pip
pip install gliner python-docx

# Optional: Install Faker for realistic data replacement
uv pip install faker
```

## Wordcab-PII CLI Tool

The main tool is `process_docx.py`, which provides multiple commands for working with Word documents.

### Commands

#### 1. Detect PII

Detect and analyze PII in Word documents.

```bash
# Basic detection (uses all PII types by default)
python process_docx.py detect text_sample.docx

# Extract text from tables (uses all PII types by default)
python process_docx.py detect table_sample.docx

# Detect specific PII group
python process_docx.py detect text_sample.docx --pii   # Personal identifiers
python process_docx.py detect text_sample.docx --phi   # Health information
python process_docx.py detect text_sample.docx --pci   # Payment card data

# Different output formats
python process_docx.py detect text_sample.docx --format json
python process_docx.py detect text_sample.docx --format redacted

# Save results to file
python process_docx.py detect text_sample.docx --output results.json

# Adjust detection threshold
python process_docx.py detect text_sample.docx --threshold 0.3
```

#### 2. Replace PII

Replace PII with fake data (realistic with Faker, generic without).

```bash
# Basic replacement (uses all PII types by default)
python process_docx.py replace text_sample.docx

# Specify output file
python process_docx.py replace text_sample.docx --output anonymized.docx

# Replace specific PII group
python process_docx.py replace text_sample.docx --pii   # Personal identifiers
python process_docx.py replace text_sample.docx --phi   # Health information
python process_docx.py replace text_sample.docx --pci   # Payment card data

# Replace specific PII types (use underscores instead of spaces)
python process_docx.py replace text_sample.docx --pii-types name ssn credit_card phone_number
```

### Example Output

```
============================================================
PII DETECTION SUMMARY
============================================================
Total PII instances found: 15

PII by type:

name: 4 instance(s)
  - John Michael Smith
  - Jane Smith
  - Dr. Sarah Johnson
  - Robert Williams

ssn: 1 instance(s)
  - 123-45-6789

phone number: 2 instance(s)
  - (415) 555-1234
  - (415) 555-5678
```

## PII Type Format

When specifying custom PII types with `--pii-types`, use underscores instead of spaces:
- `phone_number` (not "phone number")
- `credit_card` (not "credit card")
- `email_address` (not "email address")
- `location_address` (not "location address")
- `name_medical_professional` (not "name medical professional")
