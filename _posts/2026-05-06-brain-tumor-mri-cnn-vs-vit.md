---
title: "Multi-class brain tumor classifier
date: 2026-05-06
categories:
  - projects
tags:
  - medical-imaging
  - interpretability
  - vision-transformer
  - grad-cam
header:
  teaser: /assets/images/brain-tumor-mri-cnn-vs-vit-teaser.png
excerpt: "A 30-layer CNN I built from scratch vs. ViT-B/16 on brain MRI for 4 types of brain tumors."
---

## Overview

A multi-class brain tumor classifier deployed on Arxelos, comparing a 30-layer CNN I built from scratch against ViT-B/16 on the same MRI scans. Every prediction ships with interpretability maps.

## The most interesting failure

A pituitary tumor scan. The CNN classified it as "no tumor" at **97% confidence**. The ViT correctly identified it as pituitary at **50.3% confidence**.

That confidence asymmetry is the actual finding, not the misclassification. The CNN wasn't uncertain and wrong — it was *confidently* wrong, which is the worst kind of failure mode for a clinical model. The ViT wasn't confidently right — it was hedging but correct, which is honest calibration in the face of an ambiguous case.


## The agreement case — same tools, different story

To make the interpretability comparison honest, I ran the same tooling on a "no tumor" scan both models correctly classified.

CNN Grad-CAM was broad and warm, centered on ventricles, thalami, and basal ganglia — reasonable attention to normal anatomical landmarks. When the CNN is right, it's at least looking in the right neighborhood.

ViT Attention Rollout was tightly focused on bilateral hotspots corresponding to choroid plexus / thalamic structures — the ViT was using anatomical symmetry as evidence of normalcy. A more sophisticated reasoning strategy than the CNN's "everything looks normal-shaped."


## Stack

- **CNN:** 30-layer architecture built from scratch in TensorFlow/Keras, 96.2% test accuracy on the multi-class task
- **ViT:** ViT-B/16, fine-tuned from `google/vit-base-patch16-224`
- **Interpretability:** Grad-CAM (`pytorch-grad-cam`), Attention Rollout (custom implementation), SmoothGrad
- **Serving:** FastAPI backend, Docker container, deployed on Azure Container Apps
- **Frontend:** the live neuro-AI page on Arxelos, with per-prediction visualization overlays and a focus-region extractor for both models

## What I'd change next

**Consensus confidence.** The 97% wrong CNN vs. 50% correct ViT pattern is the argument for a joint metric that penalizes disagreement rather than displaying either raw softmax. Multiplying the two distributions and renormalizing is the naive version; a properly calibrated ensemble is the real move. Either way, showing 97% confidence on the CNN's misclassification without context is a UI failure I want to fix.

**Sharper Attention Rollout.** My current implementation averages heads uniformly. The Chefer et al. (2021) method that combines gradients with attention would produce cleaner localization, particularly for smaller lesions where uniform-head averaging blurs the signal.

**Annotation audit.** In the pituitary failure case, the ViT's attention on the parietal region (not the sellar area where pituitary tumors actually sit) suggests the ground-truth label may itself be wrong on that scan. Before extending the training set, I'd want to review the annotations on the ambiguous cases with someone who reads MRIs for a living.

## Links

- Live: [arxelos.com/neuro-ai](https://arxelos.com/neuro-ai)
- Github: [github.com/aryanp2107](https://github.com/aryanp2107/Arxelos/blob/main/Brain_tumor_detection.ipynb)