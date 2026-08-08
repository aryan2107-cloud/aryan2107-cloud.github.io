---
title: "100 Days of ML: A Structured Self-Study Curriculum"
date: 2025-11-13
categories:
  - projects
tags:
  - self-study
  - machine-learning
  - deep-learning
  - curriculum
header:
  teaser: /assets/images/100-days-of-ml-structured-self-study-teaser.png
excerpt: "A 100-day self-designed ML curriculum in three phases — data foundations, classical ML, deep learning — with capstones between phases. Currently at 67/100"
---

## Overview

A structured self-study challenge that walks ML end-to-end, from NumPy fundamentals to transformers and GANs. Three phases, capstone projects between them, so each phase forces a synthesis before moving on.

## Progress

**46/100 days complete.** Phase 1 and Phase 2 done, Phase 3 in progress.

- **Phase 1 — Data Foundations (Days 1–15):** NumPy/pandas, missing data, outliers, feature engineering, PCA, EDA, time series, geospatial. Capstone: rain prediction on Australian weather data.
- **Phase 2 — Classical ML (Days 16–40):** regression, trees, random forests, XGBoost, SVMs, KNN, clustering (K-Means, DBSCAN, hierarchical, GMM), ensembles, SHAP interpretability. Capstone: loan default prediction on Lending Club data.
- **Phase 3 — Deep Learning (Days 41–60):** neural networks, CNNs, transfer learning, augmentation, object detection, RNNs, transformers, GloVe, autoencoders, VAEs, GANs, U-Nets, autonomous perception, RL basics.

## Why this format

The capstones between phases are the point. It's easy to work through tutorials and feel like you understand something; it's harder to build an end-to-end pipeline that has to run. Requiring a synthesis project before moving on makes gaps visible — feature engineering isn't a concept until I've built a pipeline for it, SHAP isn't understood until I've explained it on my own model.

## Featured capstones

**Rain prediction (Day 15).** Binary classification on Australian weather. Missing data handling → encoding → feature scaling → model selection.

**Loan default prediction (Days 38–40).** End-to-end pipeline on Lending Club data: EDA → feature engineering → hyperparameter tuning → SHAP interpretability → production pipeline. [Standalone repo](https://github.com/aryanp2107/loan-default-prediction).

## Stack

- **Languages:** Python
- **ML:** scikit-learn, XGBoost, LightGBM
- **Deep learning:** PyTorch, TensorFlow/Keras
- **Data:** pandas, NumPy
- **Viz:** matplotlib, seaborn
- **Interpretability:** SHAP

## Links

- Full repo: [github.com/aryanp2107/100-ML-Projects](https://github.com/aryanp2107/100-ML-Projects)
- Loan default capstone: [github.com/aryanp2107/loan-default-prediction](https://github.com/aryanp2107/loan-default-prediction)