---
title: "Demographic-Aware Face Generation: Per-Ethnicity FID for a Fairness cGAN"
date: 2026-02-11
categories:
  - projects
tags:
  - generative-models
  - cgan
  - fairness
  - computer-vision
header:
  teaser: /assets/images/demographic-aware-face-generation-cgan-teaser.png
excerpt: "Conditional GAN for face generation across 7 ethnic categories. Per-ethnicity FID variance <3% and human perceptual variance <8% — no significant ethnic bias at the model's capability level."
---

## Overview

Sports video games (EA FC, NBA 2K, Madden) auto-generate faces for scouted international players — badly. Same faces recur, ethnic demographics don't match countries, and detail is allocated unevenly across groups. Four-person course project exploring whether a conditional GAN could produce a fairer baseline for auto-generated player faces.

## The finding

**No significant ethnic bias detected at the model's capability level.** Trained on FairFace (62,575 images filtered to ages 20–49, resized to 128×128), the cGAN produced faces of comparable quality across all 7 ethnic categories at the optimal checkpoint (epoch 33 of 50):

- **Per-ethnicity FID:** 139.3–155.2, ~3% variance across groups
- **Human perceptual rating:** 4.2–6.8 / 10, ~7.7% variance

The important word is *comparable*. Absolute quality was limited — an FID around 150 means generated faces are far from photorealistic, and the perceptual survey confirmed the ceiling. But the *distribution* of quality across ethnicities was flat, which was the fairness question being tested.

## My role

Owned hyperparameter tuning, model training, and optimal checkpoint selection across two full training runs on a Colab A100. Trade-offs I was navigating:

- **Generator vs. discriminator learning rates:** settled on 2e-4 / 1e-4 to keep D from dominating and collapsing the generator
- **Batch size** at 128 balanced training stability against A100 memory
- **Checkpoint selection** turned out to matter more than expected — model quality peaked at epoch 33 of 50, with later checkpoints showing signs of mode collapse. Tracking per-ethnicity FID across epochs was necessary rather than just using final weights

Teammates handled evaluation methodology (Caleb Lee), data pipeline (Gaurav Bhatnagar) and UI (Yohanan Ben-Gad).

## Stack

- **Model:** cGAN with projection discriminator (Miyato & Koyama 2018), spectral normalization (Miyato et al. 2018)
- **Data:** FairFace (Karkkainen & Joo 2021), 62,575 images, 7 ethnic categories, 128×128
- **Training:** PyTorch, Colab A100, batch size 128, 50 epochs
- **Evaluation:** clean-fid for per-group FID, human perceptual survey (1–10 scale)
- **Referenced work:** TTUR (Heusel et al. 2017)

## What I'd change next

The model at ~100K parameters is undersized for the problem — that's the ceiling on realism, and it's why FID sits around 150. A larger backbone (StyleGAN2 at even a modest scale) would likely bring FID into the 30–50 range. The interesting piece — per-ethnicity variance under 3% is the methodology that carries forward, and it should hold or improve at higher model capacity.

## Links
- Github: [github.com/calebl37/demographic-aware-face-generation](https://github.com/calebl37/demographic-aware-face-generation)
