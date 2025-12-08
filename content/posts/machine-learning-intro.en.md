---
title: "My Machine Learning Starter Guide"
date: 2025-03-13T14:30:00+08:00
language: en
draft: false
categories:
- Technical Discussion
tags:
- Machine Learning
- Artificial Intelligence
keywords:
- Supervised Learning
- Unsupervised Learning
- Deep Learning
---



**▌What's preprocessing for?**
Real-world data is messy: missing values, noise, inconsistent units (e.g., height 1.8m vs. weight 80kg). Feeding raw data to models = guaranteed failure.
**Must-have toolkit**:
```bash
pip install scikit-learn
```

---

**Normalization**
**Case 1: Min-Max Scaling**
- **Formula**: (Current value - column min) / (column max - min)
- **Best for**: Uniformly distributed data (e.g., pixel values 0-255)
- **Weakness**: A 3-meter-tall giant? All values get squashed below 0.5.

**Case 2: Standardization**
- **Formula**: (Current value - mean) / standard deviation
- **Best for**: Most cases! Especially skewed data (e.g., right-skewed income data).
- **Strength**: Immune to outliers—even a $1M/month salary won’t break it.

*Code: normalization.ipynb (use sklearn's MinMaxScaler & StandardScaler)*

---

**Dimensionality Reduction: Last resort for slow models**
**PCA (Principal Component Analysis)**:
- Finds directions of maximum variance ("capturing the main contradiction").
- **Best for**: High features, low samples (e.g., genomics).
- **Weakness**: Handles linear relationships only.

**SVD (Singular Value Decomposition)**:
- Matrix factorization to extract core components.
- **Best for**: Balanced features and samples (e.g., user-product rating matrices).
- *Mystical take: PCA is a special case of SVD.*

*Code: dimension_reduction.ipynb (see sklearn.decomposition)*

---

**Dataset Splitting = Mandatory!**
- **Training set (70%)**: Model’s textbook.
- **Test set (30%)**: Final exam—use only once.
- **Overfitting**: Model memorizes typos in the textbook, fails the exam.

*Code: dataset_split.ipynb (train_test_split function = the GOAT)*

---

**Todo**
1. Prioritize standardization unless data is perfectly uniform.
2. Dimensionality reduction isn’t essential but saves lives with 1M+ features.
3. Skipping test set ≈ Open-book exam cheating—models fake competence.
4. Read sklearn docs—don’t reinvent wheels.

---
*Translation preserves original tone and technical intent.*