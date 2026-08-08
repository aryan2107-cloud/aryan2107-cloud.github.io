---
title: "Arxelos: Virtual Lesion Analyzer — Why ViT Beats ResNet on a Pituitary Scan"
date: 2026-03-15
categories:
  - projects
tags:
  - interpretability
  - medical-imaging
  - vision-transformer
  - explainable-ai
header:
  teaser: /assets/images/arxelos-vla-teaser.png
  overlay_image: /assets/images/arxelos-vla-hero.png
  overlay_filter: 0.5
excerpt: "A side-by-side comparison of ResNet-50 and ViT-B/16 on brain MRI classification using Grad-CAM, Attention Rollout, and SmoothGrad. ResNet misclassifies a pituitary scan at 97% confidence; ViT gets it right — and its attention map explains why."
---

## Overview

The Virtual Lesion Analyzer is a module within [Arxelos](https://arxelos.com) that runs the same brain MRI through both **ResNet-50** and **ViT-B/16** and visualizes what each model attends to. The goal isn't to declare a winner — it's to make the failure modes of each architecture legible.

## The finding

On one pituitary tumor scan, ResNet-50 predicted **glioma at 97% confidence**. ViT-B/16 correctly classified it as pituitary. The interpretability maps explain the disagreement:

- **Grad-CAM on ResNet-50** showed diffuse activation spread across cortical regions unrelated to the sellar area — the model was confident but not for the right reasons.
- **Attention Rollout on ViT-B/16** cleanly localized to the sellar region where the tumor actually sits.
- **SmoothGrad** on both models confirmed the pattern: ResNet's saliency was noisy and non-anatomical, ViT's was concentrated.

This is a canonical case of a CNN's local inductive bias failing on a mid-scale anatomical structure that a self-attention model handles more gracefully.

## Stack

- **Models:** ResNet-50 (torchvision, fine-tuned), ViT-B/16 (`google/vit-base-patch16-224` fine-tuned)
- **Interpretability:** Grad-CAM (`pytorch-grad-cam`), Attention Rollout (custom implementation), SmoothGrad
- **Serving:** FastAPI backend, Docker container on Azure Container Apps
- **Frontend:** Live at [arxelos.com](https://arxelos.com)

## What I'd change next

The attention rollout implementation averages heads uniformly — the [Chefer et al. 2021](https://arxiv.org/abs/2103.15679) method that combines gradients with attention would likely produce sharper localization, particularly for smaller lesions. I'm also curious whether a hybrid ConvNeXt architecture would collapse the difference or preserve it.

## Links

- Live demo: [arxelos.com](https://arxelos.com)
- Code: [github.com/aryanp2107](https://github.com/aryanp2107)
