# Investigation on the relationship between amount of sugar in recipe and rating

## Overview
This is a data science project for DSC80 at UCSD focusing on exploring the relationship between the rating of a recipe and the proportion of sugar contributing to the total calories of the given recipe.

## Introduction
Food is a critical component of our daily lives, and cooking is a hobby that brings joy and satisfaction for many. While there are many of us who have a sweet tooth and love sugary food, the health risks associated with them, such as diabetes, are not negligible.  According to the National Diabetes Statistics Report from Centers for Disease Control and Prevention, 11.6% of the US population have diabetes and over 38.0% of people over 18 years old have prediabetes. With this information in our mind, we want to investigate the relationship between rating and the amount of sugar present in the recipe. **We wonder if people would rate a sugary recipe lower due to the health concerns relating to it**. To do so, we are analyzing two dataset consisting of recipes and ratings posted since 2008 on [food.com](https://www.food.com/). The original purpose of the datasets is for the recommender system research paper, [Generating Personalized Recipes from Historical User Preferences](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf) by Majumder et al.

The first dataset, `recipe`, contains 83782 rows, indicating 83782 unique recipes, with 10 columns recording the following information:

| Column | Description |
| :-------- | :------- |
| `'name'` | Recipe name |
| `'id'` | Recipe ID |
| `'minutes'` | Minutes to prepare recipe |
| `'contributor_id'` | User ID who submitted this recipe |
| `'submitted'` | Date recipe was submitted |
| `'tags'` | Food.com tags for recipe |
| `'nutrition'` | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'` | Number of steps in recipe |
| `'steps'` | Text for recipe steps, in order |
| `'description'` | User-provided description |
| `'ingredients'` | Text for recipe ingredients |
| `'n_ingredients'` | Number of ingredients in recipe |

The second dataset, `interactions`, contains 731927 rows and each row contains a review from the user on a specific recipe. The columns it includes are:

| Column | Description |
| :-------- | :------- |
| `'user_id'` | User ID |
| `'recipe_id'` | Recipe ID |
| `'date'` | Date of interaction |
| `'rating'` | Rating given |
| `'review'` | Review text |

**Given the datasets, we are investigating whether people rate sugary recipes and the non-sugary recipes on the same scale.** To facilitate the investigation of our question, we separated the values in the `'nutrition'` columns into the corresponding columns,`'calories (#)'`, `'total fat (PDV)'`, `'sugar (PDV)'`, etc. PDV, or percent daily value shows how much a nutrient in a serving of food contributes to a total daily diet. Moreover, we calculated the proportion of sugar in terms of calories out of the total calories of a given recipe and stored the information in a new column, `'prop_sugar'`. because sugary in here will be referring to the recipes with value `'prop_sugar'` higher than the average `'prop_sugar'`. The relevant columns to answer our question are `'calories(#)'`, `'sugar (PDV)'`, `'prop_sugar'`, described above, and `'rating'`, which is the rating that user gave on a recipe.

By seeking an answer to our question, we would have an insight on people’s preference on sugary recipes, which could help contributors on Food.com revise and improve their recipes to align with the public’s interests. In addition, the new pieces of information could lead to future work on diving deeper into how much awareness people have on the negative health effects of sweets.

## Data Cleaning and Exploratory Data Analysis
To make our analysis of the dataset more efficient and convenient, we conducted the following data cleaning steps.

1. Left merge the recipes and interactions datasets on id and recipe_id.
    - This step helps match the unique recipes with their rating and review. 

1. Check data types of all the columns.
    - This step helps us evaluate what data cleaning steps are appropriate for the dataset and if we need to conduct data type conversion.
    -   | Column | Description |
        | :-------- | :------- |
        | `'name'` | object |
        | `'id'` | int64 |
        | `'minutes'` | int64 |
        | `'contributor_id'` | int64 |
        | `'submitted'` | object |
        | `'tags'` | object |
        | `'nutrition'` | object |
        | `'n_steps'` | int64 |
        | `'steps'` | object |
        | `'description'` | object |
        | `'ingredients'` | object |
        | `'n_ingredients'` | int64 |
        | `'user_id'` | float64 |
        | `'recipe_id'` | float64 |
        | `'date'` | object |
        | `'rating'` | float64 |
        | `'review'` | object |

1. Fill all ratings of 0 with np.nan. 
    - Rating is generally on a scale from 1 to 5, 1 meaning the lowest rating while 5 means the highest rating. With that being said, a rating of 0 indicates missing values in rating. Thus, to avoid bias in the ratings, we filled the value 0 with np.nan.

1. Add column average_rating containing average rating per recipe.
    - Since a recipe can have numerous ratings from different users, we take an average of all the ratings to get a more comprehensive understanding of the rating of a given recipe.

1. Split values in the nutrition column to individual columns of floats.
    - Even though the values in the nutrition column look like a list, they are actually objects, which act like strings.  Given by the description of the columns of the recipe dataset, we know what each individual values inside the brackets mean. To split up the values, we applied a lambda function then converted the columns to floats. It will allow us to conduct numerical calculations on the columns.

1. Convert submitted and date to datetime.
    - These two columns are both stored as objects initially, so we converted them into datetime to allow us conduct analysis on trends over time if needed.

1. Add `'is_dessert'` to the dataframe 
    - `'is_dessert'` is a boolean column checking  if the tags of recipes contain 'dessert' since desserts typically have a higher amount of sugar. This step separates the recipes into two groups, ones that are dessert and ones that are not. This provides us another way to compare ratings of recipes with more sugar and ones with less sugar.

1. Add prop_sugar to the dataframe 
    - prop_sugar is the proportion of sugar of the total calories in a recipe. To calculate this, we use the values in the sugar (PDV) column to divide by 100% to get it in the decimal form. Then, we multiply by 25 to convert the values to grams of sugar since 25 grams of sugar is the 100% daily value (PDV). We got this value of 25 grams from experimenting on food.com with different amounts of sugar in a recipe. The experimentation allows us to understand the nutrition formula used on the website for recipes. Lastly, we multiply by 4 since there are 4 calories in 1 gram of sugar. After all these conversions, we end up with the number of calories of sugar, so we can divide by the total amount of calories in the recipe to get the proportion of sugar of the total calories. This data cleaning step is critical to allow us to make parallel comparisons on the amount of sugar in a recipe without concerns of extremely large values since all the values will be between 0 and 1.

#### Result

| Column | Description |
| :-------- | :------- |
| `'name'` | object |
| `'id'` | int64 |
| `'minutes'` | int64 |
| `'contributor_id'` | int64 |
| `'submitted'` | datetime64[ns] |
| `'tags'` | object |
| `'nutrition'` | object |
| `'n_steps'` | int64 |
| `'steps'` | object |
| `'description'` | object |
| `'ingredients'` | object |
| `'n_ingredients'` | int64 |
| `'user_id'` | float64 |
| `'recipe_id'` | float64 |
| `'date'` | datetime64[ns] |
| `'rating'` | float64 |
| `'review'` | object |
| `'average rating'` | object |
| `'calories (#)'` | float64 |
| `'total fat (PDV)'` | float64 |
| `sugar (PDV)'` | float64 |
| `'sodium (PDV)'` | float64 |
| `'protein (PDV)'` | float64 |
| `'saturated fat (PDV)'` | float64 |
| `'carbohydrates (PDV)'` | float64 |
| `'is_dessert'` | bool |
| `'prop_sugar'` | float64 |



### Univariate Analysis
For this analysis, we examined the distribution of the proportion of sugar in a recipe. As the plot below shows, the distribution skewed to the right, indicating that most of the recipes on food.com have a low proportion of sugar. There is also a decreasing trend, indicating that as the proportion of sugar in recipes gets higher, there are less of those recipes on food.com. 

<iframe
  src="assets/univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis
For this analysis, we examined the distribution of the rating of the recipe conditioned between the sugary recipes and non-sugary recipes. The graph below shows that recipes with rating of 3, 4 and 5 are more likely to be non-sugary recipes while the recipes with rating of 1 and 2 are more likely to be sugary recipes. We would dive deeper to see if the difference in these proportions are significant in later sections.

<iframe
  src="assets/bivariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates
For this section, we investigated the relationship between the cooking time in minutes and proportion of sugar of the recipes.  After grouping the data, we created a data visualization, shown below, to understand it better. Interestingly, the graph shows that as the cooking time increases the proportion of sugar in a recipe fluctuates more and more. Also, the shapes of the line for mean and median looks very similar, especially for the recipes with shorter cooking time. 


<iframe
  src="assets/interesting_agg.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Assessment of Missingness

Three columns, `'date'`, `'rating'`, and `'review'`,  in the merged dataset have a significant amount of missing values, so we decided to assess the missingness on the dataframe.

### NMAR Analysis
We believe that the missingness of the `'review'` column is NMAR, because if people feel indifferent about the recipe, they are less likely to leave a review for it since they would feel like they have nothing significant to talk about. People usually will leave a review only if they have stronger emotions towards the recipe. Their emotions would motivate them to go onto the page, click multiple buttons to leave, and take some time out of their day to write a review. For example, people who enjoyed the recipe would be willing to do all the work to leave a good review for the recipe.

### Missingness Dependency
We moved on to examine the missingness of `'rating'` in the merged DataFrame by testing the dependency of its missingness. We are investigating whether the missiness in the `'rating'` column depends on the column `'prop_sugar'`, which is the proportion of sugar out of the total calories, or the column `'n_steps'`, which is the number of steps of the recipe.

> Proportion of Sugar and Rating
**Null hypothesis:** The missingness of ratings does not depend on the proportion of sugar in the recipe.

**Alternate hypothesis:** The missingness of ratings does depend on the proportion of sugar in the recipe.

**Test statistic:** The absolute difference of mean in the proportion of sugar of the distribution of the group without missing ratings and the distribution of the group without missing ratings. 

**Significance level:** 0.05

<iframe
  src="assets/distr_rating_sugar.html
  width="800"
  height="600"
  frameborder="0"
></iframe>

We ran a permutation test by shuffling the missingness of rating for 1000 times to collect 1000 simulating mean differences in the two distributions as described in the test statistic.


<iframe
  src="assets/empirical_diff_sugar.html
  width="800"
  height="600"
  frameborder="0"
></iframe>

The **observed statistic** of **0.0063** is indicated by the red vertical line on the graph. Since the **p_value** that we found **(0.0)** is < 0.05 which is the significance level that we set, we **reject the null hypothesis**. The missingness of `'rating'` does depend on the `'prop_sugar'`, which is proportion of sugar in the recipe.

> Minutes and Rating
**Null hypothesis:** The missingness of ratings does not depend on the cooking time of the recipe in minutes.

**Alternate hypothesis:** The missingness of ratings does depend on the cooking time of the recipe in minutes.

**Test statistic:** The absolute difference of mean in cooking time of the recipe in minutes of the distribution of the group without missing ratings and the distribution of the group without missing ratings. 

**Significance level:** 0.05
