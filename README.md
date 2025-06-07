# Fat-content-analysis
DSC80 project: Analyzing Total Fat Content In Relation To Recipe Ratings

# Recipes and Ratings: Total Fat Content and Recipe Ratings
**Mihir Vad**

---

## Introduction

This project investigates whether the time it takes to prepare a recipe influences its saturated fat content. Saturated fat is linked to heart disease, and processed foods — often fast or overly complex — are high in it. Using real recipe and review data from Food.com (2008–2018), we explore relationships between prep time and nutritional health, clean the data, analyze trends, test hypotheses, and build a predictive model for saturated fat content.

---

## Data Cleaning and Exploratory Data Analysis

We cleaned and prepared the dataset by:
- Merging recipes and reviews
- Converting `nutrition` into separate columns
- Replacing invalid ratings (0s) with `NaN`
- Removing outliers in calories and minutes
- Adding features like `n_ingredients` and discretized prep time

### 📊 Distribution of Saturated Fat
<iframe src="assets/fig1.html" width="800" height="600" frameborder="0"></iframe>

Saturated fat follows a left-skewed distribution, centered around 20–30% of daily value, with some very high outliers.

### 📈 Prep Time vs Saturated Fat
<iframe src="assets/fig2.html" width="800" height="600" frameborder="0"></iframe>

There’s a weak upward trend: recipes with longer prep times tend to have more saturated fat, possibly due to oil-heavy methods like frying or roasting.

### 📋 Pivot Table: Rating vs Fat and Minutes

```markdown
| Rating | Minutes | Saturated Fat (sat_fat) |
|--------|---------|--------------------------|
| 1.0    | 99.67   | 46.68                    |
| 2.0    | 98.02   | 42.88                    |
| 3.0    | 87.50   | 40.08                    |
| 4.0    | 91.58   | 36.43                    |
| 5.0    |106.92   | 39.23                    |