<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="/.github/assets/sparky_dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="/.github/assets/sparky_light.svg">
    <img src="/.github/assets/sparky_light.svg" alt="Sparky Logo" width="200"/>
  </picture>

  <h1>Sparky Open Datasets</h1>

  <p>
    <strong>Curated, research-grade datasets for advancing open science and data-driven innovation</strong>
  </p>

  <p>
    <a href="https://creativecommons.org/licenses/by/4.0/"><img src="https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg" alt="License: CC BY 4.0"/></a>
    <a href="datasets/"><img src="https://img.shields.io/badge/datasets-2-blue.svg" alt="Datasets"/></a>
    <img src="https://img.shields.io/badge/GDPR-compliant-success.svg" alt="GDPR Compliant"/>
    <img src="https://img.shields.io/badge/EU-funded-yellow.svg" alt="EU Funded"/>
  </p>

  <p>
    <a href="#-available-datasets"><strong>Explore Datasets</strong></a> •
    <a href="#-getting-started"><strong>Get Started</strong></a> •
    <a href="CONTRIBUTING.md"><strong>Contribute</strong></a> •
    <a href="docs/RESEARCH_GUIDE.md"><strong>Research Guide</strong></a>
  </p>
</div>

---

## 🌟 Welcome

Welcome to **Sparky Open Datasets**, a commitment to advancing open science by providing high-quality, well-documented datasets that enable researchers, educators, and practitioners worldwide to conduct reproducible research, explore market trends, develop analytical methods, and foster innovation through open data.

Our mission is to connect people, technology, and data to drive meaningful insights across industries, from sustainability to healthcare.

---

## 📊 Available Datasets

<table>
<tr>
<td width="50%" valign="top">

### 💼 ESG LinkedIn Jobs Dataset
[![Domain](https://img.shields.io/badge/domain-Job%20Market%20%7C%20Sustainability-green)]()
[![Size](https://img.shields.io/badge/size-3,190%20jobs-blue)]()

**Comprehensive ESG job market analysis from LinkedIn**

Explore the evolving landscape of Environmental, Social, and Governance (ESG) careers across Europe. This dataset captures 3,190 unique job postings from August 2025 to January 2026, providing insights into sustainability careers, skill requirements, geographic trends, and industry adoption.

**Key Features:**
- 🌍 Global coverage with Europe focus
- 📊 5-month temporal data
- 📝 Full job descriptions for NLP analysis
- 🏢 2,827 unique companies

**Quick Access:**
- [JSON](datasets/esg-linkedin-jobs/esg_linkedin_jobs.json) • [CSV](datasets/esg-linkedin-jobs/esg_linkedin_jobs.csv) • [Parquet](datasets/esg-linkedin-jobs/esg_linkedin_jobs.parquet)

[**📖 View Documentation →**](datasets/esg-linkedin-jobs/README.md)

**Research Applications:**
- Job market trend forecasting
- Skills gap analysis
- Geographic ESG adoption patterns
- Compensation benchmarking
- Curriculum development

</td>
<td width="50%" valign="top">

### 🏥 HealthChain Sensor Data
[![Domain](https://img.shields.io/badge/domain-Healthcare%20%7C%20Medical%20Devices-red)]()
[![Size](https://img.shields.io/badge/size-183,736%20measurements-blue)]()

**Real-time health monitoring from medical-grade wearable sensors**

High-frequency biosensor data from Sparky Health Sensors (Class IIa medical devices) deployed at KBC Rijeka Hospital. This dataset contains 183,736 measurements at 52 Hz from 3 sensors during a 55-patient clinical study, providing rich 9-axis IMU data for healthcare AI research.

**Key Features:**
- 🏥 Real clinical environment
- ⚡ 52 Hz sampling (high-frequency)
- 🔐 GDPR compliant & fully anonymized
- 🏆 Medical-grade certified sensors

**Quick Access:**
- [CSV](datasets/healthchain-sensor-data/healthchain_sensor_data.csv) • [Parquet](datasets/healthchain-sensor-data/healthchain_sensor_data.parquet) • [JSON Sample](datasets/healthchain-sensor-data/healthchain_sensor_data_sample.json)

[**📖 View Documentation →**](datasets/healthchain-sensor-data/README.md)

**Research Applications:**
- Fall detection algorithms
- Activity recognition (ML/DL)
- Gait analysis & mobility assessment
- Sleep quality monitoring
- Sensor fusion techniques
- Real-time patient monitoring

</td>
</tr>
</table>

---

## 🚀 Getting Started

### Installation

```bash
# Clone the repository
git clone https://github.com/SparkyScience/sparky-open-datasets.git
cd sparky-open-datasets

# Install Python dependencies
pip install pandas numpy scipy scikit-learn matplotlib seaborn pyarrow
```

### Quick Start Examples

<details>
<summary><strong>📊 ESG Job Market Analysis</strong></summary>

```python
import pandas as pd
import matplotlib.pyplot as plt

# Load ESG jobs dataset
df = pd.read_parquet('datasets/esg-linkedin-jobs/esg_linkedin_jobs.parquet')

# Analyze job trends
print(f"Total ESG Jobs: {len(df):,}")
print(f"Average text length: {df['text'].str.len().mean():.0f} characters")

# Extract and visualize top locations from full_header
df['location'] = df['full_header'].str.split(' | ').str[0]
top_locations = df['location'].value_counts().head(10)

# Create visualization
fig, ax = plt.subplots(figsize=(10, 6))
top_locations.plot(kind='barh', ax=ax, color='#2E7D32')
ax.set_title('Top 10 Locations for ESG Jobs', fontsize=16, fontweight='bold')
ax.set_xlabel('Number of Job Postings')
plt.tight_layout()
plt.show()

# Skills analysis from job descriptions
skills = ['Python', 'sustainability', 'ESG', 'reporting', 'analysis', 'climate']
for skill in skills:
    count = df['text'].str.contains(skill, case=False, na=False).sum()
    print(f"Jobs requiring '{skill}': {count} ({count/len(df)*100:.1f}%)")
```

</details>

<details>
<summary><strong>🏥 Healthcare Sensor Analysis</strong></summary>

```python
import pandas as pd
import numpy as np

# Load HealthChain sensor data (Parquet is 6x faster!)
df = pd.read_parquet('datasets/healthchain-sensor-data/healthchain_sensor_data.parquet')

# Basic statistics
print(f"Total Measurements: {len(df):,}")
print(f"Unique Sensors: {df['sensor_id'].nunique()}")
print(f"Date Range: {df['datetime'].min()} to {df['datetime'].max()}")
print(f"Sampling Rate: ~52 Hz")

# Calculate movement intensity
df['movement'] = np.sqrt(df['AccX']**2 + df['AccY']**2 + df['AccZ']**2)
df['rotation'] = np.sqrt(df['GyroX']**2 + df['GyroY']**2 + df['GyroZ']**2)

print(f"\nMovement Statistics:")
print(f"  Average: {df['movement'].mean():.2f} m/s²")
print(f"  Max: {df['movement'].max():.2f} m/s²")

# Detect high-activity events (potential falls or rapid movements)
high_activity = df[df['movement'] > 15]  # > 1.5g
print(f"\nHigh Activity Events: {len(high_activity):,}")

# Analyze by sensor
sensor_stats = df.groupby('sensor_id').agg({
    'movement': ['mean', 'std', 'max'],
    'datetime': 'count'
}).round(2)
print(f"\nPer-Sensor Statistics:")
print(sensor_stats)
```

</details>

<details>
<summary><strong>🔬 Advanced: Fall Detection Model</strong></summary>

```python
import pandas as pd
import numpy as np
from scipy import signal
from sklearn.ensemble import RandomForestClassifier

# Load data
df = pd.read_parquet('datasets/healthchain-sensor-data/healthchain_sensor_data.parquet')

# Feature engineering for fall detection
df['acc_magnitude'] = np.sqrt(df['AccX']**2 + df['AccY']**2 + df['AccZ']**2)
df['gyro_magnitude'] = np.sqrt(df['GyroX']**2 + df['GyroY']**2 + df['GyroZ']**2)

# Detect sudden changes (potential fall indicators)
df['acc_change'] = df['acc_magnitude'].diff().abs()
df['gyro_change'] = df['gyro_magnitude'].diff().abs()

# Identify high-impact events
fall_candidates = df[
    (df['acc_change'] > 5) |  # Sudden acceleration change
    (df['gyro_change'] > 50) |  # Sudden rotation
    (df['acc_magnitude'] > 20)  # High impact
]

print(f"Potential fall events detected: {len(fall_candidates)}")

# Extract features for ML (window-based)
def extract_features(window):
    return {
        'acc_mean': window['acc_magnitude'].mean(),
        'acc_std': window['acc_magnitude'].std(),
        'acc_max': window['acc_magnitude'].max(),
        'gyro_mean': window['gyro_magnitude'].mean(),
        'gyro_std': window['gyro_magnitude'].std(),
        'acc_peak_count': len(signal.find_peaks(window['acc_magnitude'], height=15)[0])
    }

# Example: Extract features for first 100 windows
window_size = 52 * 2  # 2 seconds at 52 Hz
features = []
for i in range(0, min(10000, len(df) - window_size), window_size):
    window = df.iloc[i:i+window_size]
    features.append(extract_features(window))

feature_df = pd.DataFrame(features)
print(f"\nExtracted {len(feature_df)} feature windows")
print(feature_df.head())
```

</details>

---

## 📂 Repository Structure

```
sparky-open-datasets/
│
├── 📊 datasets/
│   ├── esg-linkedin-jobs/           # ESG job market data
│   │   ├── README.md                # Comprehensive documentation
│   │   ├── data_dictionary.md       # Field definitions
│   │   └── *.json / *.csv / *.parquet
│   │
│   └── healthchain-sensor-data/     # Real-time health sensors
│       ├── README.md                # Comprehensive documentation
│       ├── data_dictionary.md       # Field definitions
│       ├── *.csv / *.parquet / *.json
│       ├── by_sensor/               # Per-sensor data files
│       └── sensor_mapping.json
│
├── 📚 docs/
│   └── RESEARCH_GUIDE.md            # Comprehensive research guide
│
├── 🎨 .github/
│   └── assets/                      # Logos and images
│
├── 📄 README.md                     # This file
├── 📜 LICENSE                       # CC BY 4.0 + MIT
└── 🤝 CONTRIBUTING.md               # Contribution guidelines
```

Each dataset includes:
- 📄 **README.md** – Comprehensive documentation
- 📖 **Data Dictionary** – Schema and field descriptions
- 📊 **Multiple Formats** – JSON, CSV, Parquet
- 📈 **Statistics** – Dataset summary and quality metrics
- 🐍 **Code Examples** – Python usage snippets
- 📜 **Citation** – BibTeX and APA formats

---

## 🎯 Use Cases

### For Researchers
- Conduct reproducible research with clean, validated data
- Access real-world clinical and job market datasets
- Publish findings with proper citations
- Contribute to open science initiatives

### For Educators
- Build course materials and assignments
- Teach data science and ML with real datasets
- Demonstrate best practices in data handling
- Inspire students with impactful research questions

### For Practitioners
- Benchmark algorithms against standardized data
- Prototype healthcare AI solutions
- Analyze job market trends for workforce planning
- Develop data-driven business insights

### For Students
- Learn data analysis with real-world datasets
- Complete thesis and capstone projects
- Build portfolio projects
- Gain hands-on ML/AI experience

---

## 📚 Citation

If you use these datasets in your research, please cite appropriately:

**ESG LinkedIn Jobs Dataset:**
```bibtex
@dataset{sparky_esg_2026,
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

**HealthChain Sensor Dataset:**
```bibtex
@dataset{sparky_healthchain_2026,
  author = {Sparky* (Sparky solution d.o.o.)},
  title = {HealthChain Real-Time Health Sensor Dataset},
  year = {2026},
  month = {January},
  publisher = {GitHub},
  version = {1.0},
  url = {https://github.com/SparkyScience/sparky-open-datasets/tree/main/datasets/healthchain-sensor-data},
  note = {IMU sensor measurements from 3 Sparky Health Sensors, July 2025}
}
```

---

## 🤝 Contributing

We welcome contributions from the community! Whether you want to:

- 🐛 Report data quality issues
- 📖 Improve documentation
- 💡 Suggest new datasets
- 🔧 Enhance processing scripts
- 🌍 Translate documentation

Please read our [**Contributing Guidelines**](CONTRIBUTING.md) to get started.

**Stay tuned!** Follow this repository for updates.

---

## 📜 License

**Datasets:** Licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE)
- ✅ Free to share and adapt
- ✅ Commercial use allowed
- ⚠️ Attribution required

**Code & Scripts:** Licensed under MIT License
- ✅ Free to use, modify, and distribute

---

## 📞 Contact & Support

<div align="center">

**Questions? Ideas? Found an issue?**

[![Issues](https://img.shields.io/badge/Report-Issues-red?logo=github)](https://github.com/SparkyScience/sparky-open-datasets/issues)
[![Discussions](https://img.shields.io/badge/Join-Discussions-blue?logo=github)](https://github.com/SparkyScience/sparky-open-datasets/discussions)
[![Website](https://img.shields.io/badge/Visit-Website-orange?logo=firefox)](https://sparky.science)

</div>

- 🐛 **Bug Reports:** [Open an issue](https://github.com/SparkyScience/sparky-open-datasets/issues)
- 💬 **Questions:** [Start a discussion](https://github.com/SparkyScience/sparky-open-datasets/discussions)
- 📧 **Email:** [info@sparky.science](mailto:info@sparky.science)
- 🌐 **Website:** [sparky.science](https://sparky.science)

---

<div align="center">

  **⭐ Star this repository if you find it useful!**

  <p>
    <strong>Made with ❤️ by <a href="https://sparky.science">Sparky*</a></strong><br>
    <em>Connecting people, technology, and data for a better future</em>
  </p>

  <p>
    <a href="#-welcome">Back to Top ↑</a>
  </p>

</div>
