## TL;DR (For Recruiters & Hiring Managers)

- Reframed fraud detection as a **risk-ranking problem under review constraints**
- Analyzed **global vs local anomaly detectors** (Isolation Forest, LOF)
- Showed that **model disagreement is informative**, not noise
- Built a **rank-based ensemble** that improves fraud capture without labels
- Demonstrated a **supervised XGBoost upper bound** as a reference, not a replacement
- Evaluated everything using **Precision@K**, aligned with real-world operations

This project emphasizes **decision quality, robustness, and reasoning**, not leaderboard metrics.


## How to Explain This Project in Interviews

### 1. What problem were you solving?
Traditional fraud models optimize ROC-AUC, but real fraud teams can only review a small fraction of transactions. I reframed the problem as **risk ranking under capacity constraints**, where Precision@K matters more than global accuracy.

---

### 2. Why anomaly detection instead of supervised models?
In many real systems, fraud labels are **delayed, incomplete, or noisy**. I focused on unsupervised methods that can prioritize risk without relying on strong labeling assumptions.

---

### 3. Why use both Isolation Forest and LOF?
They capture **different notions of abnormality**:
- Isolation Forest finds **globally rare behavior**
- LOF finds **locally inconsistent behavior**

Fraud often looks normal globally but abnormal relative to peers.

---

### 4. What was the most important insight?
The most valuable fraud signal appeared in **model disagreement**, especially transactions flagged by LOF but not by Isolation Forest. These locally anomalous cases had the highest fraud concentration.

---

### 5. How did you combine models?
I avoided score averaging and instead used **rank-based ensembling**, which:
- Preserves interpretability
- Avoids scale issues
- Works naturally under review limits

This significantly improved both precision and fraud capture.

---

### 6. Why include XGBoost if the focus is unsupervised?
XGBoost establishes an **upper bound**: how good ranking can get when labels are trusted. It performs much better but relies on strong assumptions and collapses diversity, which unsupervised methods preserve.

---

### 7. What would you do next in a real system?
- Use rank ensembles as a **front-line prioritizer**
- Feed reviewed outcomes back into supervised models
- Monitor disagreement regions for emerging fraud patterns