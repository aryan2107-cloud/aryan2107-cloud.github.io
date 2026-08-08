---
title: "Lung Disease Detection: Comparing Sequential, Functional and VGG-16 CNNs"
date: 2025-01-19
categories:
  - projects
tags:
  - deep-learning
  - medical-imaging
  - cnn
  - transfer-learning
header:
  teaser: /assets/images/lung-disease-detection-comparing-cnn-architectures-teaser.png
excerpt: "Three CNN architectures benchmarked across pneumonia, tuberculosis, and lung cancer detection from chest X-rays and CT scans. Sequential won the X-ray tasks (F1 98%+); functional won CT cancer classification (99.9% accuracy)."
---

## Overview

Undergraduate research paper co-authored with Rohit Chauhan and Suryajeet Gupta at Quantum University. We benchmarked three CNN variants — a custom sequential model, a custom functional (multi-branch) model, and a VGG-16 transfer-learning setup across chest X-ray and CT-scan classification for three conditions.

## Results

- **Pneumonia (sequential, chest X-ray):** F1 98.55%, accuracy 98.43%, recall 96.33%
- **Tuberculosis (sequential, chest X-ray):** F1 97.99%, accuracy 99.4%, recall 98.88%
- **Lung cancer (functional, CT scans):** accuracy 99.9%, specificity 99.89%

Sequential model outperformed on X-ray tasks; the functional model (7×7 and 1×1 branches concatenated, feeding five 3×3 conv layers) generalized better on CT cancer classification.

## Stack

- **Framework:** TensorFlow / Keras
- **Datasets:** Covid-19 radiography database (Kaggle), Mendeley chest X-ray posteroanterior collection, lung CT-scan dataset
- **Preprocessing:** 224×224 resize; horizontal flip, zoom, shear, rotation, rescale augmentation
- **Sequential:** 5 conv layers, LeakyReLU (α=0.66), max pooling, Adam @ lr=1e-4, 50 epochs
- **Functional:** 7×7 and 1×1 branches over 3×3, concatenated, then five 3×3 conv layers
- **Pretrained:** VGG-16 with ImageNet weights

## What I'd change next

The reported accuracies are strong but the test sets are small (85 images for TB, 278 for cancer) and the training data leaves external validity unproven. A properly held-out external cohort, cross-validation, and interpretability checks would be table stakes today — themes that motivated my later work.

## Links
- Preprint (unpublished): [PDF](/assets/ResearchPaper(LungDamage).pdf)
- Notebook: [github.com/aryanp2107/DL-Projects](https://github.com/aryanp2107/DL-Projects/blob/main/Lung_Disease_%7C_TensorCTScan.ipynb)