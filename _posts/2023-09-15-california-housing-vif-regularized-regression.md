---
title: "California Housing: VIF-Guided Regularized Regression"
date: 2023-09-15
categories:
  - projects
tags:
  - regression
  - regularization
  - eda
  - scikit-learn
header:
  teaser: /assets/images/california-housing-vif-regularized-regression-teaser.png
excerpt: "Ridge beat lasso and OLS on California housing prices by 12% RMSE, after VIF analysis flagged multicollinearity across 10+ economic and geographic features."
---

## Overview

Early ML project on the California housing dataset — a classic setup where economic and geographic features (median income, population density, latitude/longitude, proximity to the ocean) are heavily correlated. OLS looks fine on training data but generalizes poorly.

## The finding

VIF analysis flagged clear multicollinearity — several features exceeded VIF > 10, meaning they were largely redundant with linear combinations of others. Regularization was the right response.

Benchmarking ridge, lasso, and OLS with cross-validated RMSE:

- **Ridge won**, delivering 12% lower RMSE vs. the OLS baseline
- Ridge shrunk correlated coefficients toward each other rather than zeroing them out (lasso's tendency), which suited the multicollinearity pattern
- 4-member team; I led feature analysis and regularization strategy

## Stack

- **Data:** California housing (10+ demographic and economic features)
- **Preprocessing:** missing-value imputation, categorical encoding, feature scaling
- **Diagnostics:** VIF (variance inflation factor) for multicollinearity
- **Models:** OLS, Ridge, Lasso via scikit-learn, tuned by cross-validation
- **Evaluation:** cross-validated RMSE

## What I'd change next

A nonlinear baseline (random forest or gradient boosting) would separate two questions the linear-only benchmark can't distinguish: how much of the "regularization win" was really about handling multicollinearity vs. simply modeling nonlinearity better. Fixed effects on geographic clusters instead of raw lat/long would also help.

## Links

- Github: [github.com/aryanp2107/PRODIGY_Tasks](https://github.com/aryanp2107/PRODIGY_Tasks)

*Completed during a summer ML internship at Prodigy Infotech, July–September 2023.*