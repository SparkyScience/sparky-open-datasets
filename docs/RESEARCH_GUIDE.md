# Research Guide - Sparky Open Datasets

This guide provides researchers with practical advice on using Sparky Open Datasets for various research applications.

## Table of Contents

1. [Getting Started](#getting-started)
2. [Dataset-Specific Research Ideas](#dataset-specific-research-ideas)
3. [Cross-Dataset Research Opportunities](#cross-dataset-research-opportunities)
4. [Methodological Considerations](#methodological-considerations)
5. [Publishing Research](#publishing-research)

## Getting Started

### Prerequisites

```bash
# Python 3.8+
pip install pandas numpy scipy scikit-learn matplotlib seaborn

# For Parquet files (recommended)
pip install pyarrow

# For machine learning
pip install tensorflow pytorch

# For NLP (ESG jobs)
pip install transformers spacy nltk
```

### Quick Start

```python
import pandas as pd

# Load ESG jobs
esg_df = pd.read_parquet('datasets/esg-linkedin-jobs/esg_linkedin_jobs.parquet')

# Load HealthChain sensors
health_df = pd.read_parquet('datasets/healthchain-sensor-data/healthchain_sensor_data.parquet')

# Explore
print(f"ESG Jobs: {len(esg_df)} records")
print(f"Health Sensors: {len(health_df)} measurements")
```

## Dataset-Specific Research Ideas

### ESG LinkedIn Jobs Dataset

#### 1. Job Market Trend Analysis
**Research Question:** How is ESG job demand evolving across different regions and industries?

**Approach:**
- Temporal analysis of job postings over the 5-month period
- Geographic clustering of ESG roles
- Industry classification using company_info field
- Skill requirement extraction from job descriptions

**Methods:**
- Time series analysis
- Geographic information systems (GIS)
- Text mining and NLP
- Trend forecasting

**Potential Findings:**
- Growth rates in ESG hiring
- Regional differences in ESG priorities
- Emerging ESG job titles and roles

#### 2. Skills Gap Analysis
**Research Question:** What skills are most demanded in ESG roles, and how do they vary by position?

**Approach:**
- Extract skills from job descriptions using NLP
- Cluster jobs by skill requirements
- Compare technical vs. soft skills
- Identify underserved skill areas

**Methods:**
- Named Entity Recognition (NER)
- TF-IDF analysis
- K-means clustering
- Association rule mining

**Tools:**
```python
from sklearn.feature_extraction.text import TfidfVectorizer
import spacy

# Extract skills
nlp = spacy.load('en_core_web_sm')
docs = [nlp(desc) for desc in esg_df['text']]

# Common patterns: "experience with X", "knowledge of Y"
# Technical skills: Python, R, GIS, SQL, sustainability reporting
```

#### 3. Compensation Analysis
**Research Question:** What factors influence ESG role compensation?

**Approach:**
- Extract salary information from job text using NLP/regex
- Correlate with job title, location (from `full_header`), company size
- Compare EU vs. non-EU compensation

**Challenges:**
- Salary information embedded in text (no dedicated field)
- Sparse salary mentions (estimated ~2% of postings)
- Multiple currencies and formats require normalization
- Self-selection bias in salary disclosure

#### 4. Sustainability Reporting Trends
**Research Question:** How are companies communicating their ESG hiring needs?

**Approach:**
- Sentiment analysis of job descriptions
- Keyword frequency analysis (climate, sustainability, carbon, social impact)
- Company size vs. ESG focus correlation

**Methods:**
- Sentiment analysis (VADER, TextBlob)
- Topic modeling (LDA)
- Discourse analysis

### HealthChain Real-Time Sensor Dataset

#### 1. Fall Detection Algorithm Development
**Research Question:** Can we accurately detect falls using 9-axis IMU data?

**Approach:**
- Label fall events (if ground truth available) or simulate falls
- Extract features: acceleration magnitude, gyro peaks, impact detection
- Train classifier (Random Forest, SVM, Neural Network)
- Validate with cross-validation

**Features:**
```python
import numpy as np
from scipy import signal

# Acceleration magnitude
df['acc_mag'] = np.sqrt(df['AccX']**2 + df['AccY']**2 + df['AccZ']**2)

# Peak detection
peaks, _ = signal.find_peaks(df['acc_mag'], height=20)  # >2g threshold

# Angular velocity
df['gyro_mag'] = np.sqrt(df['GyroX']**2 + df['GyroY']**2 + df['GyroZ']**2)

# Feature vector: [peak_acc, peak_gyro, duration, pre_fall_variance]
```

**Evaluation Metrics:**
- Sensitivity (true positive rate)
- Specificity (true negative rate)
- False alarm rate
- Time to detection

#### 2. Activity Recognition
**Research Question:** Can we classify patient activities (lying, sitting, standing, walking) from sensor data?

**Approach:**
- Segment data into windows (e.g., 2-second segments)
- Extract time-domain and frequency-domain features
- Train supervised ML model
- Validate with leave-one-sensor-out cross-validation

**Features:**
- Mean, variance, min, max of each axis
- Frequency domain: FFT, dominant frequencies
- Zero-crossing rate
- Signal energy

**Models:**
- Decision Trees
- Random Forest
- Gradient Boosting (XGBoost)
- LSTM Neural Networks (for temporal dependencies)

```python
from scipy.fft import fft
from sklearn.ensemble import RandomForestClassifier

def extract_features(window_df):
    features = {}

    # Time domain
    for col in ['AccX', 'AccY', 'AccZ', 'GyroX', 'GyroY', 'GyroZ']:
        features[f'{col}_mean'] = window_df[col].mean()
        features[f'{col}_std'] = window_df[col].std()
        features[f'{col}_min'] = window_df[col].min()
        features[f'{col}_max'] = window_df[col].max()

    # Frequency domain (accelerometer only)
    acc_mag = np.sqrt(window_df['AccX']**2 + window_df['AccY']**2 + window_df['AccZ']**2)
    fft_vals = np.abs(fft(acc_mag))
    features['dominant_freq'] = np.argmax(fft_vals[:len(fft_vals)//2])
    features['fft_energy'] = np.sum(fft_vals**2)

    return features
```

#### 3. Gait Analysis and Mobility Assessment
**Research Question:** Can we quantify patient mobility and gait quality from wearable sensors?

**Approach:**
- Detect walking periods using accelerometer patterns
- Extract gait parameters: stride length, cadence, symmetry
- Compare across patients or time periods
- Correlate with recovery outcomes

**Gait Parameters:**
- Stride time
- Step regularity
- Gait speed
- Postural stability

**Methods:**
- Peak detection in vertical acceleration
- Autocorrelation for periodicity
- Harmonic ratio for smoothness

#### 4. Sleep Quality Assessment
**Research Question:** Can we assess sleep quality using movement sensors?

**Approach:**
- Identify sleep periods (low movement)
- Detect restlessness and position changes
- Calculate sleep fragmentation index
- Compare with traditional actigraphy

**Sleep Metrics:**
- Total sleep time
- Wake after sleep onset (WASO)
- Number of awakenings
- Sleep efficiency

```python
# Detect sleep periods (low movement)
df['movement'] = np.sqrt(df['AccX']**2 + df['AccY']**2 + df['AccZ']**2)
df['is_sleep'] = df['movement'].rolling(window=52*60).std() < 0.5  # Low variance

# Count awakenings
sleep_periods = df['is_sleep'].diff()
awakenings = (sleep_periods == -1).sum()
```

#### 5. Sensor Fusion and Orientation Tracking
**Research Question:** How can we optimally combine accelerometer, gyroscope, and magnetometer for robust orientation estimation?

**Approach:**
- Implement complementary filter or Kalman filter
- Compare sensor fusion algorithms
- Evaluate drift and accuracy
- Apply to posture recognition

**Algorithms:**
- Complementary Filter
- Extended Kalman Filter (EKF)
- Madgwick Filter
- Mahony Filter

```python
from scipy.spatial.transform import Rotation

def complementary_filter(acc, gyro, dt, alpha=0.98):
    """
    Combine accelerometer and gyroscope for tilt estimation

    acc: [AccX, AccY, AccZ] in m/s²
    gyro: [GyroX, GyroY, GyroZ] in deg/s
    dt: time step in seconds
    alpha: complementary filter coefficient (0.98 = 98% gyro, 2% acc)
    """
    # Convert gyro to rad/s
    gyro_rad = np.array(gyro) * np.pi / 180

    # Gyroscope-based angle update
    angle_gyro = gyro_rad * dt

    # Accelerometer-based angle
    acc_norm = acc / np.linalg.norm(acc)
    angle_acc = np.array([
        np.arctan2(acc_norm[1], acc_norm[2]),
        np.arctan2(-acc_norm[0], np.sqrt(acc_norm[1]**2 + acc_norm[2]**2))
    ])

    # Combine
    angle = alpha * angle_gyro[:2] + (1 - alpha) * angle_acc

    return angle * 180 / np.pi  # Convert back to degrees
```

## Cross-Dataset Research Opportunities

### 1. Work-Health Nexus
**Research Question:** How do job demands (from ESG dataset) relate to health monitoring needs (HealthChain)?

**Approach:**
- Identify physically demanding ESG roles from job descriptions
- Compare with activity patterns in HealthChain data
- Develop occupational health risk models

### 2. Digital Health for Workplace Wellness
**Research Question:** How can wearable sensor data inform workplace wellness programs in ESG-focused companies?

**Approach:**
- Extract company wellness initiatives from ESG job descriptions
- Model employee activity levels using HealthChain patterns
- Propose data-driven wellness interventions

## Methodological Considerations

### Privacy and Ethics

1. **Data Anonymization:**
   - Both datasets are fully anonymized
   - Do not attempt to re-identify individuals
   - Report any potential privacy concerns immediately

2. **Ethical Use:**
   - Use data for beneficial research purposes only
   - Consider implications of findings on vulnerable populations
   - Follow your institution's IRB guidelines

3. **Bias Awareness:**
   - ESG jobs: Overrepresentation of European markets
   - HealthChain: Hospital-only setting, not representative of general population
   - Both: Selection bias in data collection

### Data Quality

1. **ESG Dataset:**
   - Location data in `full_header` requires parsing (pipe-separated)
   - Multiple languages (English, German primarily)
   - Some fields have partial data (e.g., `remote_option` ~70% complete)
   - Cleaned HTML but some formatting variations may remain

2. **HealthChain Dataset:**
   - Limited temporal scope (2 days)
   - Small sensor count (3 sensors)
   - No clinical labels or ground truth
   - Hospital environment may affect magnetic readings

### Statistical Power

1. **Sample Size:**
   - ESG: 3,190 jobs (adequate for most analyses)
   - HealthChain: 183,736 measurements but only 3 sensors (limited for patient-level analysis)

2. **Generalizability:**
   - ESG: Europe-focused, may not generalize to other regions
   - HealthChain: Hospital patients, may not generalize to healthy populations

### Reproducibility

1. **Version Control:**
   - Always cite dataset version
   - Document preprocessing steps
   - Share code via GitHub

2. **Random Seeds:**
   - Set random seeds for reproducibility
   - Document train/test splits

```python
import numpy as np
import random

# Set seeds
np.random.seed(42)
random.seed(42)

# For PyTorch
import torch
torch.manual_seed(42)

# For TensorFlow
import tensorflow as tf
tf.random.set_seed(42)
```

## Publishing Research

### Citation

Always cite the dataset:

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

### Acknowledgments

Include in your paper:
- EU funding acknowledgment (for both datasets)
- Hospital collaboration (for HealthChain)
- Dataset contributors

### Share Your Results

We encourage researchers to:
1. Share code on GitHub
2. Link to Sparky Open Datasets
3. Submit papers to open-access journals
4. Present at conferences
5. Notify us of publications (we'll feature them!)

## Resources

### Learning Materials

**Machine Learning:**
- Scikit-learn documentation: https://scikit-learn.org
- Fast.ai courses: https://www.fast.ai

**Time Series Analysis:**
- TSFresh: https://tsfresh.readthedocs.io
- Statsmodels: https://www.statsmodels.org

**NLP:**
- spaCy: https://spacy.io
- Hugging Face Transformers: https://huggingface.co

**Sensor Data Analysis:**
- SciPy Signal Processing: https://docs.scipy.org/doc/scipy/reference/signal.html
- MNE-Python (biosignals): https://mne.tools

### Community

- GitHub Discussions: Ask questions and share insights
- Issues: Report bugs or request features
- Twitter: Follow @sparky_data for updates

---

**Questions?** Open an issue or start a discussion. Happy researching!
