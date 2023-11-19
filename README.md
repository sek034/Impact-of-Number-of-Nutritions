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
## Data Cleaning and EDA

The data that are using two different datasets: recipe, interaction

> Checking Data Types

Let's look into their columns and datatypes.

This is recipe data.


| Column name     | Type          |
|-----------------|---------------|
| `name`          | object        |
| `id`            | int64         |
| `minutes`       | int64         |
| `contributor_id`| int64         |
| `submitted`     | object        |
| `tags`          | object        |
| `nutrition`     | object        |
| `n_steps`       | int64         |
| `steps`         | object        |
| `description`   | object        |
| `ingredients`   | object        |
| `n_ingredients` | int64         |

and this is interaction(rating) data

| Column name     | Type          |
|-----------------|---------------|
| `user_id`       | int64         |
| `recipe_id`     | int64         |
| `date`          | object        |
| `rating`        | int64         |
| `review`        | object        |

> Merging Two Datasets

For convenience purpose, The first step of cleaning is going to be merging the two data sets.
Two datasets can be merged based on 'ID' from recipe and 'recipe_id' from interaction as they both share same recipe id. The merge will be conducted based on recipe data.

> Replace Missing Value to np.NaN

There are some missing value in merged data (ex. rating) indicated as 0. The missing value has to be converted to np.NaN instead of 0 for future hypothesis and permutation testing as the testing would recognize the missing value if it is in np.NaN.

> Adding Column for Calories Only

As calories for each recipe is included in nutrition column, we isolated the calories and make separate column for it.

> Rating to Average Rating

Another important information that we would need is rating mean per recipe. We made another columns for rating mean per recipe. As Average Rating still contains np.NaN, we will be able to do hypothesis and permutation testing based on the missingness for later.

> Categorize Number of Ingredients

As the number of ingredients are dispersed and it would be easier to conduct permutation testing if it's categorized, we made another column for "low_number". If the n_ingredients is less than its median, it will say True and False for otherwise.

> Log Transformation for Calories

Calories data are very skewed to right. In this case, it would be hard for us to get the accurate estimation for hypothesis and permutation testing. Therefore, we made another column for log(calories - minimum of calories) for each recipe.

> Clean Result

| Column name       | Type          |
|-----------------  |---------------|
| `name`            | object        |
| `id`              | int64         |
| `minutes`         | float64       |
| `contributor_id`  | int64         |
| `submitted`       | object        |
| `tags`            | object        |
| `nutrition`       | object        |
| `n_steps`         | int64         |
| `steps`           | object        |
| `description`     | object        |
| `ingredients`     | object        |
| `n_ingredients`   | int64         |
| `user_id`         | float64       |
| `recipe_id`       | float64       |
| `date`            | object        |
| `rating`          | float64       |
| `review`          | object        |
| `rating_mean`     | float64       |
| `calories`        | float64       |
| `low_number`      | bool          |
| `log_calories`    | float64       |
| `rate_missingness`| bool          |


> Example

This is the example of 
I took out only columns that are important 

|     id | name                                 | n_ingredients | calories        | log_calories  | rating_missingness|
|-------:|:-------------------------------------|--------------:|:----------------|--------------:|:------------------|
| 333281 | 1 brownies in the world best ever    |        9      |            138.4|       4.937347| False             |
| 453467 | 1 in canada chocolate chip cookies   |        11     |            595.1|       6.390408| False             |
| 306168 | 412 broccoli casserole               |        9      |            194.8|       5.277094| False             |
| 306168 | 412 broccoli casserole               |        9      |            194.8|       5.277094| False             |
| 306168 | 412 broccoli casserole               |        9      |            194.8|       5.277094| False             |
| 306168 | 412 broccoli casserole               |        9      |            194.8|       5.277094| False             |


---

### Exploratory Data Analysis

### Univariate Analysis

In the univariate analysis, we would analyze the distribution of number of ingredients and the distribution of calories

<iframe src='graph/distribution_n_ingredients.html' width=600 height=600 frameBorder=0></iframe>

This is the distribution of number of ingredients. We can tell that the distribution is right skewed.


<iframe src='graph/distribution_calories.html' width=600 height=600 frameBorder=0></iframe>

This is the distribution of calories. As there were outliers that make the graph to hard to interpret, I limited the calories to 20k so that people can see the distribution of majority. We can tell that the distribution of calories is also right skewed data.


### Bivariate Analysis

In Bivariate Analysis, we would analyze the correlation between calories and number of ingredients.

<iframe src='graph/scatter.html' width=600 height=600 frameBorder=0></iframe>

This is the scatter plots that explains the distribution between caloreis and number of ingredients. As dots are congreated near 10 ingredients and 10k calories. We can tell the distribution is right skewed but not as much as the scatter plots from univariate. Also, we can also see there are some outliers with extremely high calories.

<iframe src='graph/box_plot.html' width=600 height=600 frameBorder=0></iframe>

This is the box plots that explains the dispersion of calories between low number of ingredients and high number of ingredients (low number = lower than the median of number of ingredients and vice versa). As the box plot of high number of ingredients lower in calories than the lower number of ingredients, we can tell that calories tend to me lower in large number of ingredients. Furthermore, we can also tell that dispersion of two categories are not that different.


### Interesting Aggregates

|   rating |     False |      True |
|---------:|----------:|----------:|
|        1 | 0.0125714 | 0.0136221 |
|        2 | 0.0112957 | 0.0102612 |
|        3 | 0.0337719 | 0.031544  |
|        4 | 0.173422  | 0.16647   |
|        5 | 0.768939  | 0.778103  |

I made the pivot table with proportion of recipes per each rating from 1 to 10. We can tell that low number of ingredients and higher number of ingredients have similar proportion for every rating. We can also tell that the total number of 5 rating is the highest number in total.

This graphs also explains the description.

<iframe src='graph/barr.html' width=600 height=600 frameBorder=0></iframe>

---
## Assessment of Missingness
This section of the study will examine missingness in certain columns on the combined dataset. 

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

<iframe src='graph/fig_cal.html' width=600 height=600 frameBorder=0></iframe>


Then, we performed a permutaion test by first calculating the observed test statistic - which is the absolute mean difference in `calories` between the group of recipes with ratings and the group without ratings. We got 69.007 as the observed test statistic. 

We generated 1000 simulation results for each absolute difference by performing permutation testing to shuffle the missingness of rating 1000 times. 

The empirical distribution of the absolute difference in calorie means over 1000 permutations is plotted below; our observed test statistic is indicated by the red line. 

<iframe src='graph/fig_c.html' width=600 height=600 frameBorder=0></iframe>


We calculated the p-value to be approximately 0.0, which is less than the significance value we chose, 0.05. Therefore, we **reject the null hypothesis** that the missingness of `rating` column does not depend on calories. Since we can conclude that the missingness of `rating` **does** depend on calories, we can say that the missingness of `rating` is **MAR** as it depends another column which is `calories`.


---


#### 2. Rating and Minutes

**Null hypothesis**: The missingness of `rating` column does not depend on minutes.

**Alnernative hypothesis**: The missingness of `rating` column does depend on minutes.

**Test Statistics**: The absolute mean difference between two distributions of rating and minutes.

Similarly, we constructed a plot that indicates distributions of minutes with and without the presence of rating below.

<iframe src='graph/fig_minutes.html' width=600 height=600 frameBorder=0></iframe>


We performed a permutaion test by first calculating the observed test statistic - which is the absolute mean difference in `minutes` between the group of recipes with ratings and the group without ratings. We got 51.451 as the observed test statistic. 

We generated 1000 simulation results for each absolute difference by performing permutation testing to shuffle the missingness of rating 1000 times. 

The empirical distribution of the absolute difference in minutes means over 1000 permutations is plotted below; our observed test statistic is indicated by the red line. 

<iframe src='graph/fig_m.html' width=600 height=600 frameBorder=0></iframe>


We calculated the p-value to be approximately 0.109, which is greater than the significance value we chose, 0.05. Therefore, we **fail to reject the null hypothesis** that the missingness of `rating` column does not depend on minutes. Since we can conclude that the missingness of `rating` **does not** depend on minutes, we can say that the missingness of `rating` is **MCAR** as it doesn't depend on another column or the values themselves.

---
### Hypothesis Testing

The question for Hypothesis testing is: are calories(numbers) and number of ingredients (categories --> lower or higher) correlated? (is there any relationship between them)

We are going to use permutation testing in order to find the relationship between them. For significance value, we are going to use 0.05

For test statistic, we are going to use absolute mean difference

> Setting Up the Testing

As we already mentioned, we already made the columns for categorizing the number of ingredients by categorizing the number of ingredients that is lower than median as True and False for otherwise. 

Therefore, dataframe only with "id", "low_number", and calories looks like this:

         |      id|    low_number|   calories |
    |---:|-------:|:-------------|-----------:|
    |  0 | 333281 | False        |      138.4 |
    |  1 | 453467 | False        |      595.1 |
    |  2 | 306168 | False        |      194.8 |
    |  3 | 306168 | False        |      194.8 |
    |  4 | 306168 | False        |      194.8 |


The reason why we chose absolute mean difference is because we have one categorical data with numerical data to compare.
For comparison, we chose two-sided data as it is a binary testing. H0 = no relationshiop, H1 = There is a relationship.

Also we use log transformation for calories as calories is a highly skewed data with large outliers. It could cause a confusion and mis calculation for permutation testing.
That is why we used log transformation in order to decrease the effect of skewness.

> Permutation
We ran permutation test for 10000 times to get the accurate data. 
The bar graph shows the result of the permutation testing.

<iframe src='graph/fig_permu.html' width=600 height=600 frameBorder=0></iframe>

> Conclusion

The p-value for the testing is 0.00099, which is way smaller than significant level of 0.05. Therfore, we reject the null hypothesis.

This result might be caused by due to the reason with existence of heteroscedasticity, which means that the dispersion of error is not constant. Also, there might be other factor such as kinds of ingredients that they use. For example, french fries only take about 3 different ingredients, but it is considered as a very high calories food.
