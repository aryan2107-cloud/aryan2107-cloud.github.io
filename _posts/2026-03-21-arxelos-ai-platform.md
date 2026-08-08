---
title: "Arxelos: A Deployed AI/ML Platform"
date: 2026-03-21
categories:
  - projects
tags:
  - platform
  - deployment
  - fastapi
  - onnx
  - azure
header:
  teaser: /assets/images/arxelos-ai-platform-teaser.jpg
excerpt: "A solo-built, production-deployed platform hosting my AI/ML work under one roof — brain tumor classification with CNN vs ViT interpretability, an autonomous perception ensemble on BDD100K, and a PubMed-grounded literature RAG pipeline."
---

## Overview

Arxelos is the platform I built to deploy my AI/ML work under one roof — not scattered notebooks.

 [arxelos.com](https://arxelos.com).


Building one platform forced decisions individual notebooks avoid:

- **Consistent model serving.** Every model behind the same FastAPI backend, containerized, deployed to Azure Container Apps. One request/response contract, one latency budget, one error-handling path.
- **ONNX across frameworks.** All models exported to ONNX so PyTorch, TensorFlow, and pre-trained checkpoints run through the same serving code without framework-specific glue.
- **Interpretability and structured outputs as first-class citizens.** Grad-CAM, Attention Rollout, SmoothGrad, depth colormaps, segmentation overlays, citations. Every prediction ships with the artifacts a user needs to interpret it, not just a scalar output.

## Stack

- **Frontend:** the arxelos.com site itself, with per-module visualization
- **Backend:** FastAPI, containerized with Docker
- **Deployment:** Azure Container Apps
- **Models:** TensorFlow/Keras (custom CNN), PyTorch (ViT, U-Net from scratch), pre-trained checkpoints (RT-DETR, MiDaS)
- **Model format:** ONNX across the board
- **RAG stack:** LangChain, ChromaDB, PubMed as source corpus
- **Interpretability:** `pytorch-grad-cam`, custom Attention Rollout, SmoothGrad

## What I'd change next

- **Observability.** No logging or per-endpoint telemetry yet. Adding request logs, latency histograms, and prediction-distribution tracking would let me detect drift and debug production failures properly.
- **Shared uncertainty layer.** Each module surfaces confidence differently — softmax on the classifier, IoU on segmentation, retrieval scores on RAG. A shared calibrated-uncertainty layer across modules would make outputs comparable.
- **RAG improvements.** Retrieval is fine on well-formed clinical queries, degrades on vague ones. Reranking, query rewriting, and hybrid dense-plus-sparse search are the next moves.

## Links

- platform: [arxelos.com](https://arxelos.com)