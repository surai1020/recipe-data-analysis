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
|  | description | Description of the recipe |
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

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>recipe_id</th>
      <th>minutes</th>
      <th>contributor_id</th>
      <th>tags</th>
      <th>num_tags</th>
      <th>n_steps</th>
      <th>n_ingredients</th>
      <th>calories</th>
      <th>sugar_pdv</th>
      <th>user_id</th>
      <th>rating</th>
      <th>avg_rating</th>
      <th>user_interest</th>
      <th>description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1 brownies in the world    best ever</td>
      <td>333281.0</td>
      <td>40</td>
      <td>985201</td>
      <td>[60-minutes-or-less, for-large-groups, desserts, lunch, snacks, cookies-and-brownies, chocolate, bar-cookies, brownies]</td>
      <td>9</td>
      <td>10</td>
      <td>9</td>
      <td>138.4</td>
      <td>50.0</td>
      <td>386585.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1 in canada chocolate chip cookies</td>
      <td>453467.0</td>
      <td>45</td>
      <td>1848091</td>
      <td>[60-minutes-or-less, north-american, for-large-groups, canadian, british-columbian]</td>
      <td>5</td>
      <td>12</td>
      <td>11</td>
      <td>595.1</td>
      <td>211.0</td>
      <td>424680.0</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>5.0</td>
      <td>this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.</td>
    </tr>
  </tbody>
</table>

It's important to address why I'll be using user interest and not other metrics.
<iframe
  src="assets/dist_of_given_ratings.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
Here, we see that average ratings are heavily skewed left. This means that average ratings are mostly 4-5 stars. 
Additionally, the average reviews are heavily skewed *right*. What does this mean?
<iframe
  src="assets/dist_of_given_reviews.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
If a lot of recipes have high ratings, but only 1 or 2 reviews, then using only 'average rating' as a metric to observe tagging influence would not make much sense, as average rating alone does not show that a recipe is regarded well by many people. Because of this, I've decided to analyze 'user interest' as a whole. This factors both the number of reviews given and the average ratings of a recipe.

Now, let's look at the distribution of tags
<iframe
  src="assets/dist_of_num_tags.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
Most recipes have around 7-10 tags, though the distribution number of tags is significantly more spread out than that of review count or average ratings. Increasing the number of tags may increase the outreach the recipe has. However, it seems most recipe authors did not subscribe to this, as the number of tags tapers off towards the end. This is arguably a positive indicator, as inflated tag counts suggest an attempt to attract more clicks.

<iframe
  src="assets/avg_interest_by_tag.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
Up until about ~29 tags, we see the average user interest slowly increase up to about 21.5 with minor fluctuations. However, the fluctuations begin to grow around 30-tags, with a peak at 37 tags and a user interest of 36.4. When paired with the previous figure, the rapid fluctuations could reflect a small amount of recipes that overtagged and recieved mixed user-reception. 

I also wanted to examine the relationship of tags with eachother. What percentage of each tag is *also* tagged with another? For this, I created an aggregate table, though the heatmap may make the table easier to understand. 

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>is_dessert</th>
      <th>is_high_protein</th>
      <th>is_healthy</th>
      <th>is_easy</th>
      <th>is_low-in-something</th>
      <th>is_main-dish</th>
      <th>is_60-minutes-or-less</th>
      <th>is_3-steps-or-less</th>
      <th>is_30-minutes-or-less</th>
      <th>is_meat</th>
      <th>is_vegetables</th>
      <th>is_15-minutes-or-less</th>
      <th>is_taste-mood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>is_dessert</th>
      <td>1.00</td>
      <td>0.00</td>
      <td>0.13</td>
      <td>0.52</td>
      <td>0.31</td>
      <td>0.00</td>
      <td>0.34</td>
      <td>0.17</td>
      <td>0.22</td>
      <td>0.00</td>
      <td>0.02</td>
      <td>0.18</td>
      <td>0.24</td>
    </tr>
    <tr>
      <th>is_high_protein</th>
      <td>0.01</td>
      <td>1.00</td>
      <td>0.16</td>
      <td>0.67</td>
      <td>0.83</td>
      <td>0.70</td>
      <td>0.25</td>
      <td>0.24</td>
      <td>0.32</td>
      <td>0.66</td>
      <td>0.02</td>
      <td>0.14</td>
      <td>0.17</td>
    </tr>
    <tr>
      <th>is_healthy</th>
      <td>0.11</td>
      <td>0.03</td>
      <td>1.00</td>
      <td>0.66</td>
      <td>0.88</td>
      <td>0.21</td>
      <td>0.26</td>
      <td>0.32</td>
      <td>0.25</td>
      <td>0.12</td>
      <td>0.23</td>
      <td>0.27</td>
      <td>0.18</td>
    </tr>
    <tr>
      <th>is_easy</th>
      <td>0.13</td>
      <td>0.04</td>
      <td>0.19</td>
      <td>1.00</td>
      <td>0.41</td>
      <td>0.27</td>
      <td>0.25</td>
      <td>0.44</td>
      <td>0.26</td>
      <td>0.22</td>
      <td>0.24</td>
      <td>0.31</td>
      <td>0.23</td>
    </tr>
    <tr>
      <th>is_low-in-something</th>
      <td>0.12</td>
      <td>0.08</td>
      <td>0.38</td>
      <td>0.64</td>
      <td>1.00</td>
      <td>0.29</td>
      <td>0.27</td>
      <td>0.29</td>
      <td>0.25</td>
      <td>0.23</td>
      <td>0.27</td>
      <td>0.25</td>
      <td>0.19</td>
    </tr>
    <tr>
      <th>is_main-dish</th>
      <td>0.00</td>
      <td>0.08</td>
      <td>0.11</td>
      <td>0.51</td>
      <td>0.36</td>
      <td>1.00</td>
      <td>0.36</td>
      <td>0.17</td>
      <td>0.26</td>
      <td>0.58</td>
      <td>0.22</td>
      <td>0.05</td>
      <td>0.26</td>
    </tr>
    <tr>
      <th>is_60-minutes-or-less</th>
      <td>0.17</td>
      <td>0.03</td>
      <td>0.14</td>
      <td>0.48</td>
      <td>0.34</td>
      <td>0.37</td>
      <td>1.00</td>
      <td>0.15</td>
      <td>0.00</td>
      <td>0.26</td>
      <td>0.26</td>
      <td>0.00</td>
      <td>0.21</td>
    </tr>
    <tr>
      <th>is_3-steps-or-less</th>
      <td>0.10</td>
      <td>0.03</td>
      <td>0.21</td>
      <td>1.00</td>
      <td>0.43</td>
      <td>0.20</td>
      <td>0.18</td>
      <td>1.00</td>
      <td>0.23</td>
      <td>0.17</td>
      <td>0.22</td>
      <td>0.43</td>
      <td>0.16</td>
    </tr>
    <tr>
      <th>is_30-minutes-or-less</th>
      <td>0.13</td>
      <td>0.05</td>
      <td>0.17</td>
      <td>0.60</td>
      <td>0.37</td>
      <td>0.31</td>
      <td>0.00</td>
      <td>0.24</td>
      <td>1.00</td>
      <td>0.22</td>
      <td>0.25</td>
      <td>0.00</td>
      <td>0.18</td>
    </tr>
    <tr>
      <th>is_meat</th>
      <td>0.00</td>
      <td>0.10</td>
      <td>0.09</td>
      <td>0.53</td>
      <td>0.36</td>
      <td>0.76</td>
      <td>0.33</td>
      <td>0.18</td>
      <td>0.23</td>
      <td>1.00</td>
      <td>0.19</td>
      <td>0.06</td>
      <td>0.27</td>
    </tr>
    <tr>
      <th>is_vegetables</th>
      <td>0.01</td>
      <td>0.00</td>
      <td>0.16</td>
      <td>0.61</td>
      <td>0.45</td>
      <td>0.30</td>
      <td>0.35</td>
      <td>0.25</td>
      <td>0.27</td>
      <td>0.20</td>
      <td>1.00</td>
      <td>0.15</td>
      <td>0.26</td>
    </tr>
    <tr>
      <th>is_15-minutes-or-less</th>
      <td>0.13</td>
      <td>0.02</td>
      <td>0.21</td>
      <td>0.85</td>
      <td>0.45</td>
      <td>0.07</td>
      <td>0.00</td>
      <td>0.53</td>
      <td>0.00</td>
      <td>0.06</td>
      <td>0.17</td>
      <td>1.00</td>
      <td>0.17</td>
    </tr>
    <tr>
      <th>is_taste-mood</th>
      <td>0.18</td>
      <td>0.03</td>
      <td>0.15</td>
      <td>0.66</td>
      <td>0.35</td>
      <td>0.40</td>
      <td>0.32</td>
      <td>0.20</td>
      <td>0.22</td>
      <td>0.32</td>
      <td>0.29</td>
      <td>0.18</td>
      <td>1.00</td>
    </tr>
  </tbody>
</table>

<iframe
  src="assets/shared_tag_hmp.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

The 'easy' tag seems to be paired with other tags significantly more than others. In specific, the tag is often paired with '15-minutes-or-less' or '3-steps-or-less.' This makes sense, as both of these tags could be interchangable with the 'easy' tag due to its displayed simplicity of the recipe. Many of the shared tags are not surprising, as they are tags you would normally expect to be paired together, such as 'meat' and 'main-dish' and so on. Associations between tags are important, as they can explain why certain tags may both effect user interest. 

## Assessment of Missingness
# NMAR
A column that I believe is NMAR is the 'description' column. For a column to be NMAR, the probability of a missing value is dependent on the column itself. In this case, it is likely that a person did not want to include a description because it would be either too long or too short. For example, if the person writing out the description writes out a description to discover it is too short and casual, they may not want to include it because they feel there is no point. On the contrast, a description that is too long might seem to intense, leading the author to not want to include it. This would mean that the content of the column itself is why the value is missing, making it NMAR. 

# MAR vs MCAR
The assess missingness, I created two different functions to test on different kinds of data. One function computes the Total-Variation Distance as a test statistic, which is optimal for the categorical columns such as the one-hot encoded tags. The other function computes difference in means, which is good for more continuous variables. Both of these functions test whether or not the missingness of the 'rating' column is MAR. 

I decided to test the 'is_healthy' column and the 'calories' column. The 'is_healthy' column utilizes the test statistic of TVD. 
Null hypothesis: The missing data in 'rating' is not dependent on 'is_healthy'
Alternate hypothesis: The missing data in 'rating' is dependent on 'is_healthy', making the 'rating' column MAR.
Significance Level: 0.05
The resulting **p-value is 0.941.** This means that we fail to reject the null hypothesis.
<iframe
  src="assets/missingness_fig1.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

Next, I used the 'calories' column, using difference in means as the test statistic. 
Null hypothesis: The missing data in 'rating' is not dependent on 'calories'
Alternate hypothesis: The missing data in 'rating' is dependent on 'calories', making the 'rating' column MAR.
Significance Level: 0.05
The resulting **p-value is 0.0.** This means that we reject the null hypothesis. The missinginess of the 'rating' column is dependent on the 'calories' column, making 'rating' MAR. 
<iframe
  src="assets/missingness_fig2.html"
  width="600"
  height="400"
  frameborder="0"
></iframe>

## Hypothesis Testing
As the number of tags increases, it would not be an unreasonable assumption that more people will click on the recipe, and as a result, review it. I decided to explore this via a permuatation test. Is there a positive correlation between the number of tags and the user interest of a recipe?

- Null hypothesis: There is no correlation between the number of tags and the user interest
- Alternate hypothesis: There is a positive correlation between the number of tags and the user interest.
- Test statistic: r (taken using np.corrcoef(num_tags, user_interest))
- Significance Level: 0.05

The choice of Pearson's Correlation Coeffecient as the test statistic was fitting because I am trying to measure the association between the number of tags and user interest, but more specifically, the direction of the association--if any. That being said, the observed statistic is *very* low, with an r-score of about 0.05. 

After conducting the hypothesis test, it is reported that the **p-value is 0.0** (or, a very small number close to 0). With this, we reject the null hypothesis, as there is evidence that suggests a positive correlation between num_tags and user_interest. This is surprising, as the observed statistic is 0.05. The dataset is quite large, however, which can explain the results of the hypothesis test. Because of the size of the data, statistical power, or the ability to reject a false null, is increased. Smaller deviations from the null are seen as statistically significant, which could explain the results of this test.

Additionally, I wanted to observe the impact of specific tags and user interest. I created a function that takes a column name as an input and performs a hypothesis test similar to the following:

- Null hypothesis: The distribution of user interest is not different in 'dessert' recipes versus non 'dessert' recipes
- Alternative hypothesis:  The distribution of user interest is not different in 'dessert' recipes versus non 'dessert' recipes
- Test statistic: abs(mean_user_interest('is_high_protein' == 1) - mean_user_interest('is_high_protein' == 0)
- Significance Level: 0.05

As for the 'is_high_protein' column, the results are as follows:
1. Observed difference in mean: 2.678
2. P-val: 0.0
3. Average difference in mean of permutations: 0.367

In this case, we can more confidently affirm that there is a significant difference in the actual difference in mean user interest of high protein recipes versus the permutated difference in mean user interest. Because the **p-value is 0.0**, we reject the null hypothesis; The distribution of user interest is different in is_high_protein recipes versus non is_high_protein recipes.

Using this function, I performed the same hypothesis test on the top-10 most used tags. Below are the results of each:

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>obs_mean</th>
      <th>p_val</th>
      <th>perm_mean</th>
      <th>verdict</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>easy</th>
      <td>1.51</td>
      <td>0.00</td>
      <td>0.13</td>
      <td>we reject the null</td>
    </tr>
    <tr>
      <th>low-in-something</th>
      <td>1.11</td>
      <td>0.00</td>
      <td>0.13</td>
      <td>we reject the null</td>
    </tr>
    <tr>
      <th>main-dish</th>
      <td>0.22</td>
      <td>0.20</td>
      <td>0.14</td>
      <td>we fail to reject null</td>
    </tr>
    <tr>
      <th>60-minutes-or-less</th>
      <td>0.23</td>
      <td>0.18</td>
      <td>0.14</td>
      <td>we fail to reject null</td>
    </tr>
    <tr>
      <th>3-steps-or-less</th>
      <td>0.77</td>
      <td>0.00</td>
      <td>0.14</td>
      <td>we reject the null</td>
    </tr>
    <tr>
      <th>30-minutes-or-less</th>
      <td>0.30</td>
      <td>0.08</td>
      <td>0.15</td>
      <td>we fail to reject null</td>
    </tr>
    <tr>
      <th>meat</th>
      <td>0.65</td>
      <td>0.00</td>
      <td>0.15</td>
      <td>we reject the null</td>
    </tr>
    <tr>
      <th>vegetables</th>
      <td>0.90</td>
      <td>0.00</td>
      <td>0.15</td>
      <td>we reject the null</td>
    </tr>
    <tr>
      <th>15-minutes-or-less</th>
      <td>0.83</td>
      <td>0.00</td>
      <td>0.16</td>
      <td>we reject the null</td>
    </tr>
    <tr>
      <th>taste-mood</th>
      <td>2.06</td>
      <td>0.00</td>
      <td>0.17</td>
      <td>we reject the null</td>
    </tr>
  </tbody>
</table>



## Framing a Prediction Problem
I will be predicting average rating via Regression. Though I used user interest as a metric for the other aspects of the analysis, I believe it will be more worthwile to predict a metric that will be displayed on food.com itself. Additionally, I believe average rating is a good response variable to measure because it can be both useful to consumers and producers of recipes to decide if the recipe they would like to use has a good overall perception and if they should use or produce such a recipe.

A few potential features for the baseline include: number of tags, or specific tags (one-hot-encoded). Because I'm analyzing tags specifically, I would like to predict average rating using as many tag related features as I can, though I do plan on including other kinds of features as the model develops. Additionally, we would know tags before the recipe gets posted, as the author considers these things while forming the recipe. Things such as number of reviews, number of ratings, or user interest cannot be used because these are all things we only find out after the recipe has been posted. 

The metric I chose to test the model is RMSE. I chose this because it preserves the original units, making it easy to interpret. MSE does not preserve the original units, and MAE does not penalize larger errors like RMSE does. Additionally, I did not choose R^2 because it also doesn't use the same units and is harder to interpret whether the model is "good" or not.

## Baseline Model
As stated previously, my baseline model is using only tags as predictors. I chose these tags by looking at the results of the hypothesis test on the top 10 tags. I decided to go with three: 'is_meat', 'is_15-minutes-or-less', and 'is_easy'. I believe each of these could predict the average rating. All three of these, though categorical, are already one-hot-encoded due to previous operations. Because of this, no other transformers were necessary, though I included a Standard Scaler so that the weights would be reflective of the features' importance. 

The model reported a **RMSE of 0.640812**. Though this seemse like a small number, it is important to note that average rating is a continous scale from 1 to 5, so 0.640812 could be off by a star-class. Because of this, though our model is not bad, I will have to consider adding other features other than a few popular tags. 

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

Because the data has missing values, I made sure to impute by the median. I chose not to impute by mean because some features have wide spreads (ie. calories), and a median is better representative of a standard value for that col in this case.


The modeling algorithm I chose was a Polynomial Linear Regression model. The first hyperparameter I tested out was the different polynomial degrees: 1, 2, 3, 4, 5, 6. I also tested out the interaction--whether the model includes multiplying the same feature by itself or not. The method I used to choose the best one was GridSearchCV. Through its search, it discovered that the best possible polynomial degree was 2 and that the interaction is best kept False, meaning both interaction terms and power terms are good. The **RMSE of this new model is 0.640635**. Though it is not significantly smaller than the baseline model, it is still an improvement, as the features aren't contained to just tags, and explain unconsidered influences to average rating. 

## Fairness Analysis
I conducted two Fairness Analysis tests. The first is column specfic, whereas the second takes the whole model into account. 
- X : 'is_easy' == 1
- Y : 'is_easy' == 0
- Signifigance Level : 0.05
- Test Statistic : RMSE of X - RMSE of Y
- Null :  The model is fair.
- Alternative : The model is unfair. The RMSE for non-'is_easy' recipes and 'is_easy' recipes is different. 

I conducted the Fairness Analysis via Permutation test, and the reported **p-value is 0.563**. We fail to reject the null hypothesis, and the model is fair when it comes to the two 'is_easy' groups. In other words, the model performs siilarly between easy recipes and non-easy recipes. 

The second one, though not an analysis of two specific groups within the model, I felt was important to include, as it could explain the behavior of the model. I decided to test if the model predicted higher average rated recipes or lower average rated recipes. 

- X : Low average rated recipe (obs_avg_rating < median of all average ratings)
- Y : High average rated recipe (obs_avg_rating >= median of all average ratings)
- Signifigance Level : 0.05
- Test Statistic : RMSE of X - RMSE of Y
- Null :  The model is fair and unbiased when it comes to the average rated recipes
- Alternative : The model is biased and performs better on higher rated recipes

This test was also conducted via Permutation test. The reported **p-value is 0.0**. We reject the null hypothesis. The model is biased and performs better on higher rated recipes. The visualizations from the Exploratory Analysis section support this, as average rating is heavily skewed toward higher ratings. This likely means most of the training data consisted of higher ratings. 
