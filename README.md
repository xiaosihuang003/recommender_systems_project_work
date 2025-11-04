# Project 1: User-Based Collaborative Filtering for Group Recommendations

## Project Overview
This project implements user-based collaborative filtering and group recommendation strategies using the MovieLens 100K dataset.

**Students:** Oskari Perikangas, Xiaosi Huang  
**Course:** DATA.ML.360-2025-2026  
**Date:** November 4, 2025

---

## How to Run the Code

### Step 1: Download Dataset

The MovieLens 100K dataset is already included in `data/ml-latest-small/`

If you need to download it manually:
- Go to: https://grouplens.org/datasets/movielens/
- Download: `ml-latest-small.zip`
- Extract to: `data/ml-latest-small/`

### Step 2: Open Jupyter Notebook
```bash
cd Project1
jupyter notebook notebooks/PartI_(final).ipynb
```

### Step 3: Run All Cells

**Option A: Run all at once**
- Menu: `Cell` → `Run All`
- Or: Keyboard shortcut `Ctrl+Shift+Enter` (Windows/Linux) or `Cmd+Shift+Enter` (Mac)

**Option B: Run cell by cell**
- Click on a cell
- Press `Shift+Enter` to run and move to next cell
- Or click the ▶️ Run button

---

## What Each Section Does

### Part (a): Data Understanding
- Loads MovieLens 100K dataset
- Displays statistics and distributions
- Checks data quality

**Input:** `data/ml-latest-small/ratings.csv`  
**Output:** Data statistics, visualizations

### Part (b): Pearson Similarity
- Calculates Pearson correlation between users
- Finds top 10 similar users to User 1
- Basis for collaborative filtering

**Output:** Top 10 similar users with similarity scores

### Part (c): Rating Prediction
- Implements collaborative filtering prediction formula
- Predicts ratings for movies User 1 hasn't watched
- Uses top-k similar neighbors

**Output:** Predicted ratings for unseen movies

### Part (d): Enhanced Pearson Similarity
- Improves Pearson by considering data quality
- Adds bonus for users with more overlapping ratings
- Shows improvement over standard Pearson

**Output:** Comparison of standard vs enhanced similarity

### Part (e): Group Recommendations
- Generates recommendations for 3-user group [1, 414, 599]
- Implements TWO aggregation methods:
  - **Average Method:** All users have equal voting power
  - **Least Misery Method:** One person can veto

**Output:** Top 10 movies by each method, comparison

### Part (f): Balancing Strategy
- Implements disagreement-aware group recommendations
- Minimizes user disagreement using Balancing strategy
- Compares all three methods

**Output:** Top 10 movies with disagreement levels

---

## Key Results Summary

### Group Test: User [1, 414, 599]

| Metric | Value |
|--------|-------|
| Total Recommendations | 50+50+21 movies |
| Common Movies | 14 |
| Top Movie (All Methods) | Movie 1230 |

### Method Comparison

| Method | Movie 1230 | Movie 1276 | Philosophy |
|--------|-----------|-----------|
| Average | 4.75 | 4.38 |
| Least Misery | 4.50 | 3.76 | 
| Balancing | 4.70 | 4.26 | 

---

## References

- **Masthoff, J. (2004).** Group Recommender Systems: Combining Individual Models
- **MovieLens Dataset:** https://grouplens.org/datasets/movielens/
- **Pearson Correlation:** Standard similarity measure in collaborative filtering


