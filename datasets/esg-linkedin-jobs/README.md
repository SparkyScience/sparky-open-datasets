<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="../../.github/assets/sparky_dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="../../.github/assets/sparky_light.svg">
    <img src="../../.github/assets/sparky_light.svg" alt="Sparky Logo" width="200"/>
  </picture>

  <h1>ESG LinkedIn Jobs Dataset</h1>

  <p>
    <a href="https://creativecommons.org/licenses/by/4.0/"><img src="https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg" alt="License: CC BY 4.0"/></a>
    <a href="https://esg4pmchange.com"><img src="https://img.shields.io/badge/Project-ESG4PMChange-green" alt="ESG4PMChange"/></a>
    <img src="https://img.shields.io/badge/size-3,190%20jobs-blue.svg" alt="Size"/>
    <img src="https://img.shields.io/badge/period-Aug%202025%20--%20Jan%202026-orange.svg" alt="Period"/>
  </p>

  <p>
    <a href="#-overview"><strong>Overview</strong></a> •
    <a href="#-research-applications"><strong>Research</strong></a> •
    <a href="#-data-files"><strong>Data</strong></a> •
    <a href="#-usage-examples"><strong>Examples</strong></a> •
    <a href="#-citation"><strong>Citation</strong></a>
  </p>
</div>

---

## 📋 Overview

This dataset contains **3,190 unique job postings** related to Environmental, Social, and Governance (ESG) roles, collected between August 2025 and January 2026 as part of the **ESG4PMChange** project.

### ESG4PMChange Project

The dataset was collected as part of the **ESG4PMChange** project, which has received funding from the European Union's Erasmus+ programme under grant agreement No. 101187376. The project aims to enhance project management competencies in ESG contexts through education, training, and data-driven insights into the evolving job market for sustainability professionals.

**Sparky*** serves as a lead data collection and curation partner, focusing on building comprehensive datasets that inform educational curriculum development, skills gap analysis, and labor market trends in the ESG sector.

**Key Statistics:**
- **Total Unique Jobs:** 3,190
- **Geographic Coverage:** Global (Europe-focused)
- **Jobs with Full Descriptions:** 3,190 (100%)
- **Average Text Length:** ~3,850 characters per job
- **Data Period:** August 2025 - January 2026

## 🎯 Research Applications

This dataset is valuable for:
- **Job Market Analysis:** Understanding ESG employment trends and demand
- **Skills Research:** Analyzing required competencies in sustainability roles using NLP
- **Text Mining:** Extracting key themes, requirements, and benefits from job descriptions
- **Industry Analysis:** Comparing ESG adoption across sectors
- **Temporal Trends:** Tracking how ESG job postings evolve over time
- **Curriculum Development:** Informing educational programs in sustainability
- **Machine Learning:** Training models for job classification, skill extraction, and trend prediction

## 📊 Data Files

| File | Format | Size | Description |
|------|--------|------|-------------|
| `esg_linkedin_jobs.json` | JSON | ~17 MB | Complete dataset with full structure |
| `esg_linkedin_jobs.csv` | CSV | ~16 MB | Spreadsheet-compatible format |
| `esg_linkedin_jobs.parquet` | Parquet | ~2.5 MB | Compressed columnar format (recommended) |
| `data_dictionary.md` | Markdown | - | Field definitions and schema |


## 📖 Data Schema

Each job record contains the following fields:

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `job_id` | string | Anonymized job identifier | "ESG2DD49368E8D8" |
| `title` | string | Job title | "ESG Analyst", "Sustainability Manager" |
| `remote_option` | string | Remote work availability | "Yes", "No", "Hybrid" |
| `text` | string | Full cleaned job description | Complete job description text (3,000-5,000 chars typical) |
| `company_info` | string | Company description | Company overview and background |
| `full_header` | string | Structured header information | "Vienna, Austria \| 6 days ago \| 22 applicants \| Promoted" |

**Note:** All HTML and special characters have been cleaned. The `full_header` field uses pipe separators (`|`) for easy parsing.

[**➡️ View Full Data Dictionary**](data_dictionary.md)

## 🔍 Data Quality

### Strengths
- ✅ **High Uniqueness:** Duplicates removed during collection
- ✅ **Complete Text:** 100% of jobs have full description text
- ✅ **Cleaned Data:** HTML and special characters removed
- ✅ **Anonymized IDs:** Job identifiers are hashed for privacy
- ✅ **Geographic Diversity:** Jobs from multiple European countries and beyond
- ✅ **Rich Text Content:** Average ~3,850 characters per job description

### Data Cleaning Applied
1. ✓ Duplicate removal based on original identifiers
2. ✓ HTML tag removal and character cleaning
3. ✓ Job ID anonymization using cryptographic hashing
4. ✓ Text normalization and whitespace cleanup
5. ✓ Header formatting with pipe separators

## 📊 Sample Analysis

### Quick Statistics

```python
import pandas as pd

# Load dataset
df = pd.read_parquet('esg_linkedin_jobs.parquet')

print(f"Total jobs: {len(df):,}")
print(f"Average text length: {df['text'].str.len().mean():.0f} characters")
print(f"Remote options: {df['remote_option'].value_counts().to_dict()}")
```

### Top Themes

Common themes found in job descriptions:
- Sustainability reporting and ESG metrics
- Climate change and carbon reduction
- Corporate social responsibility
- Regulatory compliance and standards
- Stakeholder engagement
- Data analysis and reporting

## 🛠️ Usage Examples

### Load and Explore

```python
import pandas as pd

# Load the dataset (Parquet is fastest)
df = pd.read_parquet('esg_linkedin_jobs.parquet')

# Basic exploration
print(df.info())
print(df.head())

# Check for missing values
print(df.isnull().sum())
```

### Extract Key Information from Headers

```python
# Parse full_header field
df['header_parts'] = df['full_header'].str.split(' | ')

# Extract components
def parse_header(header):
    parts = header.split(' | ')
    result = {
        'location': parts[0] if len(parts) > 0 else '',
        'posted_time': parts[1] if len(parts) > 1 else '',
        'applicants': parts[2] if len(parts) > 2 else '',
    }
    return result

# Apply parsing
header_info = df['full_header'].apply(parse_header)
df_parsed = pd.DataFrame(header_info.tolist())
df = pd.concat([df, df_parsed], axis=1)

print(df[['title', 'location', 'applicants']].head())
```

### Text Analysis - Skill Extraction

```python
import pandas as pd

# Load data
df = pd.read_parquet('esg_linkedin_jobs.parquet')

# Search for specific skills in job descriptions
skills = ['Python', 'R', 'sustainability', 'ESG reporting',
          'carbon', 'climate', 'GHG', 'CSRD', 'GRI']

for skill in skills:
    df[f'requires_{skill.replace(" ", "_")}'] = \
        df['text'].str.contains(skill, case=False, na=False)

# Count jobs requiring each skill
skill_counts = {}
for skill in skills:
    count = df[f'requires_{skill.replace(" ", "_")}'].sum()
    skill_counts[skill] = count

# Display results
for skill, count in sorted(skill_counts.items(), key=lambda x: x[1], reverse=True):
    print(f"{skill}: {count} jobs ({count/len(df)*100:.1f}%)")
```

### Topic Modeling with NLP

```python
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.decomposition import LatentDirichletAllocation
import pandas as pd

# Load data
df = pd.read_parquet('esg_linkedin_jobs.parquet')

# Create TF-IDF vectors
vectorizer = TfidfVectorizer(max_features=1000, stop_words='english')
tfidf = vectorizer.fit_transform(df['text'])

# Apply LDA for topic modeling
lda = LatentDirichletAllocation(n_components=5, random_state=42)
topics = lda.fit_transform(tfidf)

# Get top words for each topic
feature_names = vectorizer.get_feature_names_out()
for topic_idx, topic in enumerate(lda.components_):
    top_words = [feature_names[i] for i in topic.argsort()[-10:]]
    print(f"Topic {topic_idx + 1}: {', '.join(top_words)}")
```

### Classification by Job Level

```python
import pandas as pd

df = pd.read_parquet('esg_linkedin_jobs.parquet')

# Classify by seniority level based on title
def classify_level(title):
    title_lower = title.lower()
    if any(word in title_lower for word in ['director', 'head', 'chief', 'vp']):
        return 'Senior Leadership'
    elif any(word in title_lower for word in ['senior', 'lead', 'principal']):
        return 'Senior'
    elif any(word in title_lower for word in ['junior', 'assistant', 'associate']):
        return 'Junior'
    else:
        return 'Mid-Level'

df['level'] = df['title'].apply(classify_level)

# Analyze distribution
print(df['level'].value_counts())
```

## 📚 Citation

If you use this dataset in your research, please cite:

```bibtex
@dataset{sparky_esg_linkedin_2026,
  author = {Sparky* (Sparky solution d.o.o.)},
  title = {ESG LinkedIn Jobs Dataset},
  year = {2026},
  month = {January},
  publisher = {GitHub},
  version = {1.0},
  url = {https://github.com/SparkyScience/sparky-open-datasets/tree/main/datasets/esg-linkedin-jobs},
  note = {3,190 ESG job postings, August 2025 - January 2026}
}
```

**APA Format:**
Sparky solution d.o.o. (2026, January). *ESG LinkedIn Jobs Dataset* (Version 1.0) [Dataset]. GitHub. https://github.com/SparkyScience/sparky-open-datasets/tree/main/datasets/esg-linkedin-jobs

**MLA Format:**
Sparky solution d.o.o. "ESG LinkedIn Jobs Dataset." Version 1.0, GitHub, Jan. 2026, https://github.com/SparkyScience/sparky-open-datasets/tree/main/datasets/esg-linkedin-jobs.

## ⚖️ Legal and Ethical Considerations

### Data Collection Method
This dataset was collected for academic research purposes as part of the ESG4PMChange Erasmus+ project.

**Important Considerations:**
- ✅ Only publicly available job postings were collected
- ✅ No personal data or user profiles were collected
- ✅ All identifiers have been anonymized
- ✅ Rate limiting and ethical practices were employed
- ✅ Data is provided for research and educational purposes only

### Usage Restrictions
Users of this dataset should:
- ✓ Respect intellectual property rights
- ✓ Use data responsibly and ethically
- ✓ Not use data for automated job applications
- ✓ Provide appropriate attribution (see Citation above)
- ✓ Comply with applicable data protection regulations (GDPR, etc.)

### Privacy
- Job identifiers are cryptographically anonymized
- No direct links or contact information included
- Company information is limited to publicly disclosed descriptions

## 🎓 Funding Acknowledgment

This dataset was collected as part of the **ESG4PMChange** project.

**Funding:** This project has received funding from the European Union's Erasmus+ programme under grant agreement No. 101187376.

**Disclaimer:** Views and opinions expressed are however those of the author(s) only and do not necessarily reflect those of the European Union or European Education and Culture Executive Agency (EACEA). Neither the European Union nor the granting authority can be held responsible for them.


## 📞 Contact

For questions, issues, or feedback about this dataset:

- **GitHub Issues:** [Report an issue](https://github.com/SparkyScience/sparky-open-datasets/issues)
- **Email:** [info@sparky.science](mailto:info@sparky.science)
- **Sparky* Website:** [sparky.science](https://sparky.science)
- **Project Website:** [ESG4PMChange](https://esg4pmchange.com)


## 📜 License

This dataset is licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

**You are free to:**
- ✓ Share — copy and redistribute the material in any medium or format
- ✓ Adapt — remix, transform, and build upon the material for any purpose, even commercially

**Under the following terms:**
- ⚠️ **Attribution** — You must give appropriate credit, provide a link to the license, and indicate if changes were made

---

<div align="center">

**Part of the [Sparky Open Datasets](../../) collection**

[← Back to All Datasets](../../README.md) | [Data Dictionary](data_dictionary.md) | [Report Issue](https://github.com/SparkyScience/sparky-open-datasets/issues)

<br><br>

## 🌐 Project Information

<a href="https://esg4pmchange.com" target="_blank">
  <img src="../../.github/assets/esg4pmchange/esg_main.png" alt="ESG4PMChange Project" height="80"/>
</a>

<p>
  <strong><a href="https://esg4pmchange.com" target="_blank">Visit ESG4PMChange Project Website →</a></strong>
</p>

<p>
  <em>
    This dataset was created as part of the ESG4PMChange project,<br>
    funded by the European Union's Erasmus+ programme<br>
    under grant agreement No. 101187376
  </em>
</p>

<br>

---

<p>
  <strong>Made with ❤️ by <a href="https://sparky.science">Sparky*</a></strong><br>
  <em>Advancing open science through quality datasets</em>
</p>

</div>
