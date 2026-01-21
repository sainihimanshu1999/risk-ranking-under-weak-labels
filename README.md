# risk-ranking-under-weak-labels

Decision-oriented risk ranking under weak and noisy labels, focusing on ranking stability, evaluation under fixed review capacity constraints, and failure modes of anomaly detection systems in transactional data.

## Overview
This project studies how anomaly detection models behave as **risk ranking systems** under **weak and noisy labels**, rather than as pure classifiers.

Using the IEEE-CIS transactional dataset, the focus is on evaluating model behavior under **realistic operational constraints**, where only a limited fraction of transactions can be reviewed. Although fraud is not ultra-rare in this dataset (~3.5%), label noise, delayed feedback, and review capacity limitations still necessitate ranking-based decision systems.

The analysis emphasizes **ranking stability, threshold sensitivity, and failure modes**, rather than headline metrics such as ROC-AUC. Unsupervised and semi-supervised anomaly detection methods are compared as risk rankers. A supervised XGBoost model is included strictly as an **upper-bound reference** to quantify the value of labels, not as a deployable alternative. Key conclusions are validated on a benchmark dataset to assess robustness across data regimes.
