# HealthChain Sensor Data - Data Dictionary

## Overview

This document provides detailed descriptions of all fields in the HealthChain Real-Time Sensor Dataset.

**Dataset:** HealthChain Real-Time Health Sensor Data
**Version:** 1.0
**Last Updated:** 2026-01-27
**Total Fields:** 14
**Total Records:** 183,736

## Field Definitions

### Identification Fields

#### `sensor_id`
- **Type:** String
- **Required:** Yes
- **Unique:** No (multiple records per sensor)
- **Description:** Anonymized sensor identifier using cryptographic hashing
- **Format:** 12-character hexadecimal string (uppercase)
- **Example:** "FFEC838A6903"
- **Anonymization:** SHA-256 hash of original sensor MAC address, truncated to 12 chars
- **Cardinality:** 3 unique sensors
- **Notes:** Cannot be reversed to identify original device or patient
- **Privacy:** GDPR compliant, no PII exposure

**Sensor Distribution:**
- FFEC838A6903: 139,688 records (76.0%)
- D7DF2CD97604: 34,548 records (18.8%)
- 14B05B22F4B4: 9,500 records (5.2%)

### Temporal Fields

#### `datetime`
- **Type:** Timestamp (datetime)
- **Required:** Yes
- **Unique:** No
- **Description:** Precise timestamp of sensor measurement in ISO 8601 format
- **Format:** YYYY-MM-DD HH:MM:SS.ffffff (microsecond precision)
- **Example:** "2025-07-05 08:00:00.019000"
- **Timezone:** UTC
- **Range:** 2025-07-05 07:00:00.000 to 2025-07-07 10:59:59.972
- **Notes:** Synthesized from date + hour + timestamp_ms for convenience
- **Null Values:** None

**Usage:**
```python
df['datetime'] = pd.to_datetime(df['datetime'])
df.set_index('datetime', inplace=True)
```

#### `date`
- **Type:** String (Date)
- **Required:** Yes
- **Unique:** No
- **Description:** Calendar date of measurement
- **Format:** YYYY-MM-DD
- **Example:** "2025-07-05"
- **Range:** 2025-07-05 to 2025-07-07
- **Unique Values:** 3 dates
- **Notes:** Extracted from directory structure during processing
- **Null Values:** None

#### `hour`
- **Type:** Integer
- **Required:** Yes
- **Unique:** No
- **Description:** Hour of day when measurement was taken (24-hour format)
- **Format:** Integer 0-23
- **Example:** 8, 14, 21
- **Range:** 7 to 15
- **Notes:** Used for temporal segmentation and hourly aggregation
- **Null Values:** None

**Recording Hours Distribution:**
- Most active hours: 10:00 (127,392 records), 15:00 (9,296 records)
- Quiet periods: 7:00 (2,320 records), 11:00 (484 records)

#### `timestamp_ms`
- **Type:** Integer
- **Required:** Yes
- **Unique:** No (within hour segment)
- **Description:** Milliseconds elapsed since the start of the hour
- **Format:** Non-negative integer
- **Example:** 0, 19, 38, 57, 75...
- **Range:** 0 to 3,599,990 (up to 1 hour in ms)
- **Resolution:** Typical intervals ~19ms (52 Hz sampling)
- **Notes:** Original timestamp from sensor, relative to hour start
- **Null Values:** None

**Sampling Characteristics:**
- Mean interval: 19.1 ms
- Median interval: 19 ms
- Sampling rate: ~52.4 Hz
- Jitter: ±2-3 ms (typical for Bluetooth LE)

### Accelerometer Fields

The 3-axis accelerometer measures linear acceleration (including gravity).

#### `AccX`
- **Type:** Float
- **Required:** Yes
- **Unit:** m/s² (meters per second squared)
- **Description:** Linear acceleration along X-axis (lateral/side-to-side)
- **Range:** -15.7 to +14.2 m/s²
- **Typical Values:** -5 to +5 m/s² during normal activity
- **Precision:** 3 decimal places
- **Example:** -4.099
- **Null Values:** None

**Physical Interpretation:**
- Positive: Acceleration to the right
- Negative: Acceleration to the left
- At rest: ~0 m/s² (unless tilted)

#### `AccY`
- **Type:** Float
- **Required:** Yes
- **Unit:** m/s² (meters per second squared)
- **Description:** Linear acceleration along Y-axis (vertical/up-down)
- **Range:** -18.8 to +12.3 m/s²
- **Typical Values:** -11 to -8 m/s² (standing, gravity dominates)
- **Precision:** 3 decimal places
- **Example:** -9.112
- **Null Values:** None

**Physical Interpretation:**
- Positive: Acceleration upward
- Negative: Acceleration downward
- At rest (horizontal): ~-9.81 m/s² (Earth's gravity)
- At rest (vertical): ~0 m/s²

#### `AccZ`
- **Type:** Float
- **Required:** Yes
- **Unit:** m/s² (meters per second squared)
- **Description:** Linear acceleration along Z-axis (forward-backward/anterior-posterior)
- **Range:** -14.6 to +13.9 m/s²
- **Typical Values:** -3 to +3 m/s² during normal activity
- **Precision:** 3 decimal places
- **Example:** -1.003
- **Null Values:** None

**Physical Interpretation:**
- Positive: Acceleration forward
- Negative: Acceleration backward
- At rest: ~0 m/s² (unless tilted)

**Accelerometer Applications:**
- **Fall Detection:** Sudden large changes (>3g = 29.4 m/s²)
- **Activity Recognition:** Walking (~1-3 m/s² variations), running (~3-6 m/s²)
- **Posture:** Y-axis ~-9.81 m/s² when vertical
- **Impact Events:** Sharp peaks indicate collisions or drops

### Gyroscope Fields

The 3-axis gyroscope measures angular velocity (rotation rate).

#### `GyroX`
- **Type:** Float
- **Required:** Yes
- **Unit:** deg/s (degrees per second)
- **Description:** Angular velocity around X-axis (roll/side-to-side rotation)
- **Range:** -30.4 to +31.9 deg/s
- **Typical Values:** -5 to +5 deg/s during normal movement
- **Precision:** 2 decimal places
- **Example:** 0.28
- **Null Values:** None

**Physical Interpretation:**
- Positive: Rolling clockwise (right shoulder down)
- Negative: Rolling counter-clockwise (left shoulder down)
- At rest: ~0 deg/s

#### `GyroY`
- **Type:** Float
- **Required:** Yes
- **Unit:** deg/s (degrees per second)
- **Description:** Angular velocity around Y-axis (pitch/forward-backward tilt)
- **Range:** -29.1 to +30.5 deg/s
- **Typical Values:** -5 to +5 deg/s during normal movement
- **Precision:** 2 decimal places
- **Example:** -1.61
- **Null Values:** None

**Physical Interpretation:**
- Positive: Pitching forward (looking down)
- Negative: Pitching backward (looking up)
- At rest: ~0 deg/s

#### `GyroZ`
- **Type:** Float
- **Required:** Yes
- **Unit:** deg/s (degrees per second)
- **Description:** Angular velocity around Z-axis (yaw/left-right turn)
- **Range:** -27.7 to +29.7 deg/s
- **Typical Values:** -5 to +5 deg/s during normal movement
- **Precision:** 2 decimal places
- **Example:** -0.42
- **Null Values:** None

**Physical Interpretation:**
- Positive: Turning right (yaw right)
- Negative: Turning left (yaw left)
- At rest: ~0 deg/s

**Gyroscope Applications:**
- **Rotation Detection:** Any non-zero value indicates rotation
- **Tremor Analysis:** Frequency analysis of gyro signals
- **Orientation Tracking:** Integrate angular velocity to get orientation
- **Gait Analysis:** Detect stride patterns and turns
- **Fall Detection:** Rapid rotation often precedes falls

### Magnetometer Fields

The 3-axis magnetometer measures magnetic field strength (Earth's magnetic field + nearby magnetic objects).

#### `MagnX`
- **Type:** Float
- **Required:** Yes
- **Unit:** μT (microtesla)
- **Description:** Magnetic field strength along X-axis
- **Range:** -110.3 to +112.8 μT
- **Typical Values:** -50 to +50 μT (varies by orientation)
- **Precision:** 3 decimal places
- **Example:** 7.138
- **Null Values:** None

**Physical Interpretation:**
- Depends on orientation relative to Earth's magnetic field
- Earth's field: ~25-65 μT (varies by latitude)
- Affected by nearby magnetic materials (beds, medical equipment)

#### `MagnY`
- **Type:** Float
- **Required:** Yes
- **Unit:** μT (microtesla)
- **Description:** Magnetic field strength along Y-axis
- **Range:** -176.9 to +45.6 μT
- **Typical Values:** -120 to -80 μT (common in dataset)
- **Precision:** 3 decimal places
- **Example:** -108.8
- **Null Values:** None

**Physical Interpretation:**
- Strong negative values suggest consistent orientation
- Large variations indicate rotation or magnetic interference

#### `MagnZ`
- **Type:** Float
- **Required:** Yes
- **Unit:** μT (microtesla)
- **Description:** Magnetic field strength along Z-axis
- **Range:** -162.3 to +48.9 μT
- **Typical Values:** -110 to -95 μT (common in dataset)
- **Precision:** 3 decimal places
- **Example:** -104.007
- **Null Values:** None

**Physical Interpretation:**
- Combined with X and Y, provides absolute heading
- Vertical component of Earth's magnetic field

**Magnetometer Applications:**
- **Compass Heading:** Calculate absolute orientation (0-360°)
- **Rotation Tracking:** Complement to gyroscope for drift correction
- **Magnetic Anomaly Detection:** Identify metallic objects or equipment
- **Sensor Fusion:** Combine with accelerometer + gyroscope for full orientation
- **Context Awareness:** Detect proximity to medical equipment

**Note on Hospital Environment:**
Magnetic fields in hospitals can be affected by:
- Metal bed frames and equipment
- Electrical wiring and motors
- Medical imaging equipment (if nearby)
- Building structure (steel reinforcement)

## Derived Measurements

Users can compute additional features from the raw sensor data:

### Magnitude Calculations

```python
# Total acceleration magnitude (remove gravity for movement analysis)
df['acc_magnitude'] = np.sqrt(df['AccX']**2 + df['AccY']**2 + df['AccZ']**2)

# Total rotation rate
df['gyro_magnitude'] = np.sqrt(df['GyroX']**2 + df['GyroY']**2 + df['GyroZ']**2)

# Total magnetic field strength
df['magn_magnitude'] = np.sqrt(df['MagnX']**2 + df['MagnY']**2 + df['MagnZ']**2)
```

### Orientation Estimation

```python
# Tilt angles from accelerometer (static orientation)
df['roll'] = np.arctan2(df['AccY'], df['AccZ']) * 180 / np.pi
df['pitch'] = np.arctan2(-df['AccX'], np.sqrt(df['AccY']**2 + df['AccZ']**2)) * 180 / np.pi

# Compass heading from magnetometer
df['heading'] = np.arctan2(df['MagnY'], df['MagnX']) * 180 / np.pi
```

### Activity Features

```python
# Signal variance (measure of activity level)
window = 52  # 1-second window at 52 Hz
df['acc_variance'] = df['acc_magnitude'].rolling(window).var()

# Zero-crossing rate (frequency of direction changes)
df['acc_zcr'] = (df['AccX'].diff().apply(np.sign).diff() != 0).rolling(window).sum()
```

## Data Quality Notes

### Completeness
- ✅ **100% Complete:** No missing values in any field
- ✅ **Consistent Sampling:** Regular ~19ms intervals (52 Hz)
- ✅ **No Outliers:** All values within sensor specification ranges

### Known Characteristics
- **Sampling Jitter:** ±2-3ms variation in sampling intervals (typical for Bluetooth LE)
- **Magnetic Interference:** Hospital environment creates complex magnetic fields
- **Gravity Component:** Accelerometer always includes 1g (~9.81 m/s²) gravity vector
- **Drift:** Gyroscope may show small drift over long periods (integrate with caution)

### Sensor Specifications (Sparky Health Sensor SHS-IMU9)

| Component | Range | Resolution | Noise |
|-----------|-------|------------|-------|
| Accelerometer | ±16g | 16-bit | <0.1 m/s² |
| Gyroscope | ±250 deg/s | 16-bit | <0.1 deg/s |
| Magnetometer | ±1300 μT | 16-bit | <1 μT |

## Common Transformations

### Filtering

```python
from scipy import signal

# Low-pass filter to remove high-frequency noise
b, a = signal.butter(4, 5, fs=52, btype='low')
df['AccX_filtered'] = signal.filtfilt(b, a, df['AccX'])
```

### Gravity Removal

```python
# High-pass filter to remove gravity (DC component)
b, a = signal.butter(4, 0.5, fs=52, btype='high')
df['AccX_nograv'] = signal.filtfilt(b, a, df['AccX'])
```

### Resampling

```python
# Resample to 10 Hz for lower-frequency analysis
df_resampled = df.set_index('datetime').resample('100ms').mean()
```

## File Formats

### CSV Format
- Encoding: UTF-8
- Line endings: Unix (LF)
- Header: Yes
- Delimiter: Comma
- Decimal: Period

### Parquet Format
- Engine: PyArrow
- Compression: Snappy
- Schema: Preserved dtypes (float64 for measurements, int64 for hour, string for IDs)

### JSON Format
- Encoding: UTF-8
- Structure: Array of objects
- Datetime: ISO 8601 strings

## Contact

For questions about the data schema or to report data quality issues:
- **GitHub Issues:** https://github.com/SparkyScience/sparky-open-datasets/issues
- **Label:** `healthchain` or `data-quality`

---

**Last Updated:** 2026-01-27
**Version:** 1.0
**Dataset:** HealthChain Real-Time Sensor Data
