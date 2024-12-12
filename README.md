# Ratings Popularity by Cuisine

by [Camille Tran] (Ctt011@ucsd.edu)
website: https://ctt011.github.io/CuisineCritique/

---
## Introduction

Food is the epitome of universal language and culture, and analyzing recipes allows us to explore cultural diversity and user preferences. This project leverages data from Food.com to investigate whether there is a relationship between cuisines of a certain ethnic type and their general perceived taste response of either a favorable rating or a not-so  favorable response with a lower taste rating.Although there are limitless and varying factors which could skew results one way or another, such as taste preferences among individuals, we were curious to see the relationships that exists for recipes’ tastes preferences in regards to its’ ethnic origin.


#### Research Question:
**Does ethnic orgin of a recipe significantly affect ratings, and which regional types are most likely to be rated highly?**

---
#### About the Data

To start exploring taste preferences among ethnic groups, we have two datasets to work with. The first dataset contains approximately 84k recipes and their respective recipe names, preparation time, number of ingredients and steps, as well as other information such as nutritional content and descriptive data in tag.  We were particularly interested in how in `n_steps`, prep time as we were curious to see how this impacted taste preferences.For the second dataset, there were over 730k rating records, with primary interest in the user rating and pertinent data submitted in the ‘tag’ column that could potentially give  us more insight into our research.

The analysis is based on two datasets:
1. **`RAW_recipes.csv`**: Contains detailed information about recipes, including their names, preparation time, ingredients, steps, and tags.
2. **`RAW_interactions.csv`**: Includes user interactions with the recipes, such as ratings and reviews.

#### RAW_recipes.csv Columns

| Column Name    | Description                                                                                     |
|:---------------|:------------------------------------------------------------------------------------------------|
| name           | Recipe name                                                                                     |
| id             | Recipe ID                                                                                       |
| minutes        | Minutes to prepare the recipe                                                                   |
| contributor_id | User ID who submitted this recipe                                                               |
| submitted      | Date the recipe was submitted                                                                   |
| tags           | Food.com tags for the recipe, often containing cuisine information                              |
| nutrition      | Nutrition information in the form [calories, fat, sugar, sodium, protein, saturated fat, carbs] |
| n_steps        | Number of steps in the recipe                                                                   |
| steps          | Text description of recipe steps                                                                |
| description    | User-provided description                                                                       |
| ingredients    | List of ingredients used in the recipe                                                          |
| n_ingredients  | Number of ingredients in the recipe                                                             |

#### RAW_interactions.csv Columns

| Column Name   | Description                                            |
|:--------------|:-------------------------------------------------------|
| user_id       | User ID of the person reviewing the recipe             |
| recipe_id     | ID of the recipe being reviewed                        |
| date          | Date of interaction                                    |
| rating        | Rating given to the recipe (0–5 scale)                 |
| review        | Text review of the recipe (not used for this analysis) |


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
     
## Data Cleaning and Exploratory Data Analysis

### Key Steps:
1. **Merging**: Combine the recipes and interactions datasets on the `recipe_id` column to link recipe details with their ratings.
2. **Handling Missing/Invalid Ratings**: Replace all ratings of `0` with `NaN` to ensure they don't distort the analysis.
3. **Calculating Average Ratings**: Compute the average rating for each recipe and add it back to the `recipes` dataset for further analysis.
4. **Extracting Cuisines**: Parse the `tags` column to identify cuisines associated with each recipe.
5. **New feature**: cuisine type added for better analysis.

In order to efficiently work with the data, we needed to perform various cleaning steps. First, joined by the ‘recipe_id’, we merged the two tables to be able to extract the taste preferences by all their attributes for better analysis.  Upon further inspection, we found ratings of 0 for many recipes and decided to replace these records with ratings of 0 to NaN, to eliminate bias in the ‘ratings’ columns values, which could skew the numbers. After replacing the missing ratings, we calculated the ‘average_rating’ across all recipes of the same ID and ensured that average rating was added back into the dataset. In addition, we created a new feature variable from the ‘tags’ column by extracting the cuisine from each row. I found that the majority The cuisine column contains ethnic histories of the recipe within the column, “tag” which was then parsed out, and added as a new feature (cuisine type). 

### Cleaned_recipes_df

| name                                 | cuisine        |   minutes | ingredients                                                                                                                                                                                                                             |   n_ingredients |   average_rating |   n_steps |
|:-------------------------------------|:---------------|----------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------------:|----------:|
| 1 brownies in the world    best ever | other          |        40 | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour']                                                          |               9 |                4 |        10 |
| 1 in canada chocolate chip cookies   | north-american |        45 | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                                                                             |              11 |                5 |        12 |
| 412 broccoli casserole               | other          |        40 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']                                                                   |               9 |                5 |         6 |
| millionaire pound cake               | north-american |       120 | ['butter', 'sugar', 'eggs', 'all-purpose flour', 'whole milk', 'pure vanilla extract', 'almond extract']                                                                                                                                |               7 |                5 |         7 |
| 2000 meatloaf                        | other          |        90 | ['meatloaf mixture', 'unsmoked bacon', 'goat cheese', 'unsalted butter', 'eggs', 'baby spinach', 'yellow onion', 'red bell pepper', 'simply potatoes shredded hash browns', 'fresh garlic', 'kosher salt', 'white pepper', 'olive oil'] |              13 |                5 |        17 |

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
- **Top Cuisines**: North-American (14,000 recipes) and American cuisines lead, followed by European (4,600) and Asian (4,100) cuisines.
- Percentages are annotated in bar plots, showcasing the dominance of Western cuisines in the dataset. North-American (14,000 recipes) and American cuisines lead, with European (4,600) and Asian (4,100) cuisines following.


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
  src="assets/distribution-ratings-by-cuisine.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Key Insight**:
- Spanish, South-west-pacific, and Greek cuisines tend to receive the highest ratings.
- North-American cuisines shows the broadest spread in ratings, indicating variability in user satisfaction.


#### Number of Steps vs. Rating:
<iframe
  src="assets/relationship_steps_rating.html"
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
  src="assets/pivot_table_cuisine.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Key Insights**:
- **Top-Rated Cuisines**:
  - Greek and Australian cuisines lead with an average rating of **4.72**, followed by puerto-rican, brazilian, middle-eastern cuisine (**4.71**).
  - Hungarian ranks the lowest in average rating but, has a lower number of recipes than most.
---

## Assessment of Missingness
### NMAR Analysis

In my dataset, the column ‘Review’ is likely to be **Not Missing at Random (NMAR)** , as it's not solely dependent on observed variables like rating. Handing NMAR values is challenging to handle since the missing mechanism cannot be fully explained or qualitatively calculated by other observed data. Which makes it necessary to consider the biases surrounding the data set, we also have to take into consideration that the user might leave reviews for recipes that they are most familiar with , or skip leaving reviews all together. 

---

### Missingness Dependency
To explore if missingness in `rating` depends on other variables, we performed permutation tests to compare missingness across cuisines. Specifically, we tested the following hypotheses:

- **Null Hypothesis (H₀):** The missingness in `rating` is independent of the cuisine type.
- **Alternative Hypothesis (H₁):** The missingness in `rating` depends on the cuisine type.

The results of the permutation test are as follows:
- **Observed Statistic:** 117.34
- **P-value:**  0.039

<iframe
  src="assets/dependency_test_cuisine.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**Conclusion:** The p-value of 0.039 suggests that we reject the null hypothesis at a 5% significance level. This indicates that missingness in `rating` is likely dependent on the cuisine type.

---
We also analyzed the missingness of the `average_rating` column in the dataset and evaluated its dependency on two other columns: `minutes` and `n_steps`.

#### Dependent Column: `minutes`
- **Hypothesis**: The missingness of `average_rating` depends on the preparation time (`minutes`).
- **Observed Statistic**: 117.34
- **P-value**: 0.023
- **Interpretation**: The p-value is less than 0.05, indicating that the missingness of `average_rating` is likely dependent on the `minutes` column. Recipes with missing ratings tend to have significantly different preparation times compared to those with ratings.

#### Independent Column: `n_steps`
- **Hypothesis**: The missingness of `average_rating` does not depend on the number of steps (`n_steps`).
- **Observed Statistic**: 1.49
- **P-value**: 0.0
- **Interpretation**: Although the observed statistic is small, the p-value suggests there is evidence of dependence. This result is unexpected and may warrant further exploration.

---

<iframe
  src="assets/dependency_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

### Conclusion
The missingness of `average_rating` is likely dependent on `minutes` but less so on `n_steps`. These findings suggest that preparation time might influence user behavior around rating submissions. Further exploration could involve additional columns or external data sources to understand the data-generating process.

---
## Hypothesis Testing

- **Null Hypothesis (H₀):** The mean rating of Greek recipes is the same as the overall mean rating of all recipes. Any observed difference in mean ratings is due to random chance.
- **Alternative Hypothesis (H₁):** The mean rating of Greek recipes is higher than the overall mean rating of all recipes.

## Test Statistic
- The test statistic used is the difference in means between Greek recipes' ratings and the overall mean rating.
- **Observed Statistic:** 0.0996
- **P-value:** 0.0

---
The permutation test was conducted with 1,000 permutations
<iframe
  src="assets/greek_cuisine_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---
### Conclusion
Given the p-value is effectively 0.0 (less than a typical significance level of 0.05):
- **Conclusion:** We reject the null hypothesis and conclude that Greek recipes have significantly higher ratings than the overall average at the chosen significance level.

---
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

- **Model Type**: Random Forest Classifier  
- **Hyperparameters Used**:  
  - `max_depth`: None (no restriction on tree depth)  
  - `min_samples_split`: 10 (minimum samples required to split a node)  
  - `n_estimators`: 100 (number of trees in the forest)  

- **Features**:  
  1. **`minutes`**: Quantitative feature representing the time required for a recipe.  
  2. **`n_ingredients`**: Quantitative feature representing the number of ingredients in a recipe.  
  3. **`complexity_score`**: Derived quantitative feature calculated as `minutes / (n_ingredients + 1)`.  
  4. **`cuisine`**: Nominal feature representing the cuisine category of the recipe.  

  These features were selected based on their relevance to predicting whether a recipe has a high rating (`average_rating >= 4.5`).

---

### Preprocessing Steps

- **Numeric Features**: Standardized using `StandardScaler` to ensure uniform scaling and prevent dominance by large numeric values.  
- **Categorical Features**: Encoded using `OneHotEncoder` to transform categorical data (`cuisine`) into a numerical format suitable for machine learning models.  
- **Data Balancing**: Applied SMOTE (Synthetic Minority Oversampling Technique) to address the class imbalance in the target variable (`high_rating`: 75.08% positive vs. 24.92% negative).

---

### Evaluation Results

- **Test Set Performance**:
  - **Accuracy**: 61%  
  - **Precision**:  
    - Class 0 (Low Rating): 0.26  
    - Class 1 (High Rating): 0.76  
  - **Recall**:  
    - Class 0 (Low Rating): 0.30  
    - Class 1 (High Rating): 0.72  
  - **F1-Score**:  
    - Class 0 (Low Rating): 0.28  
    - Class 1 (High Rating): 0.74  
    - Weighted F1-Score: 0.571  

---

### Model Interpretation

- The model performs significantly better for recipes with **high ratings** (`Class 1`) than for recipes with **low ratings** (`Class 0`):
  - **Precision for Class 1** is 76%, meaning 76% of recipes predicted as highly rated are indeed highly rated.  
  - **Recall for Class 1** is 72%, indicating the model identifies 72% of all highly rated recipes.  

- For recipes with **low ratings** (`Class 0`), the model struggles due to the inherent class imbalance despite the use of SMOTE:  
  - **Precision for Class 0** is 26%, indicating higher misclassification of low-rated recipes as highly rated.

- The **macro average F1-Score** (0.51) reflects moderate handling of both classes, with better performance for the majority class (high ratings).

---

### Improvement from Baseline Model

The final model improves upon the baseline model in several ways:
- **Feature Engineering**: Added `n_ingredients` and `complexity_score`, which improved predictive performance.  
- **Hyperparameter Tuning**: Used `GridSearchCV` to identify optimal settings for model depth, number of estimators, and minimum split size, enhancing model generalization.  
- **Overall Performance**: The weighted F1-Score increased slightly, showing improved balance in predictions compared to the baseline model.

---

### Fairness Analysis

## Choice of Groups
- **Group X**: Recipes labeled as `italian`.
- **Group Y**: Recipes labeled as `mexican`.

## Evaluation Metric
- **Metric**: Precision  
Precision was chosen because it evaluates the model's ability to correctly predict high ratings for each group, which is critical for understanding potential disparities.

## Hypotheses
- **Null Hypothesis (H₀)**: The model is fair. The precision for recipes in the `italian` group is equal to the precision for recipes in the `mexican` group. Any differences are due to random chance.  
- **Alternative Hypothesis (Hₐ)**: The model is unfair. The precision for recipes in the `italian` group is significantly different from the precision for recipes in the `mexican` group.

#### Test Statistic and Significance Level
- **Test Statistic (T)**: The absolute difference in precision scores between the `italian` and `mexican` groups
- **Significance Level (α)**: 0.05

#### Results
- **Observed Test Statistic (Tₒᵦₛ)**: Tₒᵦₛ = 0.8265
- **p-value**: p = 1.0

#### Conclusion
Since p = 1.0 > α = 0.05, we fail to reject the null hypothesis. This means there is insufficient evidence to suggest that the model's precision differs significantly between the `italian` and `mexican` groups. The model is deemed fair based on this analysis.
#### Visualization
Below is a histogram of the null distribution (Tₙᵤₗₗ) generated from the permutation test, with the observed test statistic (Tₒᵦₛ) marked:

<iframe
  src="assets/permutation_test_fairness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram shows that the observed test statistic (\(T_{\text{obs}}\)) falls well within the distribution of the null hypothesis, reinforcing the conclusion of fairness.


