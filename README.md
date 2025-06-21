# Macro-Friendly Recipes!

by: Emy Klepacki

## Introduction

As someone who enjoys going to the gym, I’ve found that the hardest part isn’t the workouts—it’s maintaining proper nutrition. For college students balancing classes, workouts, and healthy eating, staying on track can be especially challenging, no matter their fitness goals. This project aims to take the stress out of nutrition by finding easy, macro-friendly recipes that support gains and goals.

For me, I aim to find recipes that are low-calorie and high in protein, as I want to maximize protein throughout my day. For those who aspire to lose/maintain weight or "cut", this could be ideal for finding recipes that keep you full and in a deficit/maintenance. For those who aspire to gain weight or "bulk", I would find recipes that are both high in protein and calories. Of course, there are other nutritional values that are also important to keep in mind, such as carbs, fats, etc., but this project will be mainly focusing on calories and protein. Therefore, to find the types of recipes that match nutrtional goals, the relevant columns for RAW_recipes.csv include `name`, `tags`, and `nutrition`. In addition, I would like to explore which types of these recipes are optimal for college students that are low on time. Thus, another relevant column is `minutes`. Lastly, I would like to see user ratings based on these recipes to see how they compare to other recipes, so I will also look into the 'rating' column.

After brainstorming some questions, this led me to my overall question: **Which types of recipes are the most macro-friendly and time-efficient?**

The dataset RAW_recipes.csv includes 83782 rows, and the dataset RAW_interactions.csv includes 731,927 rows. The columns I will focus on are: `name` (recipe name), `tags` (Food.com tags for recipe), `nutrition` (Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”), `rating` (Rating given), and `minutes` (Minutes to prepare recipe).



## Data Cleaning and Exploratory Data Analysis
### Cleaning
To start, I began by merging the datasets on `recipe_id` (or `id` for RAW_recipes.csv). I then extracted `protein` and `calories` from the nutrition column as their own values, and I dropped rows that did not include these values. From the ratings column, I replaced all ratings of 0 with NaN to represent missing values. I also only included columns that were necessary to my dataframe, so I got rid of all other columns (such as `date`, `submitted`, etc.). In addition, I got rid of any duplicate recipes, given that many recipes showed up numerous times depending on how many reviews they received. Instead, I took the mean of all their ratings and included that value in the `rating` column. To fill missing values in the minutes column, I used probabilistic imputation by randomly sampling from the observed minutes values with NumPy. This preserves the original distribution and avoids bias from using a fixed mean or median. I additionally filtered out recipes that took over 120 minutes, given that I'm looking for recipes that are time-efficient and wanted to remove outliers.

Here is the cleaned dataframe:

 | name                                |   minutes |   calories |   protein | tags                                                                                                                                                                                                                                                              |   rating |
|:------------------------------------|----------:|-----------:|----------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------:|
| 0 carb   0 cal gummy worms          |        45 |      384.7 |       159 | ['60-minutes-or-less', 'time-to-make', 'course', 'preparation', 'healthy', '5-ingredients-or-less', 'desserts', 'lunch', 'snacks', 'easy', 'low-fat', 'dietary', 'low-cholesterol', 'low-saturated-fat', 'high-protein', 'high-in-something', 'low-in-something'] |     4.75 |
| 0 point soup   ww                   |        55 |       26.8 |         2 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'soups-stews', 'vegetables', 'easy', 'vegetarian', 'stove-top', 'dietary', 'equipment', '3-steps-or-less']                                                                     |     4.78 |
| 007  martini                        |         5 |      146.5 |         0 | ['celebrity', '15-minutes-or-less', 'time-to-make', 'course', 'cuisine', 'preparation', 'occasion', 'for-1-or-2', '5-ingredients-or-less', 'beverages', 'easy', 'european', 'english', 'cocktails', 'novelty', 'number-of-servings']                              |     5    |
| 1  2  3  swiss meringue buttercream |        20 |      409.8 |         4 | ['30-minutes-or-less', 'time-to-make', 'course', 'preparation', 'low-protein', '5-ingredients-or-less', 'desserts', 'easy', 'cakes', 'dietary', 'low-sodium', 'cake-fillings-and-frostings', 'low-in-something']                                                  |   nan    |
| 1 2 3 4 bars                        |        75 |      217.6 |         5 | ['weeknight', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'occasion', 'for-large-groups', 'desserts', 'fruit', 'kid-friendly', 'cookies-and-brownies', 'bar-cookies', 'dietary', 'number-of-servings', '4-hours-or-less']                         |     4    |



### Univariate Analysis
To better understand the data, I looked at the distributions of calories, protein, and prep time. Most recipes have fewer than 300 calories, with a median of 294, which shows that the dataset leans toward lower-calorie meals. Protein is a bit more spread out, but the median is around 16g, meaning most recipes aren't super high in protein. When it comes to prep time, most recipes take about 32 minutes or less, making them quick and easy to make. I filtered out recipes with more than 2000 calories or 200g of protein to keep the distribution more realistic, since very few (if any) recipes go above those numbers. I also added dashed lines in each chart to show where the median falls and make trends easier to spot. TODO

 <iframe
 src="assets/calories_histogram.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
  <iframe
 src="assets/protein_histogram.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>
   <iframe
 src="assets/minutes_histogram.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

### Bivariate Anaylysis
To get a better idea of which types of recipes offer the best macros, I looked at two groups: high-protein, low-calorie and high-protein, high-calorie. I split the data using medians, so the groups were based on the overall distribution. For the high-protein, low-calorie recipes, the median protein was 30.0g and median calories were 234.8. On the other hand, the high-protein, high-calorie group had a median of 52.0g of protein and 516.6 calories. Both types of meals are high in protein, but the high-calorie ones definitely offer more. These are useful depending on your goals nutrition-wise. Additionally, high-protein, high-calorie recipes tend to take slightly longer as well, with an average of about 43.8 minutes in comparison to 36.0 minutes for the low-calorie group.

   <iframe
 src="assets/lowcal.html"
 width="1000"
 height="700"
 frameborder="0"
 ></iframe>
<iframe
 src="assets/highcal.html"
 width="1000"
 height="700"
 frameborder="0"
 ></iframe>

 

### Interesting Aggregates
I grouped recipes by prep time ranges to explore how nutrition and user ratings change with cooking time. Recipes that take longer generally have more protein and calories, as we can see a slight increase in both columns as time increases. In addition, ratings remain consistently high across all time bins, indicating that users highly rate both quick and more involved recipes alike. Thus, ratings is not the best column to predict other values.

Here is the dataframe:

| time (minutes)   |   protein |   calories |   rating |
|:-----------------|----------:|-----------:|---------:|
| 0–15             |   17.2952 |    313.555 |  4.67092 |
| 16–30            |   31.7128 |    375.745 |  4.62359 |
| 31–45            |   33.7589 |    416.863 |  4.60538 |
| 46–60            |   36.2782 |    489.552 |  4.608   |
| 61–90            |   38.3323 |    542.416 |  4.627   |

## Framing a Prediction Problem
One column that I plan on trying to predict is minutes, as I want to predict whether these macro optimal meals are feasible for people with limited time in the kitchen. This is a regression problem given minutes is a numeric and continuous variable, and the difference between values is meaningful. So, I will be predicting prep time ('minutes') based on features like calories and protein. This helps me reach the goal of finding macro-friendly recipes that are time-efficient.

I will be using Mean Squared Error (MSE) to evaluate the accuracy of my predictions. Since I’m working on a regression problem with a continuous target (minutes), MSE is appropriate because it penalizes larger errors more heavily, helping the model focus on minimizing big prediction mistakes. This allows for a more precise estimate of how far off the predicted prep times are from the actual values.

## Baseline Model

For the baseline model, I used linear regression to predict a recipe’s prep time (`minutes`) based on two numeric features: `calories` and `protein`. Since both features are already numerical and on a similar scale, I didn’t apply any additional preprocessing like scaling or encoding. The baseline regression model produced a Mean Squared Error (MSE) of 4941.87, which corresponds to a Root Mean Squared Error (RMSE) of approximately 70.3 minutes. This means that, on average, the model's predictions are off by about 70.3 minutes. Thus, this gives me a useful baseline to compare against as I improve the model using additional features.


## Final Model

To improve my baseline model, I built a final model using a Random Forest Regressor and engineered 5 new quantitative features from the recipe’s nutritional data: `total fat`, `sugar`, `carbohydrates`, `sodium`, and `PDV`. These were added to the original features (`calories` and `protein`) to better capture nutritional complexity and its potential impact on prep time. I applied appropriate transformations using a pipeline and used GridSearchCV for initial tuning. While the final model (MSE about 4790) only slightly outperformed the baseline (MSE about 4941), this small improvement suggests that nutrition may play a limited role in predicting cooking time, and that other factors like recipe steps, ingredients, or tags might be more influential in future models.

<iframe
 src="assets/scatter_plot.html"
 width="1000"
 height="700"
 frameborder="0"
 ></iframe>
