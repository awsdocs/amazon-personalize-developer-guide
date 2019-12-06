# Using Predefined Recipes<a name="working-with-predefined-recipes"></a>

 Amazon Personalize provides predefined recipes, based on common use cases, for training models\. A *recipe* is a machine learning algorithm or algorithm variant that you use with settings, or hyperparameters, and a dataset group to train an Amazon Personalize model\. With recipes, you can create a personalization system without prior machine learning experience\.

The predefined recipes use the following during training:
+ Predefined attributes of your data
+ Predefined feature transformations 
+ Predefined algorithms 
+ Initial parameter settings for the algorithms

To optimize your model, you can override many of these parameters when you create a solution\. For more information, see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\.

Amazon Personalize can automatically choose the most appropriate hierarchical recurrent neural network \(HRNN\) recipe based on its analysis of the input data\. This option is called AutoML\. To perform AutoML, set the `performAutoML` parameter to `true` when you call the [CreateSolution](API_CreateSolution.md) API\. Alternatively, you can choose a specific recipe based on what you want to accomplish and how famliar you are with the recipes\. Each recipe is designed for a specific use case\. When creating a solution, choose the recipe that best fits your needs\.

To see a list of available recipes:
+ In the Amazon Personalize console, choose a dataset group\. From the navigation pane, choose **Solutions and recipes**, and choose the **Recipes** tab\. 
+ With the AWS SDK for Python \(Boto 3\), call the [ListRecipes](API_ListRecipes.md) API\. 
+ With the AWS CLI, use the following command\.

  ```
  aws personalize list-recipes
  ```

To get information about a recipe using the SDK for Python \(Boto 3\), call the [DescribeRecipe](API_DescribeRecipe.md) API\. To get information about a recipe using the AWS CLI, use the following command\.

```
aws personalize describe-recipe --recipe-arn recipe_arn
```

Amazon Personalize provides three types of recipes\. Besides behavioral differences, each type has different requirements for getting recommendations, as shown in the following table\.


| Recipe type | API | Requirements | 
| --- | --- | --- | 
| USER\_PERSONALIZATION | [GetRecommendations](API_RS_GetRecommendations.md) |  `userId`: Required `itemId`: Optional `inputList`: NA  | 
| PERSONALIZED\_RANKING | [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) |  `userId`: Required `itemId`: NA `inputList`: list of itemId's  | 
| RELATED\_ITEMS | [GetRecommendations](API_RS_GetRecommendations.md) |  `userId`: Not used `itemId`: Required `inputList`: NA  | 

## Predefined Recipes<a name="predefined-recipes"></a>

Amazon Personalize provides the following predefined recipes\. They are listed by recipe type\.

**USER\_PERSONALIZATION Recipes**  
USER\_PERSONALIZATION recipes predicts the items that a user will interact with\.    
[HRNN](native-recipe-hrnn.md)^  
HRNN is a hierarchical recurrent neural network, which can model the user\-item interactions across a given timeframe\. Use the HRNN recipe when user behavior changes over time, which is referred to the evolving intent problem\.  
To train a model, HRNN uses the Interactions dataset from a dataset group\. A dataset group is a set of related datasets, which can include the Users, Items, and Interactions datasets\.  
[HRNN\-Metadata](native-recipe-hrnn-metadata.md)^\*  
HRNN\-Metadata is the HRNN recipe with additional features derived from metadata found in the Interactions, Users, and Items datasets\. For example, rating, movie genre, or release year\. The training data must include metadata in at least one of the datasets\. HRNN\-Metadata performs better than non\-metadata models when high\-quality metadata is available\. Training with this recipe can involve longer training times\.  
To train a model, HRNN\-Metadata uses the Users, Items, and Interactions datasets from a dataset group\. A dataset group is a set of related datasets, which can include the Users, Items, and Interactions datasets\.  
[HRNN\-Coldstart](native-recipe-hrnn-coldstart.md)^\*  
HRNN\-Coldstart is similar to the HRNN\-Metadata recipe, but also includes personalized exploration of new items\. Use the HRNN\-Coldstart recipe when you frequently add new items to a catalog and want the items to immediately be included in recommendations\.  
To train a model, HRNN\-Coldstart uses the Users, Items, and Interactions datasets from a dataset group\. A dataset group is a set of related datasets, which can include the Users, Items, and Interactions datasets\. The dataset group that supplies the training data must include an Items dataset\.   
[Popularity\-Count](native-recipe-popularity.md)  
The Popularity\-Count recipe calculates the popularity of items based on a count of events against that item in the Interactions dataset\. Use the Popularity\-Count recipe to create a baseline to compare results with other USER\-PERSONALIZATION recipes\.  
To train a model, the Popularity\-Count recipe uses the Interactions dataset from a dataset group\. A dataset group is a set of related datasets, which can include the Users, Items, and Interactions datasets\.

**PERSONALIZED\_RANKING Recipe**  
The Personalized\-Ranking recipe personalizes results\.    
[Personalized\-Ranking](native-recipe-search.md)  
The Personalized\-Ranking recipe is a hierarchical recurrent neural network \(HRNN\) recipe that also can filter and rerank results\. Personalized\-Ranking provides a list of the best recommendations\. Use the Personalized\-Ranking recipe when you’re personalizing the results for your users, such as personalized reranking of search results or curated lists\.  
To train a model, the Personalized\-Ranking recipe uses the Interactions dataset from a dataset group\. A dataset group is a set of related datasets, which can include the Users, Items, and Interactions datasets\.

**RELATED\_ITEMS Recipes**  
The RELATED\_ITEMS recipe, SIMS, returns items similar to a given item\.    
**[SIMS](native-recipe-sims.md)**  
The item\-to\-item similarities \(SIMS\) recipe generates items similar to a given item based on the co\-occurrence of the item in user history in the user\-item interaction dataset\. If sufficient user behavior data for an item isn't available, or if the specified item ID isn't found, the recipe returns popular items as recommendations\. Use the SIMS recipe to improve item discoverability and in detail pages\. Training is faster with the SIMS recipe compared to other recipes\.  
To train a model, the SIMS recipe uses the Interactions dataset from a dataset group\. A dataset group is a set of related datasets, which can include the Users, Items, and Interactions datasets\.

^ Amazon Personalize considers these recipes when performing AutoML\.

\* Metadata models: These recipes train on both interaction data and metadata\. For more information, see [Datasets and Schemas](how-it-works-dataset-schema.md)\.
