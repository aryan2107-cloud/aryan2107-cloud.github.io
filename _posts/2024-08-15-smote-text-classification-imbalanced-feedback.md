---
title: "SMOTE for Imbalanced Text Classification: Recovering Minority-Class Recall"
date: 2024-08-15
categories:
  - projects
tags:
  - nlp
  - text-classification
  - class-imbalance
  - smote
header:
  teaser: /assets/images/smote-text-classification-imbalanced-feedback-teaser.png
excerpt: "TF-IDF + SMOTE on 10,000+ customer feedback samples across 8 categories. Systematic ablation of balancing strategies lifted minority-class recall by 18% over the baseline."
---

## Overview

End-to-end text classification pipeline on 10,000+ customer feedback samples spanning 8 categories, with heavy class imbalance across categories. The interesting problem wasn't the classifier — it was picking the right response to imbalance.

## The finding

Baseline models trained on the raw distribution collapsed on minority categories — high macro accuracy hid near-zero recall on underrepresented labels. I ran a systematic ablation across balancing strategies (class weights, random undersampling, SMOTE) on TF-IDF features:

**SMOTE-based resampling lifted minority-class recall by 18% over the class-imbalanced baseline**, without meaningfully degrading majority-class precision.

Reported per-category precision-recall so the trade-offs were visible, not averaged out. Also documented failure modes for a relabeling pass — some "errors" were annotation ambiguities, not model failures.

## Stack

- **Data:** 10,000+ customer feedback samples, 8 categories
- **Features:** TF-IDF vectorization
- **Balancing:** SMOTE (imbalanced-learn), compared against class weights and random undersampling
- **Evaluation:** per-category precision, recall, F1; not just macro accuracy
- **Documentation:** pipeline architecture, ablation results, failure analysis for relabeling

## What I'd change next

Two things I'd approach differently now:

1. **SMOTE on TF-IDF is a compromise.** Synthetic samples in bag-of-words space don't correspond to plausible sentences — the geometry that makes SMOTE work on tabular features breaks down on high-dimensional sparse text. A transformer encoder (DistilBERT, RoBERTa) with class-weighted loss or focal loss would likely give cleaner gains without the synthetic-sample artifact.
2. **Systematic error analysis first, algorithmic fix second.** The failure analysis for relabeling was useful; doing it *before* deciding on SMOTE would have told me whether the "imbalance problem" was really an annotation problem in disguise.

## Links

- Github: [github.com/aryanp2107/YBI_Project](https://github.com/aryanp2107/YBI_Project)

*Completed during an AI/ML internship at YBI Foundation, November 2023 – August 2024.*