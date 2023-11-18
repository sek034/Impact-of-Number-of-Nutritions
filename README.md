# Impact of Number of Nutritions

This data science study examines the relationship between calories of the recipe and their corresponding number of ingredients. This is a project for DSC 80 at UC San Diego.

*By Bryan Cha & Chloe Kim*

## Project Introduction

In the constantly-evolving world of gastronomy, where the combination of culinary art and science is more pronounced than ever, understanding the nutritional aspects of what we eat has become essential. This project explores a fascinating area of culinary analytics: the relationship between the number of ingredients and carolies in a recipe. 

The central question of our analysis is: **Is there a relationship between the number of ingredients a recipe requires and its calorie content?** This question is not just of academic interest but of practical significance in our daily lives.


### Datasets Used in the project

We based our analysis on two comprehensive datasets.

The first dataset contains information of a variety of recipes that have been posted on food.com since 2008. This dataset contains 83782 unique recipes with `name` as its index column and 11 other columns conveying the following information:

| Column          | Description   |
|-----------------|---------------|
| `name`          | Recipe name   |
| `id`            | Recipe ID     |
| `minutes`       | Minutes to prepare recipe |
| `contributor_id`| User ID who submitted this recipe |
| `submitted`     | Date recipe was submitted |
| `tags`          | Food.com tags for recipe |
| `nutrition`     | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `n_steps`       | Number of steps in recipe |
| `steps`         | Text for recipe steps, in order |
| `description`   | User-provided description |
| `ingredients`   | List of ingredients requried for the recipe |
| `n_ingredients` | Number of ingredients required for the recipe |

The second dataset contains information of reviews and ratings submitted for the recipes through food.com, which conveys 731927 reviews with `user_id` as its index column and 4 other columns conveying the following information:

| Column          | Description   |
|-----------------|---------------|
| `user_id`       | User ID       |
| `recipe_id`     | Recipe ID     |
| `date`          | Date of interaction |
| `rating`        | Rating given  |
| `review`        | Review text   |

In our project, we mainly used `n_ingredients` column and `calories` column that we added to our dataset by extracting calories information from `nutrition` column. We also inspected `rating`, `review`, and `minutes` columns to examine missingness of certain values. 

---



