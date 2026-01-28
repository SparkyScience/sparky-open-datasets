<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="../../.github/assets/sparky_dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="../../.github/assets/sparky_light.svg">
    <img src="../../.github/assets/sparky_light.svg" alt="Sparky Logo" width="200"/>
  </picture>

  <h1>HealthChain Real-Time Sensor Data</h1>

  <p>
    <a href="https://creativecommons.org/licenses/by/4.0/"><img src="https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg" alt="License: CC BY 4.0"/></a>
    <a href="https://healthchain-i3.eu"><img src="https://img.shields.io/badge/Project-HealthChain-red" alt="HealthChain"/></a>
    <img src="https://img.shields.io/badge/size-183,736%20records-blue.svg" alt="Size"/>
    <img src="https://img.shields.io/badge/sensors-3-green.svg" alt="Sensors"/>
    <img src="https://img.shields.io/badge/GDPR-compliant-success.svg" alt="GDPR"/>
  </p>

  <p>
    <a href="#-overview"><strong>Overview</strong></a> •
    <a href="#-clinical-context"><strong>Clinical Context</strong></a> •
    <a href="#-data-files"><strong>Data</strong></a> •
    <a href="#-usage-examples"><strong>Examples</strong></a> •
    <a href="#-citation"><strong>Citation</strong></a>
  </p>
</div>

---

## 📋 Overview

This dataset contains **183,736 high-frequency sensor measurements** from real-time health monitoring using **Sparky Health Sensors** (certified medical devices, Class IIa) deployed at **KBC Rijeka Hospital** in Croatia.

### HealthChain Project

The data was collected as part of the **HealthChain** project, an EU co-funded initiative that aimed to strengthen healthcare value chains at both regional and EU levels by encouraging collaboration and the use of data-driven technologies.

The HealthChain project brought together partners from across Europe to improve how health data is shared, analyzed, and applied, supporting more efficient and better-connected healthcare systems. **Sparky*** served as a consortium partner, contributing to the project's analytical and technical work, with a focus on how data science and Health IoT can enhance transparency, interoperability, and cooperation among healthcare institutions.

Through applied data science and AI, Sparky* supported the development of approaches for secure data exchange, informed decision-making, and the integration of digital tools that assist healthcare professionals and policymakers. This dataset represents one of the key outcomes: real-world clinical sensor data that can drive innovation in digital health monitoring and patient care.

**Key Statistics:**
- **Total Records:** 183,736 measurements (Subset of original Dataset)
- **Sensors:** 3 Sparky Health Sensor SHS-IMU9 devices (3/8 used in the project)
- **Sampling Rate:** ~52 Hz (high-frequency real-time monitoring)
- **Recording Period:** July 5-7, 2025 (2 days)
- **Total Recording Hours:** 17 hours across multiple sessions
- **Data Types:** 9-axis IMU data (accelerometer, gyroscope, magnetometer)
- **Clinical Study:** 55 patients, 3-month study period, average 1-week hospital stay per patient

## 🏥 Clinical Context

### Study Design
- **Study Name:** HealthChain Real-Time Patient Monitoring and Fall Detection Study
- **Institution:** KBC Rijeka Hospital, Croatia
- **Study Duration:** 3 months (July - September 2025)
- **Patients Enrolled:** 55 patients
- **Average Hospital Stay:** 1 week per patient
- **Sensors Deployed:** 1 sensor per patient (wearable format)
- **Data Processing:** In-situ edge computing with gateway-based data aggregation
- **Ethical Approval:** Full institutional review board approval obtained

### Sensor Technology
**Sparky Health Sensor SHS-IMU9**
- **Certification:** Medical Device Class IIa (CE marked)
- **Technology:** 9-axis Inertial Measurement Unit (IMU)
- **Sensors:**
  - 3-axis Accelerometer: Measures acceleration and movement
  - 3-axis Gyroscope: Measures angular velocity and orientation
  - 3-axis Magnetometer: Measures magnetic field for absolute heading
- **Sampling Rate:** ~52 Hz (configurable 13-208 Hz)
- **Power:** Low-power Bluetooth LE connectivity
- **Form Factor:** Lightweight wearable device (< 50g)
- **Battery Life:** 7+ days continuous monitoring
- **Data Transmission:** Real-time streaming to edge gateways

### Data Collection Architecture
The study employed an **in-situ private processing** architecture to ensure patient privacy and GDPR compliance:

1. **Sensor Layer:** Sparky Health Sensors worn by patients
2. **Edge Gateway Layer:** Local data aggregation and preprocessing at hospital
3. **Privacy Layer:** Anonymization and de-identification before external storage
4. **Storage Layer:** Secure encrypted storage with access controls

**Key Privacy Features:**
- No raw patient identifiable data leaves hospital premises
- Real-time anonymization at edge gateways
- Sensor IDs cryptographically anonymized
- No PHI (Protected Health Information) included in dataset
- GDPR Article 89 compliant (research exemptions)

## 🎯 Research Applications

This dataset is valuable for:

### Healthcare & Medical Research
- **Fall Detection:** Train ML models to detect falls and movement abnormalities
- **Gait Analysis:** Study walking patterns and mobility metrics
- **Activity Recognition:** Classify patient activities (lying, sitting, walking, standing)
- **Tremor Detection:** Identify and quantify tremors in neurological conditions
- **Post-operative Monitoring:** Track patient recovery and mobility progression
- **Sleep Quality:** Analyze sleep patterns and disturbances

### Machine Learning & AI
- **Time Series Classification:** Activity and movement pattern recognition
- **Anomaly Detection:** Identify unusual patient behavior or health events
- **Signal Processing:** Filter noise, extract features, and analyze frequency spectra
- **Sensor Fusion:** Combine accelerometer, gyroscope, and magnetometer data
- **Transfer Learning:** Pre-train models on this data for other health applications
- **Real-time Prediction:** Develop algorithms for continuous patient monitoring

### Biomedical Engineering
- **Wearable Device Development:** Benchmark and validate new sensor designs
- **Signal Quality Assessment:** Study sensor performance in clinical environments
- **Power Optimization:** Analyze optimal sampling rates for battery life
- **Calibration Methods:** Develop sensor calibration and drift correction techniques

### Digital Health & Telemedicine
- **Remote Patient Monitoring:** Design RPM systems and protocols
- **Clinical Decision Support:** Build early warning systems for patient deterioration
- **Rehabilitation Programs:** Create personalized physical therapy protocols
- **Hospital Workflow:** Optimize patient monitoring and staff allocation

## 📊 Data Files

| File | Format | Size | Records | Description |
|------|--------|------|---------|-------------|
| `healthchain_sensor_data.csv` | CSV | ~18 MB | 183,736 | Complete dataset (all sensors, all timepoints) |
| `healthchain_sensor_data.parquet` | Parquet | ~3 MB | 183,736 | Compressed columnar format (recommended) |
| `healthchain_sensor_data_sample.json` | JSON | ~2.5 MB | 10,000 | Sample for exploration (first 10k records) |
| `sensor_mapping.json` | JSON | <1 KB | 3 | Anonymized sensor-to-patient mapping |
| `by_sensor/sensor_*.parquet` | Parquet | - | Varies | Individual sensor data files |

## 📖 Data Schema

Each sensor record contains:

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `sensor_id` | string | Anonymized sensor identifier | "FFEC838A6903" |
| `datetime` | timestamp | Precise measurement timestamp | "2025-07-05 08:00:00.019" |
| `date` | string | Measurement date | "2025-07-05" |
| `hour` | integer | Hour of day (0-23) | 8 |
| `timestamp_ms` | integer | Milliseconds since hour start | 19 |
| `AccX` | float | Accelerometer X-axis (m/s²) | -4.099 |
| `AccY` | float | Accelerometer Y-axis (m/s²) | -9.112 |
| `AccZ` | float | Accelerometer Z-axis (m/s²) | -1.003 |
| `GyroX` | float | Gyroscope X-axis (deg/s) | 0.28 |
| `GyroY` | float | Gyroscope Y-axis (deg/s) | -1.61 |
| `GyroZ` | float | Gyroscope Z-axis (deg/s) | -0.42 |
| `MagnX` | float | Magnetometer X-axis (μT) | 7.138 |
| `MagnY` | float | Magnetometer Y-axis (μT) | -108.8 |
| `MagnZ` | float | Magnetometer Z-axis (μT) | -104.007 |

**Sensor Axes Orientation:**
- X-axis: Lateral (left-right)
- Y-axis: Longitudinal (up-down)
- Z-axis: Anterior-posterior (front-back)

**Measurement Units:**
- Accelerometer: m/s² (meters per second squared)
- Gyroscope: deg/s (degrees per second)
- Magnetometer: μT (microtesla)

[**➡️ View Full Data Dictionary**](data_dictionary.md)

## 🔍 Data Quality & Characteristics

### Strengths
- ✅ **High Frequency:** 52 Hz sampling enables fine-grained movement analysis
- ✅ **Multi-modal:** 9-axis IMU provides rich feature space
- ✅ **Real-world Clinical Data:** Collected in actual hospital environment
- ✅ **Medical-grade Sensors:** Certified devices with calibrated measurements
- ✅ **No Missing Values:** Complete data for all 183,736 records
- ✅ **Synchronized Timing:** Precise timestamps across all sensors
- ✅ **Privacy Protected:** Fully anonymized and GDPR compliant

### Limitations
- ⚠️ **Limited Temporal Scope:** 2-day sample from 3-month study
- ⚠️ **Small Sensor Count:** 3 sensors (representative subset of 55-patient study)
- ⚠️ **No Clinical Labels:** No diagnoses, outcomes, or clinical annotations included
- ⚠️ **No Demographics:** Patient age, gender, condition not disclosed (privacy)
- ⚠️ **Environmental Variation:** Hospital-only setting (not ambulatory/home)

## 💻 Usage Examples

### Load and Explore

```python
import pandas as pd
import numpy as np

# Load dataset (Parquet is fastest and most efficient)
df = pd.read_parquet('healthchain_sensor_data.parquet')

# Basic exploration
print(f"Total records: {len(df):,}")
print(f"Sensors: {df['sensor_id'].nunique()}")
print(f"Date range: {df['datetime'].min()} to {df['datetime'].max()}")
print(f"Sampling rate: {1000 / df['timestamp_ms'].diff().mean():.1f} Hz")

# View first few records
print(df.head())
```

### Activity Detection Example

```python
import pandas as pd
import numpy as np
from scipy import signal

# Load data
df = pd.read_parquet('healthchain_sensor_data.parquet')

# Calculate acceleration magnitude (total movement)
df['acc_magnitude'] = np.sqrt(df['AccX']**2 + df['AccY']**2 + df['AccZ']**2)

# Simple activity classification based on movement intensity
df['activity'] = pd.cut(df['acc_magnitude'],
                         bins=[0, 9.5, 10.5, 20],
                         labels=['resting', 'standing/sitting', 'moving'])

# Count activities
print(df['activity'].value_counts())
```

### Fall Detection Features

```python
import pandas as pd
import numpy as np

# Load data
df = pd.read_parquet('healthchain_sensor_data.parquet')

# Calculate features commonly used in fall detection
df['acc_magnitude'] = np.sqrt(df['AccX']**2 + df['AccY']**2 + df['AccZ']**2)
df['gyro_magnitude'] = np.sqrt(df['GyroX']**2 + df['GyroY']**2 + df['GyroZ']**2)

# Detect sudden changes (potential falls)
df['acc_change'] = df['acc_magnitude'].diff().abs()
df['gyro_change'] = df['gyro_magnitude'].diff().abs()

# Identify high-impact events with realistic thresholds
# Falls typically show: sudden acceleration change AND high rotation AND impact
fall_candidates = df[
    ((df['acc_change'] > 15) & (df['gyro_change'] > 100)) |  # Sudden change + rotation
    (df['acc_magnitude'] > 30)  # OR very high impact (>3g)
]
print(f"Detected {len(fall_candidates)} potential fall events")

# Analyze fall characteristics
if len(fall_candidates) > 0:
    print(f"Average peak acceleration: {fall_candidates['acc_magnitude'].mean():.2f} m/s²")
    print(f"Average rotation rate: {fall_candidates['gyro_magnitude'].mean():.2f} deg/s")
```

### Time Series Visualization

```python
import pandas as pd
import matplotlib.pyplot as plt

# Load data for one sensor
df = pd.read_parquet('by_sensor/sensor_FFEC838A6903.parquet')

# Plot 10 seconds of data
sample = df.iloc[:520]  # 52 Hz * 10 seconds

fig, axes = plt.subplots(3, 1, figsize=(12, 8), sharex=True)

# Accelerometer
axes[0].plot(sample['datetime'], sample['AccX'], label='X')
axes[0].plot(sample['datetime'], sample['AccY'], label='Y')
axes[0].plot(sample['datetime'], sample['AccZ'], label='Z')
axes[0].set_ylabel('Acceleration (m/s²)')
axes[0].legend()
axes[0].set_title('Accelerometer Data')

# Gyroscope
axes[1].plot(sample['datetime'], sample['GyroX'], label='X')
axes[1].plot(sample['datetime'], sample['GyroY'], label='Y')
axes[1].plot(sample['datetime'], sample['GyroZ'], label='Z')
axes[1].set_ylabel('Angular Velocity (deg/s)')
axes[1].legend()
axes[1].set_title('Gyroscope Data')

# Magnetometer
axes[2].plot(sample['datetime'], sample['MagnX'], label='X')
axes[2].plot(sample['datetime'], sample['MagnY'], label='Y')
axes[2].plot(sample['datetime'], sample['MagnZ'], label='Z')
axes[2].set_ylabel('Magnetic Field (μT)')
axes[2].set_xlabel('Time')
axes[2].legend()
axes[2].set_title('Magnetometer Data')

plt.tight_layout()
plt.show()
```

### Frequency Domain Analysis

```python
import pandas as pd
import numpy as np
from scipy import signal
import matplotlib.pyplot as plt

# Load data
df = pd.read_parquet('by_sensor/sensor_FFEC838A6903.parquet')

# Select 1-minute continuous segment
minute_data = df.iloc[:3120]  # 52 Hz * 60 seconds

# Calculate power spectral density
fs = 52  # Sampling frequency
f, psd = signal.welch(minute_data['AccX'], fs=fs, nperseg=256)

# Plot frequency spectrum
plt.figure(figsize=(10, 6))
plt.semilogy(f, psd)
plt.xlabel('Frequency (Hz)')
plt.ylabel('Power Spectral Density (m²/s⁴/Hz)')
plt.title('Accelerometer X-Axis Frequency Spectrum')
plt.grid(True)
plt.show()

# Identify dominant frequencies
dominant_freq = f[np.argmax(psd)]
print(f"Dominant frequency: {dominant_freq:.2f} Hz")
```

## 📚 Citation

If you use this dataset in your research, please cite:

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

**APA Format:**
Sparky solution d.o.o. (2026, January). *HealthChain Real-Time Health Sensor Dataset* (Version 1.0) [Dataset]. GitHub. https://github.com/SparkyScience/sparky-open-datasets/tree/main/datasets/healthchain-sensor-data

**MLA Format:**
Sparky solution d.o.o. "HealthChain Real-Time Health Sensor Dataset." Version 1.0, GitHub, Jan. 2026, https://github.com/SparkyScience/sparky-open-datasets/tree/main/datasets/healthchain-sensor-data.

## ⚖️ Privacy & Ethical Compliance

### Privacy Protection
This dataset has been **fully anonymized** and complies with:
- ✅ **GDPR** (General Data Protection Regulation)
- ✅ **EU AI Act** provisions for health data
- ✅ **HIPAA** de-identification standards (Safe Harbor method)
- ✅ **ISO 27001** information security standards

**Anonymization Measures:**
1. **Sensor IDs:** Cryptographically hashed (SHA-256) before release
2. **No PHI:** No names, dates of birth, addresses, or medical record numbers
3. **No Clinical Data:** No diagnoses, medications, or treatment information
4. **Aggregated Timeframes:** Precise admission/discharge times removed
5. **K-Anonymity:** Dataset structure ensures k ≥ 3 for all quasi-identifiers

### Data Processing Location
- **Collection:** KBC Rijeka Hospital, Croatia
- **Anonymization:** Edge gateways within hospital secure network
- **Storage:** De-identified data only, encrypted at rest and in transit

### Usage Terms
Users of this dataset must:
- ✓ Not attempt to re-identify individual patients
- ✓ Use data for research, education, or innovation purposes only
- ✓ Not use data for commercial surveillance or patient tracking
- ✓ Provide appropriate attribution (see Citation above)
- ✓ Report any potential privacy concerns to dataset maintainers

## 🎓 Funding & Acknowledgments

### HealthChain Project

This dataset was collected as part of the **HealthChain** project, co-funded by the European Union.

**Project Overview:**
The HealthChain initiative aimed to strengthen healthcare value chains at both regional and EU levels by encouraging collaboration and the use of data-driven technologies. The project brought together partners from across Europe to improve how health data is shared, analyzed, and applied, supporting more efficient and better-connected healthcare systems.

**Disclaimer:** Views and opinions expressed in this dataset and documentation are those of the author(s) only and do not necessarily reflect those of the European Union or the relevant granting authority. Neither the European Union nor the granting authority can be held responsible for them.

### Project Partners

**Sparky***
As a consortium partner, Sparky* contributed to the project's analytical and technical work. The team focused on ways data science can enhance transparency, interoperability, and cooperation among healthcare institutions.

**KBC Rijeka Hospital**
Special thanks to the medical staff, administration, and patients at KBC Rijeka Hospital for their invaluable collaboration and support in conducting this research.

**Additional Partners:**
- Consector Biro
- Sustainable Solutions

### Key Outcomes
The HealthChain project contributed to:
* Improved mechanisms for secure and transparent health data exchange
* Closer collaboration between healthcare, research, and technology sectors
* A foundation for future initiatives supporting digital health transformation across Europe
* Demonstrated value of collaboration and data-driven methods in improving healthcare systems

## 📞 Contact

For questions, issues, or collaboration opportunities:

- **GitHub Issues:** [Report an issue](https://github.com/SparkyScience/sparky-open-datasets/issues)
- **Email:** [info@sparky.science](mailto:info@sparky.science)
- **Sparky* Website:** [sparky.science](https://sparky.science)
- **Project Website:** [HealthChain](https://healthchain-i3.eu)
- **Institution:** [KBC Rijeka Hospital](https://kbc-rijeka.hr)


## 📜 License

This dataset is licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

**You are free to:**
- ✓ Share — copy and redistribute the material
- ✓ Adapt — remix, transform, and build upon the material for any purpose, even commercially

**Under the following terms:**
- ⚠️ **Attribution** — You must give appropriate credit, provide a link to the license, and indicate if changes were made

**Code and scripts:** MIT License

---

<div align="center">

**Part of the [Sparky Open Datasets](../../) collection**

[← Back to All Datasets](../../README.md) | [Data Dictionary](data_dictionary.md) | [Report Issue](https://github.com/SparkyScience/sparky-open-datasets/issues)

<br><br>

## 🌐 Project Information

<a href="https://healthchain-i3.eu" target="_blank">
  <img src="../../.github/assets/healthchain/healthchain_main.png" alt="HealthChain Project" height="60"/>
</a>

<p>
  <strong><a href="https://healthchain-i3.eu" target="_blank">Visit HealthChain Project Website →</a></strong>
</p>

<p>
  <em>
    Co-funded by the European Union under grant agreement No. 101094676<br>
    Strengthening healthcare value chains through data-driven technologies
  </em>
</p>

<br>

## 🤝 Project Partners

<table>
<tr align="center">
<td>
  <img src="../../.github/assets/healthchain/kbcrijeka.jpg" alt="KBC Rijeka Hospital" height="60"/><br>
  <sub><strong>KBC Rijeka Hospital</strong><br>Clinical Partner</sub>
</td>
<td>
  <img src="../../.github/assets/healthchain/consector.png" alt="Consector Biro" height="60"/><br>
  <sub><strong>Consector Biro d.o.o.</strong><br>Project Partner</sub>
</td>
</tr>
</table>

<br>

## 💶 Funding

<table>
<tr align="center">
<td width="50%">
  <img src="../../.github/assets/healthchain/I3_funding.png" alt="I3 Funding" height="80"/>
</td>
<td width="50%">
  <img src="../../.github/assets/healthchain/co-funded.png" alt="EU Co-funded" height="80"/>
</td>
</tr>
</table>

<p>
  <em>
    Views and opinions expressed are those of the author(s) only<br>
    and do not necessarily reflect those of the European Union or the granting authority.<br>
    Neither the European Union nor the granting authority can be held responsible for them.
  </em>
</p>

<br>

---

<p>
  <strong>Made with ❤️ by <a href="https://sparky.science">Sparky*</a></strong><br>
  <em>In collaboration with KBC Rijeka Hospital and project partners</em><br>
  <em>Advancing digital health through open data and collaboration</em>
</p>

</div>
