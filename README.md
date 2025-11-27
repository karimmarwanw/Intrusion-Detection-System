# Intrusion Detection System (IDS) using Machine Learning  
A complete machine learning pipeline for detecting cyber attacks using the CICIDS2017 dataset (~2.8M network flow records).  
The model classifies each network flow as:

- **0 → BENIGN**  
- **1 → ATTACK** (any malicious label)

This project focuses on data preprocessing, feature engineering, model training, and performance evaluation.

---

## 🚀 Project Features
- Combined 8 CSV files from CICIDS2017 into one dataset (≈2,830,000 rows)
- Cleaned and normalized all numeric features
- Engineered binary labels (BENIGN vs ATTACK)
- Handled missing values, infinities, and corrupted records
- Stratified train/test split to preserve class balance
- Standardized all numerical features using `StandardScaler`
- Trained a RandomForest classifier using all CPU cores
- Achieved **~100% accuracy, precision, recall, and F1-score**

---

                     ┌─────────────────────────────┐
                     │    Raw CICIDS2017 CSVs       │
                     │   (8 network flow datasets)  │
                     └──────────────┬──────────────┘
                                    │
                      Load & Merge (pandas, glob)
                                    │
                     ┌──────────────▼──────────────┐
                     │    Combined DataFrame        │
                     │   (~2.8M rows × 79 cols)     │
                     └──────────────┬──────────────┘
                                    │
                        Clean Column Names (strip)
                                    │
                     ┌──────────────▼──────────────┐
                     │     Label Inspection         │
                     │  BENIGN / DoS / DDoS / ...   │
                     └──────────────┬──────────────┘
                                    │
                  Binary Label Mapping (BENIGN=0, ATTACK=1)
                                    │
                     ┌──────────────▼──────────────┐
                     │   Clean Invalid Values       │
                     │  drop NaN, inf, -inf         │
                     └──────────────┬──────────────┘
                                    │
                       Train/Test Split (stratified)
                                    │
                     ┌──────────────▼──────────────┐
                     │     Feature Scaling          │
                     │   (StandardScaler)           │
                     └──────────────┬──────────────┘
                                    │
                      Train RandomForest Classifier
                                    │
                     ┌──────────────▼──────────────┐
                     │       Evaluation             │
                     │  Precision / Recall / F1     │
                     │  Confusion Matrix            │
                     └─────────────────────────────┘

---

## 🧠 Dataset — CICIDS2017
This dataset contains multiple attack scenarios including:

- DoS Hulk  
- GoldenEye  
- Heartbleed  
- PortScan  
- DDoS  
- Brute Force  
- Web Attacks (XSS, SQL Injection, Brute Force)  
- Botnet activity  
- Infiltration  

All non-BENIGN labels were mapped to a single **ATTACK** class.

---

## 🛠 Model Training Workflow (Simplified)

1. **Load all CSV files**
2. **Strip and sanitize column names**
3. **Convert all attack labels → 1**
4. **Convert BENIGN → 0**
5. **Remove NaN, inf, -inf**
6. **Train-test split (stratified)**
7. **Scale numerical features**
8. **Train RandomForest**
9. **Evaluate with precision, recall, F1, confusion matrix**

---

## 📊 Model Performance

The final model achieved:

- **Accuracy:** 1.00  
- **Precision:** 1.00  
- **Recall:** 1.00  
- **F1-Score:** 1.00  

### Confusion Matrix:

|                  | Predicted BENIGN | Predicted ATTACK |
|------------------|------------------|------------------|
| **Actual BENIGN** | 453,986          | 279              |
| **Actual ATTACK** | 308              | 111,003          |

---

## 🧪 Evaluation Code

```python
from sklearn.metrics import classification_report, confusion_matrix

preds = model.predict(X_test_scaled)

print(classification_report(y_test, preds))
print(confusion_matrix(y_test, preds))
