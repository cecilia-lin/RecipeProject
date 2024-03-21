# Investigation on the relationship between amount of sugar in recipe and rating

## Overview
This is a data science project for DSC80 at UCSD focusing on exploring the relationship between the rating of a recipe and the proportion of sugar contributing to the total calories of the given recipe.

## Introduction
Food is a critical component of our daily lives, and cooking is a hobby that brings joy and satisfaction for many. While there are many of us who have a sweet tooth and love sugary food, the health risks associated with them, such as diabetes, are not negligible.  According to the National Diabetes Statistics Report from Centers for Disease Control and Prevention, 11.6% of the US population have diabetes and over 38.0% of people over 18 years old have prediabetes. With this information in our mind, we want to investigate the relationship between rating and the amount of sugar present in the recipe. **We wonder if people would rate a sugary recipe lower due to the health concerns relating to it**. To do so, we are analyzing two dataset consisting of recipes and ratings posted since 2008 on ([food.com](https://www.food.com/)). The original purpose of the datasets is for the recommender system research paper, ([Generating Personalized Recipes from Historical User Preferences](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf)) by Majumder et al.

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
    -This step helps us evaluate what data cleaning steps are appropriate for the dataset and if we need to conduct data type conversion.
    -   | Column | Description |
        | :-------- | :------- |
        | `'name'` | Recipe name |
        | `'id'` | Recipe ID |
        | `'minutes'` | Minutes to prepare recipe |
        | `'contributor_id'` | User ID who submitted this recipe |
        | `'submitted'` | Date recipe was submitted |
        | `'tags'` | Food.com tags for recipe |
        | `'nutrition'` | |
        | `'n_steps'` | Number of steps in recipe |
        | `'steps'` | Text for recipe steps, in order |
        | `'description'` | User-provided description |
