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

This cleaning reduced noise and resulted in a cleaner, structured dataset with 220,373 rows.

### 📊 Univariate Analysis: Distribution of Total Fat
<iframe src="assets/tot_fat_dist.html" width="800" height="600" frameborder="0"></iframe>

We first examined the distribution of total fat across all recipes. Most recipes have a fat content between 10%–30% of the daily recommended value, with a steep drop-off afterward. A long right tail suggests a smaller subset of high-fat recipes. The distrubtion is highly skewed to the right.


### 📈 Bivariate Analysis: Relationship between Total Fat and Mean Recipe Rating
<iframe src="assets/totalfatandmeanreciperating.html" width="800" height="600" frameborder="0"></iframe>

Next, we looked at the relationship between total fat and the mean recipe rating. Despite some noise, there appears to be a loose cluster of high-fat recipes receiving high ratings, but overall, the correlation is weak. This suggests that while fat content may contribute to taste, it's not the sole factor driving user ratings.


### 📊 Interesting Aggregates: Pivot Table: Fat Content by Recipe Rating

We grouped recipes by their user ratings to analyze whether higher-rated recipes tend to have more or less fat content and longer preparation time.
Here is a snapshot of my pivot table:

|   fat_rounded |    mean |   median |   min |   max |
|--------------:|--------:|---------:|------:|------:|
|             0 | 4.69013 |  4.91667 |     1 |     5 |
|             5 | 4.65141 |  4.83333 |     1 |     5 |
|            10 | 4.6696  |  4.83333 |     1 |     5 |
|            15 | 4.68125 |  4.875   |     1 |     5 |
|            20 | 4.69273 |  4.86667 |     1 |     5 |

#### Trends of the Pivot Table
<iframe src="assets/pivottabletrends.html" width="800" height="600" frameborder="0"></iframe>

This line plot reveals a few important patterns:

- **Mean and median ratings stay high and stable** across most fat levels, suggesting users rate most recipes positively regardless of fat content.
- However, the **minimum rating fluctuates wildly** — especially at high fat levels — indicating that while many high-fat recipes are loved, they are also more divisive.
- There’s a consistent **cluster of low-rated recipes below 100% DV** of fat, suggesting low-fat recipes may underperform in user satisfaction more often than others.

This aggregation helped shape our hypothesis: *Does fat content actually affect how a recipe is rated?* The variability in min ratings suggested a possible direction for deeper statistical testing.
