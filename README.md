# Recommender Systems Project Work
*(Part I, Part II, Part III, Part IV)*

**Students:** Oskari Perikangas, Xiaosi Huang  
**Course:** DATA.ML.360-2025-2026-1 Recommender Systems  

**Project Timeline:**  
- **Part I:** November 04, 2025  
- **Part II:** November 10, 2025  
- **Part III:** November xx, 2025  
- **Part IV:** November xx, 2025  

## How to Run the Code

### Step 1: Download Dataset

⚠️ The MovieLens 100K dataset is already included in `data/ml-latest-small/`

If you need to download it manually:
- Go to: https://grouplens.org/datasets/movielens/
- Download: `ml-latest-small.zip`
- Extract to: `data/ml-latest-small/`

### Step 2: Open Jupyter Notebook
```bash
# For Part I:
cd projects/notebooks && jupyter notebook Part_1_user_based_collaborative.ipynb

# For Part II: 
cd projects/notebooks && jupyter notebook Part_2_sequential_methods.ipynb
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

## Part I: User-Based Collaborative Filtering for Group Recommendations
Part I project implements user-based collaborative filtering and group recommendation strategies using the MovieLens 100K dataset.

## Implementation Details

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

## References

- **Masthoff, J. (2004).** Group Recommender Systems: Combining Individual Models
- **MovieLens Dataset:** https://grouplens.org/datasets/movielens/
- **Pearson Correlation:** Standard similarity measure in collaborative filtering

---

## Part II: Sequential Group Recommendations with SQUIRREL Framework

Part II implements and compares three sequential group recommendation methods from the **SQUIRREL (Sequential Group Recommendation with Reinforcement Learning)** framework, extending Part I's collaborative filtering to multi-round adaptive recommendations.


## Implementation Details

### Three Methods Compared

1. **Average Method**
   - Treats all users equally in every round
   - May consistently disadvantage minority users

2. **Least Misery Method**
   - Focuses on the least satisfied user
   - One user can act as veto, may lower overall group satisfaction

3. **SDAA (Satisfaction and Disagreement Aware Aggregation)** 
   - Dynamically adjusts user weights based on cumulative satisfaction
   - Blends Average and Least Misery using alpha parameter
   - Added fairness weighting

### SDAA Key Features

- **State Tracking:** Satisfaction history for each user across rounds
- **Fairness Weighting:** Lower satisfaction → Higher weight
- **Dynamic Aggregation:** `score = (1-α) * weighted_avg + α * least`
- **Reward Function:** SQUIRREL R_sd metric

### Testing & Results

- **Test Group:** Users [1, 414, 599]
- **Popular Movies:** 882 movies (min_ratings=30)
- **Rounds:** 10 sequential recommendation rounds
- **Top-K:** 5 movies per round

| Method          | Group Satisfaction | Group Disagreement | Avg Reward (R_sd) |
|-----------------|-------------------|-------------------|-------------------|
| Average         | 0.791             | 0.350             | 0.723             |
| Least Misery    | 0.789             | 0.350             | 0.723             |
| **SDAA**        | **0.791**         | **0.344**         | **0.736**         |

**Summary:**
- SDAA maintains same satisfaction as Average and reduces disagreement by 1.7%
- SDAA achieves 1.8% higher reward and maintains highest reward across all rounds.

## References

- **Stratigi, M., et al. (2020).** SQUIRREL: Sequential Group Recommendations with Reinforcement Learning
- **MovieLens Dataset:** https://grouplens.org/datasets/movielens/

---
## Part III:

updating...

---
## Part IV:

updating...
