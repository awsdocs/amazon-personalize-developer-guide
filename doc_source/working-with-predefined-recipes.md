# Step 1: Choosing a Recipe<a name="working-with-predefined-recipes"></a>

Amazon Personalize provides recipes, based on common use cases, for training models\. A *recipe* is an Amazon Personalize term specifying an appropriate algorithm to train for a given use case\. With recipes, you can create a personalization system without prior machine learning experience\.

Amazon Personalize recipes use the following during training:
+ Predefined attributes of your data
+ Predefined feature transformations
+ Predefined algorithms
+ Initial parameter settings for the algorithms

To optimize your model, you can override many of these parameters when you create a solution\. For more information, see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\.

Choose a specific recipe based on what you want to accomplish and how familiar you are with the recipes\. Each recipe is designed for a specific use case\. When creating a solution, choose the recipe that best fits your needs\.  

## Amazon Personalize Recipes<a name="recipe-categories"></a>

Amazon Personalize provides three types of recipes\. Besides behavioral differences, each type has different requirements for getting recommendations, as shown in the following table\.


| Recipe type | API | Requirements | Recipes | 
| --- | --- | --- | --- | 
| USER\_PERSONALIZATION | [GetRecommendations](API_RS_GetRecommendations.md) |  `userId`: Required `itemId`: Optional `inputList`: NA  |  [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) [Popularity\-Count](native-recipe-popularity.md)  [HRNN Recipe \(Legacy\)](native-recipe-hrnn.md) [HRNN\-Metadata Recipe \(Legacy\)](native-recipe-hrnn-metadata.md) [HRNN\-Coldstart Recipe \(Legacy\)](native-recipe-hrnn-coldstart.md)  | 
| PERSONALIZED\_RANKING | [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) |  `userId`: Required `itemId`: NA `inputList`: list of itemId's  |  [Personalized\-Ranking](native-recipe-search.md)  | 
| RELATED\_ITEMS | [GetRecommendations](API_RS_GetRecommendations.md) |  `userId`: Not used `itemId`: Required `inputList`: NA  |  [SIMS](native-recipe-sims.md)  | 

## Viewing Available Amazon Personalize Recipes<a name="listing-recipes"></a>

To see a list of available recipes:
+ In the Amazon Personalize console, choose a dataset group\. From the navigation pane, choose **Solutions and recipes**, and choose the **Recipes** tab\. 
+ With the AWS SDK for Python \(Boto3\), call the [ListRecipes](API_ListRecipes.md) API\. 
+ With the AWS CLI, use the following command\.

  ```
  aws personalize list-recipes
  ```

To get information about a recipe using the SDK for Python \(Boto3\), call the [DescribeRecipe](API_DescribeRecipe.md) API\. To get information about a recipe using the AWS CLI, use the following command\.

```
aws personalize describe-recipe --recipe-arn recipe_arn
```