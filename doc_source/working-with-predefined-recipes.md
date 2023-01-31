# Step 1: Choosing a recipe<a name="working-with-predefined-recipes"></a>

Amazon Personalize provides recipes, based on common use cases, for training models\. *Recipes* are Amazon Personalize algorithms that are prepared for specific use cases\.

Amazon Personalize recipes use the following during training:
+ Predefined attributes of your data
+ Predefined feature transformations
+ Predefined algorithms
+ Initial parameter settings for the algorithms

To optimize your model, you can override many of these parameters when you create a solution\. For more information, see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\.

Choose a specific recipe based on what you want to accomplish and how familiar you are with the recipes\. Each recipe is designed for a specific use case\. For help determining your use case and choosing a recipe, see [Determining your use case](determining-use-case.md) 

## Amazon Personalize recipes<a name="recipe-categories"></a>

Amazon Personalize provides three types of recipes\. Besides behavioral differences, each type has different requirements for getting recommendations, as shown in the following table\.


| Recipe type | Recipes | API | API requirements | 
| --- | --- | --- | --- | 
| USER\_PERSONALIZATION |  [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md)  [HRNN recipe \(legacy\)](native-recipe-hrnn.md) [HRNN\-Metadata recipe \(legacy\)](native-recipe-hrnn-metadata.md) [HRNN\-Coldstart recipe \(legacy\)](native-recipe-hrnn-coldstart.md)  | [GetRecommendations](API_RS_GetRecommendations.md) |  `userId`: Required `itemId`: Not used `inputList`: NA  | 
| POPULAR\_ITEMS |  [Trending\-Now](native-recipe-trending-now.md) [Popularity\-Count](native-recipe-popularity.md)  | [GetRecommendations](API_RS_GetRecommendations.md) |  `userId`: Required only if you apply a filter that requires it `itemId`: Not used `inputList`: NA  | 
| PERSONALIZED\_RANKING |  [Personalized\-Ranking](native-recipe-search.md)  | [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) |  `userId`: Required `itemId`: NA `inputList`: list of itemId's  | 
| RELATED\_ITEMS |  [Similar\-Items](native-recipe-similar-items.md) [SIMS](native-recipe-sims.md)  | [GetRecommendations](API_RS_GetRecommendations.md) |  `userId`: Required only if you apply a filter that requires it `itemId`: Required `inputList`: NA  | 
| USER\_SEGMENTATION |  [Item\-Affinity](item-affinity-recipe.md) [Item\-Attribute\-Affinity](item-attribute-affinity-recipe.md)  | [CreateBatchSegmentJob](API_CreateBatchSegmentJob.md) |  For batch workflow requirements, see [Creating a batch segment job](creating-batch-seg-job.md)\.  | 

## Viewing available Amazon Personalize recipes<a name="listing-recipes"></a>

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