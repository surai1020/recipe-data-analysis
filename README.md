# An Investigation on Recipe Tags and User Interest

## Introduction
**Questions to Explore** : What is the relationship between recipe tags and popularity of a recipe? 

> I believe this question is an important one to answer, especially for both consumers and producers of food recipes. Using this question, 
users can interpret what recipes are more likely to produce better results, and such, not waste their times using dissatisfying recipes from food.com. Additionally, this will be helpful for those who produce or upload the recipes such as food.com--allowing these netowrks to put user-satisfaction first and deciding what recipes to push or create for better results. I'm choosing to explore overall user interest as opposed to just number of reviews or average ratings for reasons that will be explained as I delve further into the analysis.

**Introduction to the Dataset**:
This large dataset (234,429  observations, 29 features) includes information of a variety of recipes from food.com. Relevant columns include:

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
Upon looking at the original dataset, the 'tags' column, contrary to its appearance, was actually a series of strings and not lists. This would make accessing specific tags quite difficult. I noticed this pattern in the 'ingredients' and 'nutrition' column as well, so I converted each of these to lists.

I then focused on getting what information I needed for the pre-provided data and removing data I did not need. This includes:
1. Getting the 'sugar_pdv' and 'calories' for each recipe from the 'ingredients' column and adding them to the data frame
2. Creating a 'user_interest' column that represents popularity of a recipe by combining the number reviews and average rating of a recipe via multiplication

This is what the clean dataframe looks like:
| name                                 |   recipe_id |   minutes |   contributor_id | tags                                                                                                                                      |   num_tags |   n_steps | ingredients                                                                                                                                                                    |   n_ingredients |   calories |   sugar_pdv |          user_id |   rating |   avg_rating |   user_interest |   is_vegan |   is_dessert |   is_high_protein |   is_healthy |   is_easy |   is_low-in-something |   is_main-dish |   is_60-minutes-or-less |   is_3-steps-or-less |   is_30-minutes-or-less |   is_meat |   is_vegetables |   is_15-minutes-or-less |   is_taste-mood |
|:-------------------------------------|------------:|----------:|-----------------:|:------------------------------------------------------------------------------------------------------------------------------------------|-----------:|----------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-----------:|------------:|-----------------:|---------:|-------------:|----------------:|-----------:|-------------:|------------------:|-------------:|----------:|----------------------:|---------------:|------------------------:|---------------------:|------------------------:|----------:|----------------:|------------------------:|----------------:|
| 1 brownies in the world    best ever |      333281 |        40 |           985201 | ['60-minutes-or-less', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies'] |          9 |        10 | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] |               9 |      138.4 |          50 | 386585           |        4 |            4 |               4 |          0 |            1 |                 0 |            0 |         0 |                     0 |              0 |                       1 |                    0 |                       0 |         0 |               0 |                       0 |               0 |
| 1 in canada chocolate chip cookies   |      453467 |        45 |          1848091 | ['60-minutes-or-less', 'north-american', 'for-large-groups', 'canadian', 'british-columbian']                                             |          5 |        12 | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    |              11 |      595.1 |         211 | 424680           |        5 |            5 |               5 |          0 |            0 |                 0 |            0 |         0 |                     0 |              0 |                       1 |                    0 |                       0 |         0 |               0 |                       0 |               0 |
| 412 broccoli casserole               |      306168 |        40 |            50969 | ['60-minutes-or-less', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                  |          6 |         6 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      194.8 |           6 |  29782           |        5 |            5 |              20 |          0 |            0 |                 0 |            0 |         1 |                     0 |              0 |                       1 |                    0 |                       0 |         0 |               1 |                       0 |               0 |
| 412 broccoli casserole               |      306168 |        40 |            50969 | ['60-minutes-or-less', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                  |          6 |         6 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      194.8 |           6 |      1.19628e+06 |        5 |            5 |              20 |          0 |            0 |                 0 |            0 |         1 |                     0 |              0 |                       1 |                    0 |                       0 |         0 |               1 |                       0 |               0 |
| 412 broccoli casserole               |      306168 |        40 |            50969 | ['60-minutes-or-less', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                  |          6 |         6 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |               9 |      194.8 |           6 | 768828           |        5 |            5 |              20 |          0 |            0 |                 0 |            0 |         1 |                     0 |              0 |                       1 |                    0 |                       0 |         0 |               1 |                       0 |               0 |

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

