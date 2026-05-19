# Temporal Split Design

## Purpose

This document defines the primary leakage-safe temporal split used in the thesis experiments.  
The goal is to ensure a **chronological, reproducible, and leakage-free evaluation** when predicting whether a user will re-engage with the same streamer within the next **3 days**.

The prediction horizon is therefore:

3 days = 432 timesteps (10-minute intervals)

Because labels depend on future activity, special care is required when splitting the dataset to avoid temporal leakage.


## Dataset Time Structure

The Twitch interaction dataset used in this project contains:

- 6,148 timesteps
- 10-minute resolution
- approximately 43 days of activity

Useful conversions:

- 1 day = 144 timesteps  
- 3 days = 432 timesteps  
- 7 days = 1,008 timesteps  
- 20 days = 2,880 timesteps


## Design Principles

The split follows four principles:

1. **Strict chronological ordering**  
   Train → Validation → Test

2. **Prediction anchors are split, not raw rows**  
   Only selected timestamps are used as prediction instances.

3. **3-day safety gaps are inserted between partitions**  
   This prevents label windows from crossing split boundaries.

4. **The final 3 days are excluded from anchors**  
   The future label window cannot be observed there.


## Anchor vs Raw Data

Not all timestamps become prediction anchors.

Some periods remain in the dataset but are excluded from anchor generation:

- safety gaps between partitions
- the final 3 days of the dataset

These periods are **not used as supervised prediction instances**, but the interactions remain in the dataset.


## Primary Temporal Split

The split is defined at the **timestep level** using half-open intervals:

[start, end)

| Segment | Timestep Range | Length | Approx. Days | Used as Prediction Anchors? |
|---|---:|---:|---:|---|
| Train | [0, 2880) | 2880 | 20.0 | Yes |
| Gap 1 | [2880, 3312) | 432 | 3.0 | No |
| Validation | [3312, 4320) | 1008 | 7.0 | Yes |
| Gap 2 | [4320, 4752) | 432 | 3.0 | No |
| Test | [4752, 5716) | 964 | 6.7 | Yes |
| Final Tail | [5716, 6148) | 432 | 3.0 | No |

Consistency check:

2880 + 432 + 1008 + 432 + 964 + 432 = 6148


## Interpretation

- **Train anchors** are used to fit the models.
- **Validation anchors** are used for hyperparameter tuning.
- **Test anchors** are used for the final evaluation.

Safety gaps ensure that the **3-day label window does not cross partition boundaries**.

The final 432 timesteps are excluded because the full future horizon cannot be observed.


## Leakage Prevention Rule

All features and graph representations must follow the **as-of-time-t rule**:

For any prediction anchor at time *t*, only data with timestamp ≤ *t* may be used.

This applies to:

- feature engineering
- graph construction
- normalization
- aggregated statistics


## Reproducibility

The temporal split is implemented once and reused consistently across all experiments.

A split table is generated and stored as:

data_processed/temporal_split.csv

This file assigns each timestep to its corresponding segment (train, gap, validation, gap, test, or tail). All feature pipelines and models must use this file to ensure that the exact same temporal split is applied throughout the project.

Using a shared split artifact guarantees that traditional ML models and graph-based models are evaluated on identical data partitions.


## Summary

The final leakage-safe split is:

- Train: `[0, 2880)`
- Validation: `[3312, 4320)`
- Test: `[4752, 5716)`

with two **3-day safety gaps** and a **3-day final tail** removed from anchor generation.

This design preserves strict temporal ordering and prevents label leakage while keeping a sufficiently large training window for model comparison.