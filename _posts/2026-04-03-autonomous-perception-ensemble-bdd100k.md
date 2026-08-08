---
title: "Autonomous Perception Ensemble: RT-DETR + U-Net + MiDaS on BDD100K"
date: YYYY-MM-DD
categories:
  - projects
tags:
  - autonomous-driving
  - object-detection
  - segmentation
  - depth-estimation
  - onnx
header:
  teaser: /assets/images/autonomous-perception-ensemble-bdd100k-teaser.png
excerpt: "A miniature self-driving perception stack on 70K BDD100K dashcam images. RT-DETR for detection, U-Net (from scratch) for drivable area, MiDaS for depth. All three exported to ONNX and deployed live on Arxelos."
---

## Overview

A self-driving perception stack in miniature. Three complementary models running on the same BDD100K dashcam frame — RT-DETR tells you *what* is in the scene, a from-scratch U-Net tells you *where* you can drive, MiDaS tells you *how far* everything is. Each handles a different piece of the scene-understanding problem; together they form a full spatial picture from a single frame.

Deployed live at [arxelos.com/perception](https://arxelos.com/perception).

## The stack

- **Detection:** RT-DETR, real-time transformer-based detector, trained on BDD100K's 10 object classes (cars, trucks, pedestrians, cyclists, traffic signs, etc.)
- **Segmentation:** U-Net implemented from scratch in PyTorch for drivable area segmentation
- **Depth:** MiDaS for monocular depth estimation
- **Deployment:** All three exported to ONNX for cross-platform inference; served through the Arxelos FastAPI backend

## The bug that taught me the most: the missing 69,000 images

When I first started training RT-DETR on BDD100K, my data loader reported only 1,156 training images. The dataset should have had ~70,000. That's a factor of 60 off.

First instinct was a wrong path or incomplete download, but `du -sh` on the directory checked out — the bytes were there. Ran two globs side by side:

```python
glob.glob(f"{IMG_TRAIN}/*.jpg")           # → 1,156
glob.glob(f"{IMG_TRAIN}/**/*.jpg", recursive=True)  # → ~70,000
```

The images existed, just not where my code was looking. BDD100K's zip stores training images across four subdirectories — `trainA/`, `trainB/`, `testA/`, `testB/` — inside `images/100k/train/`. A non-recursive glob only caught what was sitting in the top-level directory.

**But the recursive glob alone wasn't enough.** Ultralytics (the RT-DETR training framework) uses a path-swapping convention where it replaces `images/` with `labels/` to find annotation files. With images nested in subdirectories but labels stored flat, the path swap would silently break — training would run but on wrong image/label pairs.

Fix required two changes: switch to `recursive=True` globs everywhere, *and* flatten all subdirectory images into the top-level `train/` and `val/` directories with `shutil.move()`. That restored the one-to-one `images/` ↔ `labels/` mapping Ultralytics expects.

**Takeaway.** The code fix was ~10 lines. The lesson was bigger: never trust assumptions about dataset layout, and never trust that a training framework's file-path conventions are documented where you'd expect. I now run a recursive file count as the *first* step after downloading any dataset, before writing a line of training code. Cheap insurance against silent training failures — the kind where loss goes down and metrics look reasonable, but the model is learning on the wrong data.

## What I'd change next

- **Temporal consistency.** Right now each frame is processed independently. Adding a lightweight tracker (ByteTrack, SORT) across detections would give stable IDs and enable higher-level reasoning like "this pedestrian just entered the crosswalk"
- **Depth calibration.** MiDaS outputs relative depth, not metric. Calibrating to real distances (using known scene geometry, camera intrinsics, or a small labeled subset) would make the output actually usable for downstream decisions
- **Late fusion.** The three models currently run in parallel and their outputs are visualized side-by-side. A fusion step — "which detections lie on drivable area and at what depth?"  would produce a genuinely joint scene representation rather than three overlaid maps

## Links

- Live demo: [arxelos.com/perception](https://arxelos.com/perception)
- Github: [github.com/aryanp2107/Autonomous-Perception-Ensemble](https://github.com/aryanp2107/Autonomous-Perception-Ensemble)