# An Investigation on Recipe Tags and User Interest

## Introduction
**Questions to Explore** : What is the relationship between recipe tags and popularity of a recipe? 

> I believe this question is an important one to answer, especially for both consumers and producers of food recipes. Using this question, 
users can interpret what recipes are more likely to produce better results, and such, not waste their times using dissatisfying recipes from food.com. Additionally, this will be helpful for those who produce or upload the recipes such as food.com--allowing these netowrks to put user-satisfaction first and deciding what recipes to push or create for better results. I'm choosing to explore overall user interest as opposed to just number of reviews or average ratings for reasons that will be explained as I delve further into the analysis.

**Introduction to the Dataset**:
This large dataset (234,429  observations, 24 features) includes information of a variety of recipes from food.com. Relevant columns include:

| Category | Column | Description |
|----------|--------|-------------|
| **Recipe information** | name | Name of the recipe |
|  | recipe_id | Unique id of the recipe |
|  | minutes | Minutes taken to make recipe |
|  | contributor_id | Unique id of person who uploaded the recipe |
|  | num_tags | Number of tags in a recipe |
|  | nutrition | Nutritional values for the recipe |
|  | n_steps | Number of steps in the recipe |
|  | calories | Total calorie count for the recipe |
|  | sugar_pdv | Total sugar in PDV for the recipe |
| **Tag information** | tags | Various tags applied for specific recipe |
|  | num_tags | Number of tags in a recipe |
|  | is_vegan | Bool: recipe tagged with 'vegan' |
|  | is_dessert | Bool: recipe tagged with 'dessert' |
|  | is_high_protein | Bool: recipe tagged with 'high_protein' |
|  | is_healthy | Bool: recipe tagged with 'easy' |
|  | is_low-in-something | Bool: recipe tagged with 'low-in-something' |
|  | is_main-dish | Bool: recipe tagged with 'main-dish' |
|  | is_60-minutes-or-less | Bool: recipe tagged with '60-minutes-or-less' |
|  | is_3-steps-or-less | Bool: recipe tagged with '30-minutes-or-less' |
|  | is_meat | Bool: recipe tagged with 'meat' |
|  | is_vegetables | Bool: recipe tagged with 'vegetables' |
|  | is_15-minutes-or-less | Bool: recipe tagged with '15-minutes-or-less' |
|  | is_taste-mood | Bool: recipe tagged with 'taste-mood' |
| **Rating information** | user_id | Unique user id of reviewer |
|  | rating | User-provided rating |
|  | avg_rating | Average rating of all given ratings |
|  | user_interest | Number of reviews × average rating; represents recipe popularity |


## Data Cleaning Steps and Exploratory Analysis
Upon looking at the original dataset, the 'tags' column, contrary to its appearance, was actually a series of strings and not lists. This would make accessing specific tags quite difficult. I noticed this pattern in the 'nutrition' column as well, so I converted both of these to lists.

I then focused on getting what information I needed for the pre-provided data and removing data I did not need. This includes:
1. Filling in 'rating' entries that contain 0 with np.nan
2. Getting the 'sugar_pdv' and 'calories' for each recipe from the 'ingredients' column and adding them to the data frame
3. Creating a 'user_interest' column that represents popularity of a recipe by combining the number reviews and average rating of a recipe via multiplication

It is important that we fill in the 'rating' column with np.nan if there is a 0 because food.com rating range from 1 to 5, making a rating of 0 invalid. It is possible that the 0 was a placeholder for the lack of a rating given. 

This is what the clean dataframe looks like:
|    | name                                 |   recipe_id |   minutes |   contributor_id | tags                                                                                                                                      |   num_tags |   n_steps |   n_ingredients |   calories |   sugar_pdv |   user_id |   rating |   avg_rating |   user_interest |
|---:|:-------------------------------------|------------:|----------:|-----------------:|:------------------------------------------------------------------------------------------------------------------------------------------|-----------:|----------:|----------------:|-----------:|------------:|----------:|---------:|-------------:|----------------:|
|  0 | 1 brownies in the world    best ever |      333281 |        40 |           985201 | ['60-minutes-or-less', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies'] |          9 |        10 |               9 |      138.4 |          50 |    386585 |        4 |            4 |               4 |
|  1 | 1 in canada chocolate chip cookies   |      453467 |        45 |          1848091 | ['60-minutes-or-less', 'north-american', 'for-large-groups', 'canadian', 'british-columbian']                                             |          5 |        12 |              11 |      595.1 |         211 |    424680 |        5 |            5 |               5 |

It's important to address why I'll be using user interest and not other metrics.
<iframe
  src="assets/dist_of_given_ratings.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Here, we see that average ratings are heavily skewed left. This means that average ratings are mostly 4-5 stars. 
Additionally, the average reviews are heavily skewed *right*. What does this mean?
<iframe
  src="assets/dist_of_given_reviews.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
If a lot of recipes have high ratings, but only 1 or 2 reviews, then using only 'average rating' as a metric to observe tagging influence would not make much sense, as average rating alone does not show that a recipe is regarded well by many people. Because of this, I've decided to analyze 'user interest' as a whole. This factors both the number of reviews given and the average ratings of a recipe.

Now, let's look at the distribution of tags
<iframe
  src="assets/dist_of_num_tags.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Most recipes have around 7-10 tags, though the distribution number of tags is significantly more spread out than that of review count or average ratings. Increasing the number of tags may increase the outreach the recipe has. However, it seems most recipe authors did not subscribe to this, as the number of tags tapers off towards the end. This is arguably a positive indicator, as inflated tag counts suggest an attempt to attract more clicks.

<iframe
  src="assets/avg_interest_by_tag.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Up until about ~29 tags, we see the average user interest slowly increase up to about 21.5 with minor fluctuations. However, the fluctuations begin to grow around 30-tags, with a peak at 37 tags and a user interest of 36.4. When paired with the previous figure, the rapid fluctuations could reflect a small amount of recipes that overtagged and recieved mixed user-reception. 

I also wanted to examine the relationship of tags with eachother. What percentage of each tag is *also* tagged with another? For this, I created an aggregate table, though the heatmap may make the table easier to understand. 

|    is_vegan |   is_dessert |   is_high_protein |   is_healthy |   is_easy |   is_low-in-something |   is_main-dish |   is_60-minutes-or-less |   is_3-steps-or-less |   is_30-minutes-or-less |     is_meat |   is_vegetables |   is_15-minutes-or-less |   is_taste-mood |
|------------:|-------------:|------------------:|-------------:|----------:|----------------------:|---------------:|------------------------:|---------------------:|------------------------:|------------:|----------------:|------------------------:|----------------:|
| 1           |  0.110793    |        0.00429365 |    0.28919   |  0.703149 |              0.483246 |    0.189342    |             0.280519    |             0.354268 |             0.2279      | 0.000168379 |       0.448055  |             0.313858    |        0.262418 |
| 0.0379897   |  1           |        0.00204959 |    0.125603  |  0.524638 |              0.306891 |    0.000635086 |             0.34286     |             0.165382 |             0.223492    | 0.00118357  |       0.0166566 |             0.179671    |        0.23885  |
| 0.00619759  |  0.00862802  |        1          |    0.158464  |  0.673472 |              0.829505 |    0.696926    |             0.246202    |             0.236724 |             0.323733    | 0.661077    |       0.0218739 |             0.1406      |        0.171953 |
| 0.0886406   |  0.112278    |        0.0336499  |    1         |  0.660043 |              0.875697 |    0.210879    |             0.255135    |             0.321429 |             0.251574    | 0.122781    |       0.225666  |             0.265019    |        0.177565 |
| 0.0612151   |  0.133204    |        0.0406195  |    0.187471  |  1        |              0.41337  |    0.268549    |             0.24563     |             0.438415 |             0.260802    | 0.216459    |       0.236461  |             0.305335    |        0.227013 |
| 0.0650447   |  0.120469    |        0.0773511  |    0.384546  |  0.639104 |              1        |    0.290979    |             0.267069    |             0.294571 |             0.245549    | 0.226127    |       0.272734  |             0.250252    |        0.186454 |
| 0.0313205   |  0.000306381 |        0.079868   |    0.113807  |  0.510264 |              0.357602 |    1           |             0.358898    |             0.168649 |             0.25587     | 0.584951    |       0.222767  |             0.0496059   |        0.259115 |
| 0.0474036   |  0.168971    |        0.0288234  |    0.14066   |  0.476782 |              0.335297 |    0.366638    |             1           |             0.154645 |             0.000825153 | 0.263892    |       0.264746  |             0.000170721 |        0.210158 |
| 0.0703491   |  0.095777    |        0.0325665  |    0.208239  |  1        |              0.434583 |    0.202454    |             0.181724    |             1        |             0.233031    | 0.168584    |       0.219724  |             0.430637    |        0.15536  |
| 0.0459647   |  0.131459    |        0.0452346  |    0.165538  |  0.604197 |              0.367938 |    0.311973    |             0.000984837 |             0.236683 |             1           | 0.216783    |       0.247703  |             0.000356579 |        0.178901 |
| 3.59906e-05 |  0.000737808 |        0.0978945  |    0.0856217 |  0.531456 |              0.359097 |    0.755857    |             0.333795    |             0.181465 |             0.229746    | 1           |       0.191506  |             0.0559115   |        0.272521 |
| 0.0999587   |  0.0108373   |        0.00338079 |    0.16425   |  0.60595  |              0.452049 |    0.30044     |             0.349517    |             0.246854 |             0.273994    | 0.19988     |       1         |             0.153169    |        0.255212 |
| 0.0763966   |  0.127546    |        0.02371    |    0.210459  |  0.853703 |              0.45256  |    0.0729948   |             0.000245912 |             0.52787  |             0.000430346 | 0.0636706   |       0.167118  |             1           |        0.17181  |
| 0.0665542   |  0.176667    |        0.0302131  |    0.146923  |  0.661336 |              0.351326 |    0.397275    |             0.315412    |             0.198424 |             0.224965    | 0.323355    |       0.290131  |             0.179015    |        1        |

<iframe
  src="assets/shared_tag_hmp.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

The 'easy' tag seems to be paired with other tags significantly more than others. In specific, the tag is often paired with '15-minutes-or-less' or '3-steps-or-less.' This makes sense, as both of these tags could be interchangable with the 'easy' tag due to its displayed simplicity of the recipe. Many of the shared tags are not surprising, as they are tags you would normally expect to be paired together, such as 'meat' and 'main-dish' and so on. Associations between tags are important, as they can explain why certain tags may both effect user interest. 

## Assessment of Missingness
The assess missingness, I created two different functions to test on different kinds of data. One function computes the Total-Variation Distance as a test statistic, which is optimal for the categorical columns such as the one-hot encoded tags. The other function computes difference in means, which is good for more continuous variables. Both of these functions test whether or not the missingness of the 'rating' column is MAR. 

I decided to test the 'is_healthy' column and the 'calories' column. The 'is_healthy' column utilizes the test statistic of TVD. 
Null hypothesis: The missing data in 'rating' is not dependent on 'is_healthy'
Alternate hypothesis: The missing data in 'rating' is dependent on 'is_healthy', making the 'rating' column MAR.
Significance Level: 0.05
The resulting p-value is 0.941. This means that we fail to reject the null hypothesis.
<iframe
  src="assets/missingness_fig1.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

Next, I used the 'calories' column, using difference in means as the test statistic. 
Null hypothesis: The missing data in 'rating' is not dependent on 'calories'
Alternate hypothesis: The missing data in 'rating' is dependent on 'calories', making the 'rating' column MAR.
Significance Level: 0.05
The resulting p-value is 0.0. This means that we reject the null hypothesis. The missinginess of the 'rating' column is dependent on the 'calories' column, making 'rating' MAR. 
<iframe
  src="assets/missingness_fig2.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

## Hypothesis Testing
As the number of tags increases, it would not be an unreasonable assumption that more people will click on the recipe, and as a result, review it. I decided to explore this via a permuatation test. Is there a positive correlation between the number of tags and the user interest of a recipe?

Null hypothesis: There is no correlation between the number of tags and the user interest
Alternate hypothesis: There is a positive correlation between the number of tags and the user interest.
Test statistic: r (taken using np.corrcoef(num_tags, user_interest))
Significance Level: 0.05

The choice of Pearson's Correlation Coeffecient as the test statistic was fitting because I am trying to measure the association between the number of tags and user interest, but more specifically, the direction of the association--if any. That being said, the observed statistic is *very* low, with an r-score of about 0.05. 

After conducting the hypothesis test, it is reported that the p-value is 0.0 (or, a very small number close to 0). With this, we reject the null hypothesis, as there is evidence that suggests a positive correlation between num_tags and user_interest. This is surprising, as the observed statistic is 0.05. The dataset is quite large, however, which can explain the results of the hypothesis test. Because of the size of the data, statistical power, or the ability to reject a false null, is increased. Smaller deviations from the null are seen as statistically significant, which could explain the results of this test.

Additionally, I wanted to observe the impact of specific tags and user interest. I created a function that takes a column name as an input and performs a hypothesis test similar to the following:

Null hypothesis: The distribution of user interest is not different in 'dessert' recipes versus non 'dessert' recipes
Alternative hypothesis:  The distribution of user interest is not different in 'dessert' recipes versus non 'dessert' recipes
Test statistic: |mean_user_interest('is_high_protein' == 1) - mean_user_interest('is_high_protein' == 0)|
Significance Level: 0.05

As for the 'is_high_protein' column, the results are as follows:
1. Observed difference in mean: 2.678
2. P-val: 0.0
3. Average difference in mean of permutations: 0.367

In this case, we can more confidently affirm that there is a significant difference in the actual difference in mean user interest of high protein recipes versus the permutated difference in mean user interest. Because the p-value is 0.0, we reject the null hypothesis; The distribution of user interest is different in is_high_protein recipes versus non is_high_protein recipes.

Using this function, I performed the same hypothesis test on the top-10 most used tags. Below are the results of each:
|                    |   obs_mean |   p_val |   perm_mean | verdict                |
|:-------------------|-----------:|--------:|------------:|:-----------------------|
| easy               |   1.508    |   0     |    0.129368 | we reject the null     |
| low-in-something   |   1.10821  |   0     |    0.130319 | we reject the null     |
| main-dish          |   0.223597 |   0.206 |    0.138736 | we fail to reject null |
| 60-minutes-or-less |   0.230104 |   0.185 |    0.137969 | we fail to reject null |
| 3-steps-or-less    |   0.766952 |   0     |    0.137018 | we reject the null     |
| 30-minutes-or-less |   0.299882 |   0.095 |    0.146756 | we fail to reject null |
| meat               |   0.650274 |   0.001 |    0.157218 | we reject the null     |
| vegetables         |   0.898131 |   0     |    0.150573 | we reject the null     |
| 15-minutes-or-less |   0.827498 |   0     |    0.159373 | we reject the null     |
| taste-mood         |   2.0622   |   0     |    0.164944 | we reject the null     |
Selection deleted



## Framing a Prediction Problem
I will be predicting average rating via Regression. Though I used user interest as a metric for the other aspects of the analysis, I believe it will be more worthwile to predict a metric that will be displayed on food.com itself. Additionally, I believe average rating is a good response variable to measure because it can be both useful to consumers and producers of recipes to decide if the recipe they would like to use has a good overall perception and if they should use or produce such a recipe.

A few potential features for the baseline include: number of tags, or specific tags (one-hot-encoded). Because I'm analyzing tags specifically, I would like to predict average rating using as many tag related features as I can, though I do plan on including other kinds of features as the model develops. Additionally, we would know tags before the recipe gets posted, as the author considers these things while forming the recipe. Things such as number of reviews, number of ratings, or user interest cannot be used because these are all things we only find out after the recipe has been posted. 

The metric I chose to test the model is RMSE. I chose this because it preserves the original units, making it easy to interpret. MSE does not preserve the original units, and MAE does not penalize larger errors like RMSE does. Additionally, I did not choose R^2 because it also doesn't use the same units and is harder to interpret whether the model is "good" or not.

## Baseline Model
As stated previously, my baseline model is using only tags as predictors. I chose these tags by looking at the results of the hypothesis test on the top 10 tags. I decided to go with three: 'is_meat', 'is_15-minutes-or-less', and 'is_easy'. I believe each of these could predict the average rating. All three of these, though categorical, are already one-hot-encoded due to previous operations. Because of this, no other transformers were necessary, though I included a Standard Scaler so that the weights would be reflective of the features' importance. 

The model reported a RMSE of 0.640812. Though this seemse like a small number, it is important to note that average rating is a continous scale from 1 to 5, so 0.640812 could be off by a star-class. Because of this, though our model is not bad, I will have to consider adding other features other than a few popular tags. 

## Final Model
The final model I used was a Linear Regression model, using a GridSearch to verify what hyperparameters (number of Polynomial Features in this case) to use. 

Thinking of new features to introduce to the model took awhile, though I finally settled on two manufactured columns, and the three tags I used previously, along with one more tag. These are the features I used:

1. 'is_meat'
2. 'is_easy'
3. 'is_15-minutes-or-less' 
4. 'is_high_protein'
5. 'ingredient_step_complexity' - 'n_ingredients' / 'n_steps
6. 'sugar_ratio' - 'sugar_pdv' / 'calories'

Firstly, 'ingredient_step_complexity' Describes the ratio of ingredients to steps. A smaller value indicates more preparation of singular ingredients (maybe more advanced skills as well). A larger value indicates less focused preparation for a specific ingredient. I believe a smaller value could bring in smaller ratings, as people may not enjoy a more complicated recipe. Sugar ratio represents how sugary a recipe is. A larger value indicates larger proportion of sugar in the recipe. People may want to be more health concious, so I predict that larger amounts will bring in smaller ratings. I thought it was important to bring in these features because they explain more nuanced thought processes behind ratings that may have gone unnoticed. 


The modeling algorithm I chose was a Polynomial Linear Regression model. The first hyperparameter I tested out was the different polynomial degrees: 1, 2, 3, 4, 5, 6. I also tested out the interaction--whether the model includes multiplying the same feature by itself or not. The method I used to choose the best one was GridSearchCV. Through its search, it discovered that the best possible polynomial degree was 2 and that the interaction is best kept False, meaning both interaction terms and power terms are good. The RMSE of this new model is 0.640635. Though it is not significantly smaller than the baseline model, it is still an improvement, as the features aren't contained to just tags, and explain unconsidered influences to average rating. 

## Fairness Analysis
I conducted two Fairness Analysis tests. The first is column specfic, whereas the second takes the whole model into account. 
X - 'is_easy' == 1
Y - 'is_easy' == 0
Signifigance Level = 0.05
Test Statistic - RMSE of X - RMSE of Y
Null -  The model is fair.
Alternative - The model is unfair. The RMSE for non-'is_easy' recipes and 'is_easy' recipes is different. 

I conducted the Fairness Analysis via Permutation test. The reported p-value is 0.563. We fail to reject the null hypothesis, and the model is fair when it comes to the two 'is_easy' groups. 

For the second fairness test:
X - Low average rated recipe (obs_avg_rating < median of all average ratings)
Y - High average rated recipe (obs_avg_rating >= median of all average ratings)
Signifigance Level = 0.05
Test Statistic - RMSE of X - RMSE of Y
Null -  The model is fair and unbiased when it comes to the average rated recipes
Alternative - The model is biased and performs better on higher rated recipes

This test was also conducted via Permutation test. The reported p-value is 0.0. We reject the null hypothesis. The model is biased and performs better on higher rated recipes. The visualizations from the Exploratory Analysis section support this, as average rating is heavily skewed toward higher ratings. This likely means most of the training data consisted of higher ratings. 
