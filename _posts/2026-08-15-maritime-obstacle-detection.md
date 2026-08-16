---
title: "Class-Agnostic Maritime Obstacle Detection with Simulated IMU-Guided Horizon Stabilization for Autonomous Surface Vessels"
date: 2026-08-15
categories:
  - projects
tags:
  - object-detection
  - maritime
  - yolo
  - kalman-filter
  - computer-vision
header:
  teaser: /assets/images/maritime-obstacle-detection-teaser.jpg
excerpt: "Class-agnostic obstacle detection for autonomous surface vessels on the unified USVTrack + LaRS benchmark, with synthetic IMU horizon stabilization and Kalman-filtered tracking for identity stability."
youtube: "https://www.youtube.com/embed/70D5531bZ5Y?autoplay=1&mute=1&controls=0&loop=1&playlist=70D5531bZ5Y"
---

## Overview

Course project for CS5330 (Pattern Recognition and Computer Vision, Prof. Bruce Maxwell) with Ananda Sangli and Julee Chung. We built a class-agnostic obstacle detection pipeline for autonomous surface vessels — one that treats every floating obstacle as "hit-or-not-hit" rather than trying to classify it, because on the water what you need to avoid isn't neatly categorizable.

<iframe width="560" height="315"
        src="https://www.youtube.com/embed/70D5531bZ5Y?autoplay=1&mute=1&controls=0&loop=1&playlist=70D5531bZ5Y"
        title="YouTube video player"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
        referrerpolicy="strict-origin-when-cross-origin"
        allowfullscreen></iframe>

## Two questions we set out to answer

**RQ1: Does synthetic IMU-based horizon stabilization improve detection on rocking footage?**

Boats pitch and roll. Detectors trained on stable dashcam data assume a level horizon, and every degree of roll shifts objects relative to the frame. We synthesized IMU readings from video motion and applied rotational homography to stabilize each frame before detection — asking whether removing the roll before inference beats letting the detector figure it out.

**RQ2: Does Kalman-filter tracking reduce bounding-box jitter and identity flicker across frames?**

Frame-by-frame detection produces boxes that shimmer and swap IDs across consecutive frames — bad for downstream avoidance logic. We ran detections through a Kalman filter that models each obstacle's position and velocity, smoothing box coordinates and maintaining stable identity across occlusions.

## Approach

**Dataset.** Unified benchmark from USVTrack (tracking annotations) and LaRS (semantic segmentation for the maritime domain). Combining them gave us both the class-agnostic labels we wanted and the domain variance (open water, harbors, obstacles at different scales) needed for a generalizable detector.

**Detector.** YOLOv8n — nano variant chosen for real-time inference budgets on the kind of hardware an actual surface vessel would carry. Not the largest model available, deliberately.

**RQ1 pipeline.**
1. Estimate roll angle per frame from video motion (synthetic IMU)
2. Apply rotational homography to level the horizon
3. Run YOLOv8n on the stabilized frame
4. Compare mAP / recall against the unstabilized baseline

**RQ2 pipeline.**
1. Take raw YOLOv8n detections per frame
2. Feed into a Kalman filter tracking position + velocity per obstacle
3. Measure bounding box jitter and ID switch rate against the unfiltered baseline

## Results

Full result videos across all test scenarios — including RQ1 horizon stabilization comparisons and RQ2 Kalman-filtered tracking sequences — are available in the [project results folder on Google Drive](https://drive.google.com/drive/folders/1hVo72Dl8oZ0iFva8McKMr67D0-OpRV-n?usp=sharing).

## Stack

- **Detector:** YOLOv8n (Ultralytics)
- **Benchmark:** USVTrack + LaRS (unified)
- **Stabilization:** synthetic IMU + rotational homography (OpenCV)
- **Tracking:** custom Kalman filter (position + velocity state)
- **Language:** Python, PyTorch
- **Evaluation:** mAP, recall, ID switch rate, bounding-box jitter metrics


## Resources

- **Report (IEEE format):** [PDF](assets/CS5330_Final_Report.pdf)
- **Code:** [github.com/aryanp2107/maritime_obstacle_detection](https://github.com/aryanp2107/maritime_obstacle_detection)
- **Team:** Aryan Patel, Ananda Sangli, Julee Chung
- **Course:** CS5330 (Pattern Recognition and Computer Vision), Prof. Bruce Maxwell, Northeastern University