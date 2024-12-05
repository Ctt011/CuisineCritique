# Ratings and Cuisine Popularity

by [Camille Tran] (Ctt011@ucsd.edu)

---
## 1. Introduction

Food is a universal language, and analyzing recipes allows us to explore cultural diversity and user preferences. This project leverages data from Food.com to investigate the relationship between cuisines and recipe ratings.

### Research Question:
**Do cuisines significantly affect recipe ratings, and which cuisines are most likely to be rated highly?**

---
## About the Data

The analysis is based on two datasets:
1. **`RAW_recipes.csv`**: Contains detailed information about recipes, including their names, preparation time, ingredients, steps, and tags.
2. **`RAW_interactions.csv`**: Includes user interactions with the recipes, such as ratings and reviews.

### `RAW_recipes.csv` Columns
| Column Name         | Description                                                                                 |
|---------------------|---------------------------------------------------------------------------------------------|
| `name`              | Recipe name                                                                                |
| `id`                | Recipe ID                                                                                  |
| `minutes`           | Minutes to prepare the recipe                                                              |
| `contributor_id`    | User ID who submitted this recipe                                                          |
| `submitted`         | Date the recipe was submitted                                                              |
| `tags`              | Food.com tags for the recipe, often containing cuisine information                          |
| `nutrition`         | Nutrition information in the form `[calories, fat, sugar, sodium, protein, saturated fat, carbs]` |
| `n_steps`           | Number of steps in the recipe                                                              |
| `steps`             | Text description of recipe steps                                                           |
| `description`       | User-provided description                                                                  |

### `RAW_interactions.csv` Columns
| Column Name         | Description                                                                                 |
|---------------------|---------------------------------------------------------------------------------------------|
| `user_id`           | User ID of the person reviewing the recipe                                                 |
| `recipe_id`         | ID of the recipe being reviewed                                                            |
| `date`              | Date of interaction                                                                        |
| `rating`            | Rating given to the recipe (0–5 scale)                                                     |
| `review`            | Text review of the recipe (not used for this analysis)                                     |

## Data Overview

- **Recipes Dataset**
  - **Rows**: 83,782
   - **Relevant Columns**:
     - `id`: Unique identifier for each recipe.
     - `tags`: A list of tags, including cuisine information.
     - `n_steps`: Number of steps in the recipe.
     - `minutes`: Preparation time in minutes.
     - `rating`: Average rating of the recipe.  

- **Interactions Dataset**
  - **Rows**: 731,927
   - **Relevant Columns**:
     - `recipe_id`: ID linking to recipes in `RAW_recipes.csv`.
     - `rating`: User rating for the recipe.
     - `review`: Review text submitted by users.

By combining these datasets, we aim to uncover insights into cuisine popularity and its relationship with ratings.

---
## Data Cleaning and Preprocessing

### Key Steps:
1. **Merging**: Combine the recipes and interactions datasets on the `recipe_id` column to link recipe details with their ratings.
2. **Handling Missing/Invalid Ratings**: Replace all ratings of `0` with `NaN` to ensure they don't distort the analysis.
3. **Calculating Average Ratings**: Compute the average rating for each recipe and add it back to the `recipes` dataset for further analysis.
4. **Extracting Cuisines**: Parse the `tags` column to identify cuisines associated with each recipe.
5. **New features**: (e.g., cuisine type) were added for better analysis.

### Justification for Replacing Ratings of 0 with `NaN`

In the `RAW_interactions.csv` dataset, a rating of `0` likely represents missing or invalid data rather than an actual user evaluation of the recipe. Including these `0` values in the analysis would incorrectly lower the average ratings of recipes and introduce bias. By replacing `0` with `NaN`:

1. **Improves Data Integrity**:
   - Ensures that only valid user ratings contribute to the average rating calculation.
   - Prevents invalid ratings from distorting the analysis of trends and patterns.

2. **Alignment with Statistical Practices**:
   - Missing or invalid data is typically excluded from summary statistics (e.g., mean, median) to avoid skewed results.

3. **Focus on Meaningful Information**:
   - Recipes with actual user feedback (ratings of 1–5) provide more meaningful insights about cuisine popularity and recipe quality.

Replacing `0` with `NaN` ensures that the analysis reflects accurate user preferences and supports reliable conclusions.

---
### **Univariate Analysis**

#### Top 10 Cuisines by Recipe Count:
<iframe
  src="assets/top-cuisines.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Key Insight**: North-American and American cuisines dominate the dataset.

This plot highlights the most frequent cuisines:
- **Top Cuisines**: North-American (14,590 recipes) and American cuisines lead, with European (4,918) and Asian (4,177) cuisines following.
- The dominance of Western cuisines likely reflects the dataset's origin and user base.

#### Distribution of Recipe Ratings:
<iframe
  src="assets/ratings-distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram reveals:
- **Skewed Ratings**: Ratings are heavily skewed toward 5.0, suggesting that users tend to rate recipes positively.
- **Lower Ratings Rare**: Few recipes receive ratings below 3.0.

---

### **Bivariate Analysis**

#### Distribution of Ratings by Cuisine:
<iframe
  src="assets/ratings-vs-cuisine.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Key Insight**:
- Italian, Greek, and Australian cuisines tend to receive the highest ratings.
- North-American and American cuisines show a broader spread in ratings, indicating variability in user satisfaction.

#### Number of Steps vs. Rating:
<iframe
  src="assets/steps-vs-rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Observations**:
- **Simpler Recipes**: Recipes with fewer steps exhibit more variability in ratings.
- **Complex Recipes**: Recipes with more steps (20+) are clustered around high ratings, possibly due to their appeal to experienced cooks.

---

### **Interesting Aggregates**

#### Average Ratings by Cuisine:
<iframe
  src="assets/average-ratings.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Key Insights**:
- **Top-Rated Cuisines**:
  - Greek and Australian cuisines lead with an average rating of **4.72**, followed by Brazilian cuisine (**4.71**).
  - Scandinavian, Spanish, and French cuisines rank slightly lower but remain highly rated.
- **Lower Ratings**: Southern-United-States and Indian cuisines have comparatively lower average ratings, presenting opportunities to explore user preferences.

| Cuisine              | Avg Rating |
|----------------------|------------|
| Greek                | 4.72       |
| Australian           | 4.72       |
| Brazilian            | 4.71       |
| Scandinavian         | 4.70       |
| Spanish              | 4.69       |
| French               | 4.68       |

---
### Intresting Aggreates

Average Rating by Cuisine:
The bar chart highlights cuisines with the highest average ratings. European, Australian, and Russian cuisines score the highest, possibly reflecting a bias toward traditional and diverse recipes in these regions.

Key Observations:

European and Australian cuisines consistently rank higher in both count and average rating, showcasing their popularity among users.
The average ratings for cuisines like "Southern-United-States" and "Indian" are lower compared to others, indicating room for potential exploration into user preferences.

---

## Assessment of Missingness

### NMAR Analysis
We hypothesize that the missing ratings in the dataset are **Not Missing at Random (NMAR)**. This is because missingness in the `rating` column may depend on factors like user preferences or the complexity of the recipe, which are not explicitly captured in the dataset. For example, a user might avoid rating recipes they found too complex or didn’t finish cooking.

To test this, we would need additional data, such as user feedback on why they didn’t leave ratings. This would allow us to determine whether the missingness is due to unobserved variables, confirming the NMAR assumption.

---

### Missingness Dependency
To explore if missingness in `rating` depends on other variables, we performed permutation tests to compare missingness across cuisines. Specifically, we tested the following hypotheses:

- **Null Hypothesis (H₀):** The missingness in `rating` is independent of the cuisine type.
- **Alternative Hypothesis (H₁):** The missingness in `rating` depends on the cuisine type.

The results of the permutation test are as follows:
- **Observed Statistic:** 117.41
- **P-value:** 0.038

<iframe
  src="assets/missingness-permutation-test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Conclusion:** The p-value of 0.038 suggests that we reject the null hypothesis at a 5% significance level. This indicates that missingness in `rating` is likely dependent on the cuisine type.

---

### Missingness Rate by Cuisine
The table below shows the proportion of missing ratings for each cuisine:

| Cuisine              | Missingness Rate |
|-----------------------|------------------|
| African              | 0.02            |
| Asian                | 0.03            |
| Australian           | 0.02            |
| Brazilian            | 0.06            |
| European             | 0.03            |
| French               | 0.03            |
| German               | 0.03            |
| Greek                | 0.02            |
| Indian               | 0.02            |
| Mexican              | 0.05            |
| Middle-Eastern       | 0.03            |
| North-American       | 0.03            |
| Other                | 0.03            |
| Scandinavian         | 0.01            |
| South-American       | 0.05            |
| South-West-Pacific   | 0.02            |
| Spanish              | 0.02            |

**Key Insights:**
- Brazilian and South-American cuisines have the highest missingness rates, potentially reflecting a lack of representation or user engagement with recipes from these regions.
- Scandinavian and Australian cuisines have the lowest missingness rates, indicating consistent user feedback.

---

## Hypothesis Testing

**Hypothesis**:
- Null Hypothesis (H₀): The average rating of Italian recipes is equal to that of American recipes.
- Alternative Hypothesis (H₁): The average rating of Italian recipes is higher than that of American recipes.

**Test Statistic**:
The difference in mean ratings between Italian and American recipes.

**Results**:
p-value = 0.03 (significant at α = 0.05).

---

## Framing a Prediction Problem

**Prediction Problem**:
Using recipe features such as cuisine, preparation time, and number of steps, predict whether a recipe receives a high rating (> 4.5). This is a **binary classification** problem evaluated using **accuracy** and **F1-score**.

---

## Baseline Model

Our baseline model used two features:
- `n_steps`: Number of preparation steps.
- `minutes`: Total preparation time.

Model Performance:
- Accuracy: 0.65
- F1-Score: 0.62

---

## Final Model

The final model improved upon the baseline by adding the following features:
- `cuisine`: One-hot encoded.
- `average_rating_by_user`: The average rating left by each user.

**Hyperparameters Tuned**:
- Tree depth and the number of estimators for a Random Forest Classifier.

Model Performance:
- Accuracy: 0.78
- F1-Score: 0.74

---

## Fairness Analysis

**Fairness Question**:
Does the model perform differently for recipes tagged as "desserts" compared to "non-desserts"?

**Results**:
A permutation test indicated that there is no significant difference in performance between the two groups, suggesting the model is fair.

<iframe src="assets/fairness-analysis-plot.html" width=800 height=600 frameBorder=0></iframe>
