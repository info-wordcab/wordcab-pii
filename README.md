# Wordcab-PII

A collaboration between [Wordcab](https://wordcab.com) and [Knowledgator](https://www.knowledgator.com/) to produce production-grade PII, PHI, and PCI detection for open use.

This model is fine-tuned on `knowledgator/gliner-multitask-large-v0.5` for comprehensive PII detection across various domains.

## Installation

```bash
# Using uv (recommended)
uv pip install gliner

# Using pip
pip install gliner
```

## Quick Start

```python
from gliner import GLiNER

model = GLiNER.from_pretrained("wordcab/wordcab-pii-detection-large-v0.3")

text = "John Smith called from 415-555-1234 to discuss his account number 12345678."
labels = ["name", "phone number", "account number"]

entities = model.predict_entities(text, labels, threshold=0.3)

for entity in entities:
    print(f"{entity['text']} => {entity['label']}")
```

## Available PII Labels

### Personal Identifiers
- `name` - Full names
- `name given` - First/given names
- `name family` - Last/family names
- `name medical professional` - Healthcare provider names
- `dob` - Date of birth
- `age` - Age information
- `gender` - Gender identifiers
- `marital status` - Marital status

### Contact Information
- `email address` - Email addresses
- `phone number` - Phone numbers
- `location address` - Street addresses
- `location address street` - Street names
- `location city` - City names
- `location state` - State/province names
- `location country` - Country names
- `location zip` - ZIP/postal codes
- `location` - General location references
- `county` - County names
- `address` - General address information
- `zip` - ZIP codes

### Financial Information
- `credit card` - Credit card numbers
- `credit card expiration` - Card expiration dates
- `cvv` - CVV/security codes
- `account number` - Bank account numbers
- `accounts` - Account references
- `ssn` - Social Security Numbers
- `pin` - PIN codes
- `money` - Monetary amounts

### Healthcare Information
- `condition` - Medical conditions
- `medical process` - Medical procedures
- `test result` - Medical test results
- `organization medical facility` - Healthcare facility names
- `discharge date` - Hospital discharge dates

### Identification Documents
- `passport number` - Passport numbers
- `policy number` - Insurance policy numbers
- `confirmation number` - Confirmation/reference numbers
- `esidno` - ESI numbers

### Other Information
- `organization` - Organization names
- `occupation` - Job titles/occupations
- `date` - General dates
- `date interval` - Date ranges
- `time` - Time references
- `duration` - Time durations
- `month` - Month references
- `origin` - Ethnic/national origin
- `language` - Language information
- `physical attribute` - Physical descriptions
- `numerical pii` - Other numerical identifiers
- `password` - Passwords
- `filename` - File names
- `planduration` - Plan durations
- `rate` - Rates/percentages
- `number` - General numbers

## Usage Examples

### Single PII Type Detection

```python
text = "Please send the invoice to jane.doe@company.com"
labels = ["email address"]

entities = model.predict_entities(text, labels, threshold=0.3)
# Output: jane.doe@company.com => email address
```

### Multiple PII Types

```python
text = "Patient Mary Johnson, DOB 01/15/1980, was discharged on March 10, 2024 from St. Mary's Hospital"
labels = ["name", "dob", "discharge date", "organization medical facility"]

entities = model.predict_entities(text, labels, threshold=0.3)
# Output:
# Mary Johnson => name
# 01/15/1980 => dob
# March 10, 2024 => discharge date
# St. Mary's Hospital => organization medical facility
```

### Financial Information Detection

```python
text = "Card ending in 4532, CVV 123, expires 12/25. Account: 9876543210"
labels = ["credit card", "cvv", "credit card expiration", "account number"]

entities = model.predict_entities(text, labels, threshold=0.3)
```

### Comprehensive PII Scan

```python
# Use all available labels for comprehensive detection
all_labels = [
    'name', 'name given', 'name family', 'name medical professional',
    'phone number', 'email address', 'ssn', 'credit card', 'cvv',
    'credit card expiration', 'location address', 'location city',
    'location state', 'location country', 'location zip', 'dob', 'age',
    'gender', 'account number', 'organization', 'occupation', 'passport number',
    'policy number', 'condition', 'medical process', 'organization medical facility'
]

text = "Your sensitive document text here..."
entities = model.predict_entities(text, all_labels, threshold=0.3)
```

## Parameters

- `threshold`: Confidence threshold for entity detection (0.0-1.0). Default: 0.3
  - Lower values **(0.2-0.3)**: Higher recall, may include more false positives
  - Higher values **(0.5-0.7)**: Higher precision, may miss some entities

## Batch Processing

```python
texts = ["Text 1 with PII", "Text 2 with PII", "Text 3 with PII"]
labels = ["name", "phone number", "email address"]

results = model.run(texts, labels, threshold=0.3, batch_size=8)

for text_idx, entities in enumerate(results):
    print(f"Text {text_idx + 1}:")
    for entity in entities:
        print(f"  {entity['text']} => {entity['label']}")
```

## Citation

```bibtex
@misc{smechov2025wordcabpii,
      title={Wordcab-PII: Production-ready PII/PHI/PCI detection based on GLiNER multi-task},
      author={Aleksandr Smechov and Ihor Stepanov},
      year={2025},
      eprint={2406.12925},
      archivePrefix={arXiv},
      primaryClass={cs.LG}
}
```

## License

Apache 2.0