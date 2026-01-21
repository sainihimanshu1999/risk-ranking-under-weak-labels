# Decision-Oriented Fraud Risk Ranking under Weak Labels

## Overview

This project explores **fraud risk ranking under weak, noisy, and operationally constrained labeling regimes**, using large-scale transactional data.  
Rather than optimizing for global classification metrics (e.g. ROC-AUC), the focus is on **prioritization under limited review capacity**, reflecting real-world fraud operations.

The analysis demonstrates that:
- Fraud manifests in **multiple anomaly regimes**
- Global and local anomaly detectors surface **complementary risk signals**
- Model disagreement is **informative**, not noise
- Rank-based ensembling improves fraud capture without supervision
- Supervised models provide a strong upper bound but rely on strict assumptions

---

## Problem Framing

In practical fraud detection systems:

- Only a **small fraction** of transactions can be reviewed
- Labels are often **delayed, incomplete, or noisy**
- Decision quality depends on **ranking**, not binary classification

This project reframes fraud detection as a **decision-oriented ranking problem** under capacity constraints, rather than a traditional classification task.

---

## Dataset

- **Primary dataset:** IEEE-CIS Fraud Detection dataset  
- ~590K transactions  
- Fraud rate ≈ 3.5%  
- High dimensional (transactional, behavioral, device-level features)  
- Weak and incomplete labels  

---

## Methodology & Experiments

### 1. Exploratory Data Analysis (EDA)

Key findings:
- Fraud is **not globally extreme** in feature space
- Missingness and temporal structure carry signal
- Fraud clusters locally within peer groups
- Entity-level behavior differs significantly between fraud and non-fraud

These observations motivate **local anomaly detection** and ranking-based evaluation.

---

### 2. Isolation Forest (Global Anomalies)

- Captures **globally rare** behavior
- Effective at identifying extreme outliers
- Performs poorly at very tight review budgets due to legitimate rare behavior

Evaluated using **Precision@K**, aligned with operational review limits.

---

### 3. Local Outlier Factor (LOF) (Local Anomalies)

- Detects deviations **relative to local neighborhoods**
- Strong performance at tight review capacities
- Surfaces subtle, context-dependent fraud patterns

LOF consistently outperforms Isolation Forest at small K.

---

### 4. Disagreement Analysis (Core Insight)

Transactions are bucketed based on model agreement:

| Bucket | Interpretation |
|------|---------------|
| IF & LOF | Consensus anomalies |
| IF only | Globally rare but locally normal |
| LOF only | Locally anomalous but globally normal |
| Neither | Low-priority baseline |

**Key result:**  
The **LOF-only bucket exhibits the highest fraud concentration**, exceeding even the consensus bucket.

This demonstrates that:

> Model disagreement reveals distinct fraud regimes rather than noise.

---

### 5. Rank-Based Ensemble (Unsupervised)

Instead of averaging scores, models are combined using **rank aggregation**:

- Average rank
- Minimum rank (union-style)
- Weighted rank (LOF-weighted)

Results:
- Ensembles outperform individual models
- Precision@K improves substantially
- Fraud capture increases without collapsing precision
- Performance is robust to weight choice (no tuning required)

This shows that **disagreement can be exploited constructively**.

---

### 6. XGBoost Supervised Upper Bound

A supervised XGBoost model is trained on a temporal split to establish an **upper bound**.

Findings:
- Extremely high Precision@K (>90% at 0.5%)
- Demonstrates the value of strong labels
- Rankings exhibit reduced diversity

This experiment is **not a replacement**, but a reference point illustrating the trade-off between supervision and robustness.

---

## Key Takeaways

- Fraud is multi-regime: global and local anomalies matter
- Ranking quality under capacity constraints is the right objective
- Model disagreement carries actionable signal
- Rank-based ensembling improves coverage without supervision
- Supervised models perform best but require strong labeling assumptions

---

## Evaluation Philosophy

This project intentionally avoids:
- ROC-AUC optimization
- Threshold tuning
- Hyperparameter sweeps
- Kaggle-style leaderboard chasing

Instead, it emphasizes:
- Decision relevance
- Operational realism
- Interpretability
- Robustness under weak labels

---

## Why This Matters

Most real-world fraud systems:
- Cannot rely on clean labels
- Must operate under review constraints
- Require stable, interpretable prioritization

This project demonstrates how **unsupervised and semi-supervised methods can be evaluated and combined meaningfully** in such settings.

---

## Author

**Himanshu Saini**  
Data Scientist  
Focus: Decision-oriented ML, risk modeling, weakly supervised systems

---

## Final Notes

This repository is intended as a **thinking portfolio**, not a production system.  
All modeling choices are made to highlight *reasoning*, *trade-offs*, and *decision quality*.