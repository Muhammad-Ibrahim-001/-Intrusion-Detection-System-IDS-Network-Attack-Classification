# 🛡️ Intrusion Detection System (IDS) using Machine Learning

**Dataset: UNSW-NB15**

---

## 📌 Project Overview

This project implements a Machine Learning-based Intrusion Detection System (IDS) using the **UNSW-NB15 dataset**.

The system performs:
- Binary Classification (Normal vs Attack)
- Multi-Class Classification (Different attack categories)
- Feature Importance Analysis
- Performance Comparison of Ensemble Models

The models used:
- Random Forest
- XGBoost

---

## 🗂️ Dataset Used — UNSW-NB15

This project uses the **UNSW-NB15 dataset**, a modern and realistic network intrusion dataset created in 2015.

### About the Dataset

The UNSW-NB15 dataset was created by:
- The Australian Centre for Cyber Security (ACCS)
- University of New South Wales (UNSW Canberra)

It was generated using:
- IXIA PerfectStorm tool
- Real modern normal traffic
- Synthetic attack traffic

It was designed to overcome limitations of older datasets like:
- KDD99
- NSL-KDD

### Dataset Characteristics

| Property | Value |
|---|---|
| Total Features | 45 |
| Total Records | ~257,000 |
| Training Set | 175,341 records |
| Testing Set | 82,332 records |

Additional characteristics:
- Includes both normal and malicious network traffic
- Contains modern attack behaviors
- Realistic and balanced feature representation

### Target Variables

The dataset contains two important target columns:

- **`label`**
  - `0` → Normal traffic
  - `1` → Attack traffic

- **`attack_cat`** — Multi-class attack category

Attack categories include:
`Generic`, `Exploits`, `Fuzzers`, `DoS`, `Reconnaissance`, `Backdoor`, `Analysis`, `Shellcode`, `Worms`

### Feature Types

The dataset includes:
- Basic connection features (duration, protocol, service, state)
- Content features (packets, bytes, TTL, load)
- Statistical traffic features
- Flow-based features

**Important predictive features discovered in this project:**
`sbytes`, `dbytes`, `sttl`, `rate`, `sload`, `dload`, `dur`, `dmean`

---

## ⚙️ Project Workflow

### 1️⃣ Data Inspection & Cleaning

- Checked dataset structure using `head()`, `info()`, `describe()`
- Verified no missing values
- Checked duplicate records
- Validated data types

**Result:** No missing values | No duplicates

### 2️⃣ Exploratory Data Analysis (EDA)

Performed analysis on:
- Protocol distribution
- Service distribution
- State distribution
- Attack category distribution

**Tools used:** Matplotlib, Seaborn, Plotly

**Key findings:**
- Dataset contains class imbalance
- Some attack categories are rare
- Certain protocols dominate the traffic

### 3️⃣ Feature Engineering & Encoding

- Removed unnecessary columns (`id`)
- Label Encoding applied to `proto`
- One-Hot Encoding applied to `service` and `state`
- Aligned train and test datasets

**Final feature size after encoding:**
- Training → `(175341, 64)`
- Testing → `(82332, 64)`

---

## 🤖 Model Implementation

### 🔷 Binary Classification (Normal vs Attack)

#### ✅ Random Forest

- **Accuracy: 87.29%**
- Very high recall for attack class (0.99)
- Good overall generalization

**Confusion Matrix:**
```
[[27208  9792]
 [  676 44656]]
```

#### ✅ XGBoost

**Parameters:**
- `n_estimators = 100`
- `learning_rate = 0.1`
- `eval_metric = logloss`

**Accuracy: 87.33%**

**Observation:** Slightly better than Random Forest | Strong performance on attack detection

---

### 🔷 Multi-Class Classification (Attack Categories)

- **Model Used:** Optimized Random Forest
- **Accuracy: 82.72%**

**Performance Summary:**

| Attack Category | Performance |
|---|---|
| Generic | Very High |
| Normal | Strong |
| Exploits | Good |
| Fuzzers | Moderate |
| DoS | Moderate |
| Analysis | Low (Imbalanced class) |
| Backdoor | Low (Rare class) |

> Binary classification performs better than multi-class classification due to class imbalance.

---

## 📊 Final Results Summary

| Model | Task | Accuracy |
|---|---|---|
| Random Forest | Binary Classification | 87.29% |
| XGBoost | Binary Classification | 87.33% |
| Random Forest | Multi-Class Classification | 82.72% |

---

## 🔍 Key Observations

- Ensemble models perform well for IDS tasks
- Feature importance plays a critical role
- Class imbalance affects rare attack detection
- XGBoost slightly outperforms Random Forest

---

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Python 3.x | Core language |
| Pandas | Data manipulation |
| NumPy | Numerical computing |
| Matplotlib | Visualization |
| Seaborn | Statistical visualization |
| Plotly | Interactive visualization |
| Scikit-Learn | ML models & preprocessing |
| XGBoost | Gradient boosting model |

---

## 🚀 Conclusion

This project successfully builds a Machine Learning-based Intrusion Detection System using the UNSW-NB15 dataset.

The system:
- Detects malicious traffic with **~87% accuracy**
- Classifies attack categories with **~82% accuracy**
- Identifies important network features responsible for attacks

This demonstrates that ensemble learning methods are effective for modern intrusion detection systems.
