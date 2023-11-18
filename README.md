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


---

## Assessment of Missingness
This section of the study will examine missingness in certain columns on the combined dataset. 
`
### NMAR Analysis

In the study of missing data mechanisms, Not Missing At Random (NMAR) refers to a situation in which a data point's likelihood of being missing is directly related to its value itself, whether observed or not. This mechanism is particularly pertinent to the `review` column in our dataset, where it is possible to infer that a user's experience with the recipe is the explanation behind the absence of a review.

Users are not as likely to submit reviews unless they have specific critiques or complaints about the recipe. A user's subjective experience often influences their decision to write a review; feedback is likely to be generated by particularly pleasant or negative experiences with the recipe. On the other hand, if an encounter does not leave an impression or elicit a strong response - possibly because the recipe is thought to be mediocre or unremarkable - may result in a lack of desire to write a review.

This pattern suggests that the missingness of data in `review` column is potentially NMAR as the absence of a review may correlate with unobserved, experiential factors inherent to the recipe. Understanding this missingness mechanism is crucial to our research as it highlights the necessity for specialized statistical methods that can take into account any bias that may have been introduced by the NMAR data.

### Missingness Dependency

This section centers on examining the pattern of absent values in the `rating` column within our integrated dataset. The goal is to investigate if this missingness depends on specific recipe attributes - specifically, the calories contained in the dish and the amount of time spent in preparation. This inquiry is crucial as it could point to underlying patterns in the relationship between these recipe attributes and the completeness of rating data. 

- To investigate this, we created a new column `rating_missingness` in which the value is `True` if rating is missing, `False` if rating is not missing. 

#### 1. Rating and Calories

**Null hypothesis**: The missingness of `rating` column does not depend on calories.

**Alnernative hypothesis**: The missingness of `rating` column does depend on calories.

**Test Statistics**: The absolute mean difference between two distributions of rating and calories.

First, we constructed a plot that indicates distributions of calories with and without the presence of rating below.

---

Then, we performed a permutaion test by first calculating the observed test statistic - which is the absolute mean difference in `calories` between the group of recipes with ratings and the group without ratings. We got 69.007 as the observed test statistic. 

We generated 1000 simulation results for each absolute difference by performing permutation testing to shuffle the missingness of rating 1000 times. 

The empirical distribution of the absolute difference in calorie means over 1000 permutations is plotted below; our observed test statistic is indicated by the red line. 

---

We calculated the p-value to be approximately 0.0, which is less than the significance value we chose, 0.05. Therefore, we **reject the null hypothesis** that the missingness of `rating` column does not depend on calories. Since we can conclude that the missingness of `rating` **does** depend on calories, we can say that the missingness of `rating` is **MAR** as it depends another column which is `calories`.



#### 2. Rating and Minutes

**Null hypothesis**: The missingness of `rating` column does not depend on minutes.

**Alnernative hypothesis**: The missingness of `rating` column does depend on minutes.

**Test Statistics**: The absolute mean difference between two distributions of rating and minutes.

Similarly, we constructed a plot that indicates distributions of minutes with and without the presence of rating below.

---

We performed a permutaion test by first calculating the observed test statistic - which is the absolute mean difference in `minutes` between the group of recipes with ratings and the group without ratings. We got 51.451 as the observed test statistic. 

We generated 1000 simulation results for each absolute difference by performing permutation testing to shuffle the missingness of rating 1000 times. 

The empirical distribution of the absolute difference in minutes means over 1000 permutations is plotted below; our observed test statistic is indicated by the red line. 

---

We calculated the p-value to be approximately 0.109, which is greater than the significance value we chose, 0.05. Therefore, we **fail to reject the null hypothesis** that the missingness of `rating` column does not depend on minutes. Since we can conclude that the missingness of `rating` **does not** depend on minutes, we can say that the missingness of `rating` is **MCAR** as it doesn't depend on another column or the values themselves.