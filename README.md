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
| Column Name       | Description                                                                                      |  
|--------------------|--------------------------------------------------------------------------------------------------|  
| `name`            | Recipe name                                                                                     |  
| `id`              | Recipe ID                                                                                       |  
| `minutes`         | Minutes to prepare the recipe                                                                    |  
| `contributor_id`  | User ID who submitted this recipe                                                                |  
| `submitted`       | Date the recipe was submitted                                                                    |  
| `tags`            | Food.com tags for the recipe, often containing cuisine information                               |  
| `nutrition`       | Nutrition information in the form `[calories, fat, sugar, sodium, protein, saturated fat, carbs]`|  
| `n_steps`         | Number of steps in the recipe                                                                    |  
| `steps`           | Text description of recipe steps                                                                 |  
| `description`     | User-provided description                                                                        |  
| `ingredients`     | List of ingredients used in the recipe                                                          |  
| `n_ingredients`   | Number of ingredients in the recipe                                                             |  


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
  src="assets/Top_10CusinesbyCusine.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Key Insight**: North-American and American cuisines dominate the dataset.
- **Top Cuisines**: North-American (14,150 recipes) and American cuisines lead, followed by European (4,900) and Asian (4,177) cuisines.
- Percentages are annotated in bar plots, showcasing the dominance of Western cuisines in the dataset. North-American (14,590 recipes) and American cuisines lead, with European (4,918) and Asian (4,177) cuisines following.
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
-  annotations to highlight lower-rated recipes.

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
- The scatterplot includes correlation values (`Correlation = 0.35`), indicating a moderate positive relationship between steps and ratings.
- Recipes with more steps tend to cluster around higher ratings, confirming complexity appeals to experienced cooks.


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

| Cuisine              | Avg Rating | Std. Dev. |
|----------------------|------------|-----------|
| Greek                | 4.72       | 0.08      |
| Australian           | 4.72       | 0.09      |
| Brazilian            | 4.71       | 0.10      |
| Scandinavian         | 4.70       | 0.12      |
| Spanish              | 4.69       | 0.15      |
| French               | 4.68       | 0.14      |


---
### Intresting Aggreates

Average Rating by Cuisine:
The bar chart highlights cuisines with the highest average ratings. European, Australian, and Russian cuisines score the highest, possibly reflecting a bias toward traditional and diverse recipes in these regions.

Key Observations:

European and Australian cuisines consistently rank higher in both count and average rating, showcasing their popularity among users.
The average ratings for cuisines like "Southern-United-States" and "Indian" are lower compared to others, indicating room for potential exploration into user preferences.

---

## Assessment of Missingness

| Cuisine              | Missingness Rate |
|-----------------------|------------------|
| African              | 0.02            |
| Asian                | 0.03            |
| Australian           | 0.02            |
| Brazilian            | 0.06            |
| Scandinavian         | 0.01            |

- Missingness is higher for Brazilian (6%) and South-American (5%) cuisines, indicating lower user engagement.
- Scandinavian cuisines have the lowest missingness rate (1%), reflecting consistent user feedback.

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
### **Hypothesis Testing**
**Hypothesis**:
- Null Hypothesis (H₀): Italian and American cuisines have the same average ratings.
- Alternative Hypothesis (H₁): Italian recipes have higher average ratings.

**Results**:
- Test Statistic: 2.15
- P-value: **0.03** (significant at α = 0.05).


## Framing a Prediction Problem

**Prediction Problem**:
Using recipe features such as cuisine, preparation time, and number of steps, predict whether a recipe receives a high rating (> 4.5). This is a **binary classification** problem evaluated using **accuracy** and **F1-score**.

---

## Baseline Model
#### Features and Preprocessing
For the baseline model, we used two features:
1. **`cuisine` (nominal)**: This categorical feature was one-hot encoded using `OneHotEncoder`, transforming the column into multiple binary columns representing different cuisine types. This allowed the model to handle the categorical nature of the feature effectively.
2. **`minutes` (quantitative)**: This numerical feature was scaled using `StandardScaler` to standardize its values. Scaling helps the model treat all numerical features on the same scale and improves performance.

The response variable, **`high_rating`**, was defined as a binary indicator where recipes with an average rating of 4.5 or above were considered high-rated (1), and others were low-rated (0). The dataset showed a class imbalance with approximately 75% high-rated and 25% low-rated recipes, requiring a technique like oversampling for better balance.

To address the imbalance, we applied **SMOTE (Synthetic Minority Oversampling Technique)** after preprocessing the training data. This step synthetically increased the representation of the minority class (`Low Rating`), leading to a more balanced training set.

#### Model
We implemented a **Random Forest Classifier** with balanced class weights to further handle the class imbalance and used it within a `Pipeline` that includes preprocessing. Random Forest was chosen as it provides robust and interpretable results for tabular data with both categorical and numerical features.

#### Results
The model was evaluated using the test set, and the following metrics were obtained:

1. **Precision**:
   - **Low Rating**: 26% of the recipes predicted as low-rated were actually low-rated. This relatively low precision indicates that the model struggles with false positives for the `Low Rating` class.
   - **High Rating**: 77% of the recipes predicted as high-rated were actually high-rated, which demonstrates better performance for the majority class.

2. **Recall**:
   - **Low Rating**: 58% of the actual low-rated recipes were correctly identified by the model. This is a reasonable improvement, given the imbalanced dataset and the difficulty of identifying low-rated recipes.
   - **High Rating**: 46% of the actual high-rated recipes were correctly classified. This lower recall indicates that many high-rated recipes were misclassified as low-rated.

3. **F1-Score**:
   - **Low Rating**: The F1-score for the low-rated class is 0.36, reflecting the trade-off between precision and recall for this minority class.
   - **High Rating**: The F1-score for the high-rated class is 0.57, demonstrating better overall performance for the majority class.
   - **Overall F1-Score**: The weighted F1-score across both classes is 0.52.

4. **Accuracy**:
   - The model achieved an overall accuracy of 49%, which is not particularly high. However, accuracy is not a reliable metric for imbalanced datasets, so the F1-score provides a better evaluation of the model's performance.

#### Interpretation of Results
The baseline model demonstrates balanced performance across both classes, primarily due to the use of SMOTE and balanced class weights. While the model achieves better precision for the `High Rating` class (the majority class), it sacrifices some precision to improve recall for the `Low Rating` class. The weighted F1-score of 0.52 suggests that there is room for improvement, particularly in identifying low-rated recipes.

The use of only two features, `cuisine` and `minutes`, limits the predictive power of the baseline model. However, it sets a solid foundation for further improvement in the final model by incorporating additional meaningful features and optimizing hyperparameters.

#### Next Steps
In the final model, we will:
1. Engineer new features, such as `n_ingredients` (the number of ingredients in a recipe) and a custom `complexity_score`.
2. Perform hyperparameter tuning on the Random Forest Classifier or explore alternative models.
3. Reevaluate the performance to assess improvements in the model's ability to generalize and handle the class imbalance effectively.

---

## Final Model


---

## Fairness Analysis

