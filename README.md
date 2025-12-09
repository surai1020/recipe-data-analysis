# An Investigation on Recipe Tags and User Interest
An exploratory analysis on recipes and user preferences from Food.com 

**Questions to Explore** : What is the relationship between recipe tags and popularity of a recipe? 

> I believe this question is an important one to answer, especially for both consumers and producers of food recipes. Using this question, users can interpret what recipes are more likely to produce better results, and such, not waste their times using dissatisfying recipes from food.com. Additionally, this will be helpful for those who produce or upload the recipes such as food.com--allowing these netowrks to put user-satisfaction first and deciding what recipes to push or create for better results. I'm choosing to explore overall user interest as opposed to just number of reviews or average ratings for reasons that will be explained as I delve further into the analysis.

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
