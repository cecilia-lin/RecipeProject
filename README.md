# Investigation on the relationship between amount of sugar in recipe and rating

## Overview
This is a data science project for DSC80 at UCSD focusing on exploring the relationship between the rating of a recipe and the proportion of sugar contributing to the total calories of the given recipe.

## Introduction
Food is a critical component of our daily lives, and cooking is a hobby that brings joy and satisfaction for many. While there are many of us who have a sweet tooth and love sugary food, the health risks associated with them, such as diabetes, are not negligible.  According to the National Diabetes Statistics Report from Centers for Disease Control and Prevention, 11.6% of the US population have diabetes and over 38.0% of people over 18 years old have prediabetes. With this information in our mind, we want to investigate the relationship between rating and the amount of sugar present in the recipe. **We wonder if people would rate a sugary recipe lower due to the health concerns relating to it**. To do so, we are analyzing two dataset consisting of recipes and ratings posted since 2008 on ([food.com](https://www.food.com/)). The original purpose of the datasets is for the recommender system research paper, ([Generating Personalized Recipes from Historical User Preferences](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf)) by Majumder et al.

The first dataset, `recipe`, contains 83782 rows, indicating 83782 unique recipes, with 10 columns recording the following information:

| Column | Description |
| :--------: | :-------: |
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
