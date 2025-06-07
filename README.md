# Fat-content-analysis
DSC80 project: Analyzing Total Fat Content In Relation To Recipe Ratings

# Recipes and Ratings: Total Fat Content and Recipe Ratings
**Mihir Vad**

---

## Introduction

This project analyzes recipe and review data from Food.com to explore and model what influences a recipe’s rating. The core goal is to predict a recipe's rating based on its ingredients, review, and nutritional profile.

The motivation behind this question is simple but powerful: can we learn what makes a recipe popular or well-received? Given the increasing reliance on online platforms for cooking ideas, helping users navigate healthy and well-rated options can have real-world impact.

Datasets Used
We work with two datasets:

1. Recipes Dataset (83,782 entries)
Column	Description
name	Name of the recipe
id	Unique recipe ID
minutes	Total time in minutes to prepare the recipe
nutrition	List containing [calories, total fat, sugar, sodium, protein, saturated fat, carbohydrates] as % daily values
n_steps	Number of steps in the recipe
steps	Step-by-step recipe instructions
description	User-provided recipe description
tags	Tags like cuisine, meal type, etc.

2. Reviews Dataset (731,927 entries)
Column	Description
user_id	Unique ID of the user posting the review
recipe_id	The ID of the recipe reviewed
rating	User rating from 1 to 5 stars
review	Text review of the recipe
date	Date the review was submitted

After merging, the combined dataset contains 234,429 rows of recipes with corresponding reviews and ratings. This allows us to investigate our central question and build a model to predict how users will rate a given recipe.

---

## Data Cleaning and Exploratory Data Analysis

We cleaned and prepared the dataset by:
- Merging recipes and reviews
- Converting `nutrition` into separate columns
- Replacing invalid ratings (0s) with `NaN`
- Removing outliers in calories and minutes
- Adding features like `n_ingredients` and discretized prep time

### 📊 Distribution of Saturated Fat
<iframe src="assets/ratings_dist.html" width="800" height="600" frameborder="0"></iframe>

Saturated fat follows a left-skewed distribution, centered around 20–30% of daily value, with some very high outliers.

### 📈 Prep Time vs Saturated Fat
<iframe src="assets/tot_fat_dist.html" width="800" height="600" frameborder="0"></iframe>

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