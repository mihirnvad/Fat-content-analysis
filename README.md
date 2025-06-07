# Fat-content-analysis
DSC80 project: Analyzing Total Fat Content In Relation To Recipe Ratings

# Recipes and Ratings: Total Fat Content and Recipe Ratings
**Mihir Vad**

---

## Introduction

This project analyzes recipe and review data from Food.com to explore and model what influences a recipe’s rating. The goal is to predict a recipe's rating based on its ingredients, review, and nutritional value.

The motivation behind this question is simple but powerful: can we determine whether total fat content influences how highly a recipe is rated? Given the  online social presence for cooking platforms, this seeks to help users look through healthy eating options.

Datasets Used:

### 1. Recipes Dataset (83,782 entries)

| Column           | Description                                                                                                                                     |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`           | Recipe Name                                                                                                                                     |
| `id`             | Recipe ID                                                                                                                                       |
| `minutes`        | Time to Prepare Recipe in Minutes                                                                                                               |
| `contributor_id` | User ID for Recipe Uploader                                                                                                                     |
| `submitted`      | Date of Recipe Submission                                                                                                                       |
| `tags`           | Food.com Tags for Recipe                                                                                                                        |
| `nutrition`      | List of Nutrition Info in the form `[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbs (PDV)]` |
| `n_steps`        | Total Steps in a Recipe                                                                                                                         |
| `steps`          | Recipe Steps, in Order                                                                                                                          |
| `description`    | User-Provided Description                                                                                                                       |

### 2. Interaction Dataset (Reviews) (731,927 entries)

| Column      | Description        |
|-------------|--------------------|
| `user_id`   | User ID            |
| `recipe_id` | Recipe ID          |
| `date`      | Date of Review     |
| `rating`    | Rating             |
| `review`    | Review Text        |


After merging, the combined dataset contains 234,429 rows of recipes with corresponding reviews and ratings. The columns I mainly focused on were total fat (PDV) and rating for the question: 
### Does the total fat content of a recipe relate to how highly it’s rated by users?

We also explore this relationship through hypothesis testing and a predictive model that estimates a recipe’s rating based on its fat content and other features.

---

## Data Cleaning and Exploratory Data Analysis

We performed the following steps to clean and prepare our data:

- Merged the recipes and reviews datasets on `recipe ID`.
- Removed invalid ratings (replaced `0` with `NaN`) and computed each recipe’s average rating.
- Extracted nutrition values into separate columns like `calories (#)`, `total fat (PDV)`, etc.
- Converted stringified lists into real Python lists for `tags`, `steps`, and `ingredients`.
- Dropped outliers with `calories > 1500` or `cook time > 360 minutes (6 hours)`.
- Engineered new features like `n_ingredients` and dropped irrelevant columns.

Here’s a snapshot of the cleaned dataset:

| calories (#) | total fat (PDV) | minutes | rating | n_ingredients |
|--------------|------------------|---------|--------|----------------|
| 138.4        | 10               | 40      | 4      | 9              |
| 595.1        | 46               | 45      | 5      | 11             |
| 194.8        | 20               | 40      | 5      | 9              |
| 194.8        | 20               | 40      | 5      | 9              |
| 194.8        | 20               | 40      | 5      | 9              |

This pipeline reduced noise and resulted in a cleaner, structured dataset with 220,373 rows.

### 📊 Univariate Analysis: Distribution of Total Fat
<iframe src="assets/tot_fat_dist.html" width="800" height="600" frameborder="0"></iframe>

Saturated fat follows a left-skewed distribution, centered around 20–30% of daily value, with some very high outliers.

### 📈 Bivariate Analysis: Relationship between Total Fat and Mean Recipe Rating
<iframe src="assets/totalfatandmeanreciperating.html" width="800" height="600" frameborder="0"></iframe>

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