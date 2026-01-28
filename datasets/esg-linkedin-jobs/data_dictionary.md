# ESG LinkedIn Jobs Dataset - Data Dictionary

## Overview

This document provides detailed descriptions of all fields in the cleaned ESG LinkedIn Jobs Dataset.

**Dataset:** ESG LinkedIn Jobs Dataset
**Version:** 1.0 (Cleaned)
**Last Updated:** 2026-01-27
**Total Fields:** 6
**Total Records:** 3,190

## Field Definitions

### `job_id`
- **Type:** String
- **Required:** Yes
- **Unique:** Yes
- **Description:** Anonymized job identifier created using cryptographic hashing
- **Format:** "ESG" prefix + 12-character hexadecimal hash (uppercase)
- **Example:** "ESG2DD49368E8D8", "ESG01768C1AD4E7"
- **Anonymization:** SHA-256 hash of original identifier, ensuring privacy
- **Notes:** Cannot be reversed to identify original posting
- **Null Values:** None

---

### `title`
- **Type:** String
- **Required:** Yes
- **Unique:** No
- **Description:** Official job title as listed by the employer
- **Format:** Free text, typically 20-100 characters
- **Example:** "ESG Analyst", "Sustainability Manager", "Financial Accountant (m/w/d)"
- **Language:** Primarily English and German
- **Notes:** May include gender markers (m/w/d) common in German-speaking regions
- **Null Values:** None

---

### `remote_option`
- **Type:** String
- **Required:** No
- **Unique:** No
- **Description:** Indicates if remote work is available
- **Format:** Categorical text
- **Possible Values:** "Yes", "No", "Hybrid", "" (empty string)
- **Example:** "Yes", "No"
- **Notes:** Many jobs specify this in the text field instead
- **Null Values:** Some records have empty values

---

### `text`
- **Type:** String (Long text)
- **Required:** Yes
- **Unique:** No
- **Description:** Complete job description with all HTML and special characters cleaned
- **Format:** Clean plain text, typically 3,000-5,000 characters
- **Average Length:** ~3,850 characters
- **Max Length:** ~16,200 characters
- **Language:** Primarily English and German
- **Content Includes:**
  - Job responsibilities and duties
  - Required qualifications and skills
  - Benefits and compensation information
  - Company overview and culture
  - Application instructions
- **Cleaning Applied:**
  - HTML tags removed
  - HTML entities decoded
  - Multiple spaces normalized
  - Leading/trailing whitespace trimmed
- **Notes:** Primary field for text analysis, NLP, and machine learning
- **Null Values:** None (all jobs have complete descriptions)

**Example Structure:**
```
About the job
[Company introduction and context]

Responsibilities:
- [List of duties]

Requirements:
- [Qualifications and skills]

We Offer:
- [Benefits and compensation]

Application Process:
[How to apply]
```

---

### `company_info`
- **Type:** String (Long text)
- **Required:** No
- **Unique:** No
- **Description:** Company description and overview
- **Format:** Multi-paragraph text, typically 100-1,000 characters
- **Example:** "VTR Service is a specialized service provider for operational management..."
- **Cleaning Applied:**
  - HTML tags removed
  - HTML entities decoded
  - Whitespace normalized
- **Notes:** Provides context about the hiring organization and industry
- **Null Values:** Some records (~10%)

---

### `full_header`
- **Type:** String
- **Required:** Yes
- **Unique:** No
- **Description:** Structured header information with pipe separators
- **Format:** Pipe-separated values: "Location | Posted Time | Applicants | Status"
- **Example:** "Vienna, Austria | 6 days ago | 22 applicants | Promoted by hirer | Actively reviewing applicants"
- **Separator:** " | " (space-pipe-space)
- **Typical Components:**
  1. Geographic location
  2. Time since posting
  3. Number of applicants
  4. Promotion status
  5. Review status
- **Cleaning Applied:**
  - HTML tags removed
  - Multiple separators normalized to pipe
  - Whitespace cleaned
- **Parsing:** Easy to split on " | " for structured extraction
- **Null Values:** None

**Parsing Example:**
```python
parts = full_header.split(' | ')
location = parts[0] if len(parts) > 0 else ''
posted_time = parts[1] if len(parts) > 1 else ''
applicants = parts[2] if len(parts) > 2 else ''
```

---

## Data Quality Summary

| Field | Completeness | Unique | Cleaned | Notes |
|-------|--------------|--------|---------|-------|
| `job_id` | 100% | Yes | ✓ Anonymized | Primary key |
| `title` | 100% | No | ✓ | Clean, reliable |
| `remote_option` | 100% | No | ✓ | Yes, No |
| `text` | 100% | No | ✓ | **Most valuable** |
| `company_info` | ~99% | No | ✓ | Useful context |
| `full_header` | 100% | No | ✓ | Structured data |

---

## Removed Fields

The following fields were present in the original collection but have been removed for privacy and data quality reasons:

- `salary` - Mostly empty ("N/A")
- `scraped_at` - Timestamps removed for anonymization
- `date_posted` - Mostly empty ("N/A")
- `job_type` - Mostly empty ("N/A")
- `employment_type` - Mostly empty ("N/A")
- `application_count` - Mostly empty ("N/A")
- `company` - Contained identifiable location info (replaced by `full_header`)
- `job_url` - Direct links removed for privacy
- `location` - Redundant with `full_header` (location info available there)

---

## Recommended Analysis Fields

For most analyses, focus on:
1. ✅ `job_id` - Primary key and grouping
2. ✅ `title` - Job title analysis and classification
3. ✅ `full_header` - Location and timing extraction
4. ✅ `text` - **Primary field for NLP, text mining, skill extraction**
5. ✅ `company_info` - Industry classification and context

---

## Common Data Parsing Tasks

### Extract Skills from Text

```python
import pandas as pd

df = pd.read_parquet('esg_linkedin_jobs.parquet')

# Define skills to search for
skills = ['Python', 'R', 'SQL', 'Excel', 'PowerBI', 'Tableau',
          'ESG reporting', 'GRI', 'SASB', 'TCFD', 'carbon accounting',
          'sustainability', 'climate', 'GHG', 'CSRD']

# Search in text field
for skill in skills:
    df[f'has_{skill.replace(" ", "_")}'] = \
        df['text'].str.contains(skill, case=False, na=False)

# Count occurrences
skill_counts = {skill: df[f'has_{skill.replace(" ", "_")}'].sum()
                for skill in skills}
```

### Parse Header Information

```python
def parse_full_header(header):
    """Extract structured information from full_header"""
    parts = str(header).split(' | ')

    return {
        'location': parts[0].strip() if len(parts) > 0 else '',
        'posted_time': parts[1].strip() if len(parts) > 1 else '',
        'applicants': parts[2].strip() if len(parts) > 2 else '',
        'promoted': 'promoted' in header.lower(),
        'actively_reviewing': 'actively reviewing' in header.lower()
    }

df['header_parsed'] = df['full_header'].apply(parse_full_header)
df_parsed = pd.DataFrame(df['header_parsed'].tolist())
df = pd.concat([df, df_parsed], axis=1)
```

### Clean Text for NLP

```python
import re

def clean_for_nlp(text):
    """Additional cleaning for NLP tasks"""
    # Convert to lowercase
    text = text.lower()

    # Remove special characters but keep spaces
    text = re.sub(r'[^a-z0-9\s]', ' ', text)

    # Remove multiple spaces
    text = re.sub(r'\s+', ' ', text)

    return text.strip()

df['text_clean'] = df['text'].apply(clean_for_nlp)
```

---

## Data Types and Memory

**Recommended dtypes for optimal performance:**

```python
dtypes = {
    'job_id': 'string',
    'title': 'string',
    'location': 'string',
    'remote_option': 'category',  # Limited values
    'text': 'string',
    'company_info': 'string',
    'full_header': 'string'
}

df = pd.read_csv('esg_linkedin_jobs.csv', dtype=dtypes)
```

---

## Known Issues and Limitations

1. **Language Mix:** Descriptions are in multiple languages (primarily English and German)
2. **Location Ambiguity:** Location data is in `full_header` and may require parsing
3. **Remote Work:** Not consistently specified; often mentioned in text only
4. **Company Names:** Not included as separate field (found in `text` or `company_info`)
5. **Posting Dates:** Relative times only (e.g., "6 days ago") in `full_header`

---

## Contact

For questions about the data schema or to report data quality issues:
- **GitHub Issues:** https://github.com/SparkyScience/sparky-open-datasets/issues
- **Email:** info@sparky.science
- **Label:** `esg-dataset` or `data-quality`

---

**Last Updated:** 2026-01-28
**Version:** 1.0 (Cleaned)
**Dataset:** ESG LinkedIn Jobs Dataset
