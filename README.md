# Predicting User Re-Engagement on Live Streaming Platforms

**Master's Thesis** — Data Science and Society, Tilburg University (2026)  
**Author:** Emre Hayati Bekiroğlu  
**Supervisor:** Dr. Çiçek Güven

---

## Overview

This repository contains the full analysis pipeline for my master's thesis, 
which investigates whether user re-engagement on live streaming platforms 
(Twitch) can be predicted using traditional machine learning models versus 
graph-based approaches under a leakage-safe temporal evaluation setting.

---

**Dataset:** Rappaz, J., McAuley, J., & Aberer, K. (2021). Recommendation 
on Live-Streaming Platforms: Dynamic Availability and Repeat Consumption. 
RecSys 2021.

The dataset is publicly available at:  
https://cseweb.ucsd.edu/~jmcauley/datasets.html#twitch

---

## Research Questions

**Main:** How effectively can user re-engagement on live streaming platforms 
be predicted using traditional machine learning models compared to graph-based 
approaches under a leakage-safe temporal evaluation setting?

**Sub-questions:**
- SQ1: How does the predictive performance of traditional machine learning models
  compare to graph-based approaches when predicting whether a user will re-engage
  with a streamer?
- SQ2: To what extent do leakage-safe structural interaction features contribute
  to predicting user re-engagement on live streaming platforms?
- SQ3: To what extent do LightGCN-derived user and streamer representations provide
  predictive value beyond engineered behavioural features in a temporal re-engagement
  setting?

---

## Repository Structure

| Folder | Contents |
|--------|----------|
| `notebooks/` | Analysis pipeline (Phases 1–7) |
| `reports/eda/` | EDA figures and tables |
| `reports/modeling/` | Modeling figures and tables |
| `environment/` | Conda environment and requirements |
| `docs/` | Design documentation |

> `data_raw/` and `data_processed/` are excluded from this repository.

---

## Methodology

- **Task:** Binary re-engagement classification (3-day horizon)
- **Temporal split:** Train 20d / Gap 3d / Validation 7d / Gap 3d / Test
- **Models:** Logistic Regression, XGBoost, LightGCN (hybrid)
- **Primary metric:** ROC-AUC

---

## Notebooks

| # | Phase | Description |
|---|-------|-------------|
| 01 | Data Cleaning | Raw data integrity checks |
| 02 | EDA | Exploratory analysis |
| 03 | Temporal Split | Leakage-safe train/val/test construction |
| 04 | Feature Engineering | User, streamer, pair, temporal, structural features |
| 05 | Traditional Models | Logistic Regression & XGBoost baselines |
| 06 | Graph-Based Models | LightGCN + hybrid model |
| 07 | Comparative Analysis | Model comparison & feature contribution |
