# Recommender Systems Project Work
*(Part I, Part II, Part III, Part IV)*

**Students:** Oskari Perikangas, Xiaosi Huang  
**Course:** DATA.ML.360-2025-2026-1 Recommender Systems  

**Project Timeline:**  
- **Part I:** November 04, 2025  
- **Part II:** November 10, 2025  
- **Part III:** November 19, 2025  
- **Part IV:** November 28, 2025
- **Part II:** December 05, 2025  


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

---

## Part II (Upgraded): Sequential Group Recommendations with PFA

**Improved Solution:** PFA (Proportional Fairness Aggregation) - NEW METHOD

Part II (Upgraded) implements a fundamentally different sequential group recommendation method using **geometric mean aggregation** instead of arithmetic mean, addressing the instructor's feedback with a theoretically grounded approach.

## Implementation Details

### Core Innovation: Geometric Mean Aggregation

**Key Difference from SDAA:**
- **SDAA-Modified:** `score = (w₁·p₁ + w₂·p₂ + w₃·p₃) / Σw` [Arithmetic mean]
  - Averages predictions → may hide individual dissatisfaction
- **PFA:** `score = (p₁^w₁ × p₂^w₂ × p₃^w₃)^(1/Σw)` [Geometric mean]
  - Multiplies predictions → naturally penalizes imbalanced ratings

**Example:**  
For predictions [2.0, 5.0, 4.0]:
- Arithmetic: 3.67 (looks acceptable)
- Geometric: 3.42 (lower score reflects User 1's dissatisfaction)

### PFA Method Features

1. **Two-Phase Design**
   - **Phase 1:** Average aggregation filters top k×3 candidates (quality constraint)
   - **Phase 2:** Weighted geometric mean selects final k items (fairness optimization)

2. **Dynamic Gap-Based Weights**
   - Users below average satisfaction → higher weights (1.2-1.4)
   - Users above average satisfaction → lower weights (0.8-1.0)
   - Self-correcting mechanism prevents cumulative unfairness

3. **Theoretical Foundation**
   - Based on Nash Bargaining Solution
   - Implements proportional fairness from network resource allocation
   - No manual parameter tuning (no alpha parameter)

### Testing & Results

**Test Group:** Users [4, 39, 404] (medium diversity: 0.111)  
**Popular Movies:** 882 movies (min_ratings=30)  
**Rounds:** 10 sequential recommendation rounds  
**Top-K:** 5 movies per round

#### Single Group Performance

| Method          | Group Satisfaction | Group Disagreement | Avg Reward (R_sd) |
|-----------------|-------------------|-------------------|-------------------|
| Average         | 0.799             | 0.117             | 0.840             |
| Least Misery    | 0.786             | 0.103             | 0.838             |
| SDAA-Modified   | 0.800             | 0.114             | 0.834             |
| **PFA (NEW)**   | **0.795**         | **0.111**         | **0.843**         |

**Key Improvements over SDAA-Modified:**
- ✓ Reward: +1.1% (0.843 vs 0.834)
- ✓ Fairness: -2.6% disagreement (0.111 vs 0.114)
- ✓ Best overall performance among all 4 methods

#### Multi-Group Validation

**Setup:** 10 diverse groups with varying diversity levels (0.45-0.68)

| Metric                    | Value                  |
|---------------------------|------------------------|
| Mean improvement over SDAA| +0.6% (+0.0048)        |
| Win rate vs SDAA-Modified | 6 Wins / 0 Ties / 4 Losses (60%) |
| Consistency               | Positive trend across groups |

**Summary:**
- PFA demonstrates robust performance with 60% win rate across diverse groups
- Geometric mean naturally enforces fairness without manual tuning
- Two-phase design successfully balances quality and fairness objectives
- Works best for medium-diversity groups (disagreement ≈ 0.1-0.15)

## Why PFA is Fundamentally Different

1. **Mathematical Approach:** Multiplication (geometric) vs addition (arithmetic)
2. **Fairness Mechanism:** Natural penalty from geometric mean vs manual alpha blending
3. **Weight Function:** Gap-based adaptive weights vs simple normalization
4. **Theoretical Basis:** Nash Bargaining Solution vs heuristic framework

## References

- **Stratigi, M., et al. (2020).** SQUIRREL: Sequential Group Recommendations with Reinforcement Learning
- **Kelly, F. (1997).** Charging and rate control for elastic traffic. European Transactions on Telecommunications (Proportional Fairness)
- **MovieLens Dataset:** https://grouplens.org/datasets/movielens/

---

## Part III: Diverse Sequential Group Recommendations with DiGSFO

Part III adds diversity control to sequential group recommendations, preventing filter bubbles while maintaining fairness.

## Implementation Details

### Method: DiGSFO (ADAPT Framework)

**Source:** Emilia Lenzi's Lecture, pages 30-32

**Key Idea:**
- Round 1: Average aggregation
- Round 2+: Filter by genre distance → Score by weighted fairness → Select best list

**Parameters:**
- δ=0.5 (distance threshold)
- s=100 (top items per user)
- t=50 (candidate lists)
- k=5 (movies per round)

### Testing & Results

**Setup:**
- Test Group: Users [1, 414, 599]
- Rounds: 10 sequential rounds
- Recommendations: 5 movies per round

| Delta | Group Satisfaction | Group Disagreement | Avg Diversity |
|-------|-------------------|-------------------|---------------|
| 0.0   | 0.806             | 0.337             | 0.785         |
| **0.5** | **0.806**       | **0.337**         | **0.801**     |
| 0.8   | 0.796             | 0.344             | 0.960         |

**Summary:**
- DiGSFO improves diversity by 2% (0.785 → 0.801) while maintaining high satisfaction (0.806)
- Weighted fairness keeps disagreement stable at 0.337
- Best balance achieved with δ=0.5

## References

- **Lenzi, E., et al.** ADAPT: Fairness and Diversity in Sequential Group Recommendations
- **MovieLens Dataset:** https://grouplens.org/datasets/movielens/
---

## Part IV: Counterfactual Explanations for Group Recommendations

Part IV generates counterfactual explanations to help groups understand why specific movies were recommended, ensuring fairness by avoiding single-user blame.

## Implementation Details

### Method: Grow & Prune with Pareto Filtering

**Goal:** Generate explanations of the form:  
*"If the group had NOT watched items A, then item B would NOT be recommended"*

**Key Components:**

1. **Item-Level Metrics** (Slides p.12-14)
   - **Recognition:** Proportion of group members who rated the item
   - **Rating:** Average group rating for the item
   - **Influence:** Impact on target recommendation
   - **Explanatory Power:** Causal strength (fast estimation via item-item similarity)

2. **Fairness Constraint** (Slides p.16)
   - Requires ≥67% of group members to contribute to explanation
   - Prevents single-user blame

3. **Pareto Filtering** (Slides p.18-19)
   - Reduces candidates from ~1275 to 3-5 items
   - Multi-objective optimization: recognition, rating, influence, explanatory power

4. **Grow & Prune Algorithm** (Slides p.22)
   - **GROW:** Add items until target disappears from top-N
   - **PRUNE:** Remove redundant items while maintaining validity and fairness

### Testing & Results

**Setup:**
- Test Group: Users [105, 305, 477] (81 common high-rated movies)
- Recommender: Item-based KNN (k=50, RMSE=0.9700)
- Tested: Top-10 recommendations

| Metric              | Result      |
|---------------------|-------------|
| Success Rate        | 60% (6/10)  |
| Explanation Size    | Mean=1.50   |
| Fairness            | Mean=0.94   |
| Explanatory Power   | Mean=0.725  |

**Valid Explanations:**
- Rank 4 (Hoop Dreams): 3 items, Fairness=1.0
- Rank 5 (Return of the Pink Panther): 2 items, Fairness=1.0
- Ranks 7-10: 1 item each, Fairness≥0.67

**Failed Cases:**
- Ranks 1-3, 6: Complex multi-factor reasoning beyond max_size=8

**Example (Rank 10):**
> "If the group had NOT watched **Full Metal Jacket** (U105:4.0★, U305:5.0★, U477:5.0★),  
> then **Like Water for Chocolate** would NOT be recommended."

**Key Findings:**
1. **Pareto Filtering:** (1275 → 3-5) 99.6% candidate reduction 
2. **Fairness Achievement:** 5/6 perfect fairness, all above threshold
3. **Minimality:** 1.5 items average (highly concise)
4. **Efficiency:** Fast approximation reduced runtime from hours to minutes

## References

- **Stefanidis, K., & Stratigi, M. (2025).** Explaining Group Recommendations via Counterfactuals (Lecture slides, pages 12-22). DATA.ML.360 Recommender Systems, Tampere University.
- **MovieLens Dataset:** https://grouplens.org/datasets/movielens/
