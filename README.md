# Ratings and Cuisine Popularity

by [Your Name] (your_email@domain.com)

*Note*: Replace the placeholders with your own details and ensure the title and structure align with your project's focus.

---

## Introduction

In this project, we analyze the popularity and ratings of recipes across different cuisines on Food.com. The primary focus is to determine whether certain cuisines, such as Italian or Mexican, are rated more highly than others and to explore factors that might influence these ratings.

---

## Cleaning and EDA

<iframe src="assets/cleaning-eda-plot.html" width=800 height=600 frameBorder=0></iframe>

**Data Cleaning Steps**:
- Replaced missing ratings with `NaN`.
- Filtered tags to extract unique cuisines.
- Merged datasets to include relevant columns for analysis.

**Exploratory Data Analysis**:
- Plotted distributions of recipe ratings.
- Analyzed the relationship between preparation time, number of steps, and ratings.

---

## Assessment of Missingness

**NMAR Analysis**:
We hypothesize that missingness in the `review` column is likely **Not Missing At Random (NMAR)** because users who leave a low rating may choose not to leave a review. Additional information, such as the user's intent or feedback mechanism, would be required to confirm this.

**Permutation Test Results**:
<iframe src="assets/missingness-plot.html" width=800 height=600 frameBorder=0></iframe>

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
