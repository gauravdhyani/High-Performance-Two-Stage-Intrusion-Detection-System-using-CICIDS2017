# Two-Stage Network Intrusion Detection System using CICIDS2017

---

# Project Aim

The aim of this project is to build a **high-performance Network Intrusion Detection System (IDS)** that can detect malicious traffic and classify attack types using **machine learning techniques**.

The system is designed as a **two-stage detection pipeline**:

1. **Stage 1 – Attack Detection**  
   Classify network traffic as **Benign or Malicious**

2. **Stage 2 – Attack Classification**  
   Identify the **specific type of cyber attack**

The project uses the **CICIDS2017 dataset**, one of the most realistic and comprehensive datasets available for intrusion detection research.

---

# Why a Two-Stage IDS?

Most IDS systems attempt to perform **multi-class attack classification directly**, which creates problems:

- Class imbalance
- Poor performance on rare attacks
- High computational cost

A **two-stage architecture** solves this problem:

### Stage 1 – Lightweight Detector
Quickly filters **normal vs malicious traffic**.

### Stage 2 – Deep Attack Classifier
Analyzes **only malicious traffic** to determine the attack type.

Benefits:

- Faster detection
- Higher accuracy
- More scalable architecture
- Suitable for real-time deployment

---

# Dataset

This project uses the **CICIDS2017 Intrusion Detection Dataset**.

Dataset contains **realistic network traffic flows** generated in a controlled environment including:

- Benign traffic
- DoS attacks
- DDoS attacks
- Port scanning
- Web attacks
- Botnet activity
- Infiltration attacks
- Brute force attacks

### Dataset Statistics

| Property | Value |
|--------|------|
| Dataset | CICIDS2017 |
| Total Records | **2,830,743 flows** |
| Features | **79 network flow features** |
| Attack Types | **14 attack classes** |

Input files used:

```
Monday-WorkingHours.pcap_ISCX.csv
Tuesday-WorkingHours.pcap_ISCX.csv
Wednesday-workingHours.pcap_ISCX.csv
Thursday-WorkingHours-Morning-WebAttacks.pcap_ISCX.csv
Thursday-WorkingHours-Afternoon-Infilteration.pcap_ISCX.csv
Friday-WorkingHours-Morning.pcap_ISCX.csv
Friday-WorkingHours-Afternoon-PortScan.pcap_ISCX.csv
Friday-WorkingHours-Afternoon-DDos.pcap_ISCX.csv
```

---

# Methodology

## Data Processing

Steps performed:

1. Load multiple CSV files from CICIDS2017
2. Merge into a single dataset
3. Handle missing and infinite values
4. Encode labels for machine learning
5. Clean numeric features

Final dataset size:

```
2,830,743 network flows
79 features
```

---

# Exploratory Data Analysis (EDA)

EDA includes:

- Feature distribution analysis
- Correlation heatmaps
- Variance analysis

This helps identify:

- redundant features
- highly correlated features
- informative features for attack detection

---

# Feature Selection

Feature selection was applied to reduce noise and improve performance.

Techniques used:

### 1. Variance Threshold
Removes features with extremely low variance.

### 2. ANOVA Feature Selection

Using:

```
SelectKBest(f_classif)
```

Selected feature sets:

| Feature Set | Number of Features |
|-------------|-------------------|
| Stage 1 | Top **30** |
| Stage 2 | Top **30** |
| Stage 2 | Top **50** |
| Stage 2 | Top **60** |

Important high-variance features include:

- Flow Duration
- Flow IAT statistics
- Packet Length statistics
- Idle time metrics
- Packet flag counts

---

# Stage 1 – Attack Detection (Binary Classification)

Goal:

```
Benign → 0
Attack → 1
```

Dataset size used:

```
~2.8 million network flows
```

Models tested:

- Logistic Regression
- Random Forest
- Extra Trees
- LightGBM

### Stage 1 Model Performance

| Model | Recall | Accuracy | Training Time |
|------|------|------|------|
| **LightGBM** | **0.976** | **0.985** | 46s |
| Random Forest | 0.884 | 0.976 | 251s |
| Extra Trees | 0.777 | 0.950 | 91s |
| Logistic Regression | 0.758 | 0.924 | 141s |

### Best Model

**LightGBM**

Reasons:

- Highest recall
- Fastest training
- High scalability

High recall is important because **missing attacks is dangerous**.

---

# Stage 2 – Attack Classification (Multi-Class)

Stage 2 processes **only malicious traffic**.

Dataset size:

```
557,646 attack flows
```

Attack classes:

```
14 attack types
```

Examples:

- DDoS
- PortScan
- Bot
- Web Attack
- Infiltration
- SQL Injection
- Heartbleed

---

# Models Evaluated

- LightGBM
- Random Forest
- Extra Trees

Feature sets tested:

- Top 30
- Top 50
- Top 60

---

# Stage 2 Results

All models achieved **very high classification performance**.

| Model | Feature Set | Accuracy |
|------|------|------|
| LightGBM | Top60 | **1.000** |
| RandomForest | Top60 | 0.999 |
| ExtraTrees | Top50 | 0.999 |

These results show that **network flow statistics strongly separate attack types**.

---

# Key Insights

### 1. Feature Engineering Matters

Packet length statistics and inter-arrival times are extremely informative.

### 2. Two-Stage Architecture Works

Separating detection and classification improves:

- speed
- accuracy
- scalability

### 3. Tree-based Models Work Best

Models like:

- LightGBM
- Random Forest
- Extra Trees

perform better than linear models on network traffic data.

---

# Project Architecture

```
Network Traffic
      │
      ▼
Feature Extraction (CICFlowMeter)
      │
      ▼
Stage 1: Attack Detector
(Benign vs Attack)
      │
      ▼
Stage 2: Attack Classifier
(DDoS / PortScan / WebAttack / etc)
      │
      ▼
Security Alert
```

---

# Tech Stack

- **Python**
- **Pandas**
- **NumPy**
- **Scikit-learn**
- **LightGBM**
- **Matplotlib**
- **Seaborn**

Environment:

```
Kaggle Notebook
Python 3
```

---

# Potential Improvements

Future work could include:

- Real-time IDS deployment
- Deep learning models (CNN, LSTM)
- Cross-dataset evaluation (CSE-CIC-IDS2018)
- Adversarial attack detection
- Streaming IDS pipelines

---

# Use Cases

This project can be used for:

- Network security monitoring
- Intrusion detection research
- Cybersecurity ML benchmarking
- Real-time IDS prototypes
- Security analytics systems

---

# References

CICIDS2017 Dataset  
https://www.unb.ca/cic/datasets/ids-2017.html

---

# License

MIT License
