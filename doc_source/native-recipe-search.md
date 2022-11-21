# Personalized\-Ranking recipe<a name="native-recipe-search"></a>

The Personalized\-Ranking recipe generates personalized rankings of items\. A *personalized ranking* is a list of recommended items that are re\-ranked for a specific user\. This is useful if you have a collection of ordered items, such as search results, promotions, or curated lists, and you want to provide a personalized re\-ranking for each of your users\. 

To train a model, the Personalized\-Ranking recipe uses the data in your Interactions dataset, and if you created them, the Items dataset and Users dataset in your dataset group \(these datasets are optional\)\. With Personalized\-Ranking, your Items dataset can include [Unstructured text metadata](items-datasets.md#text-data) and your Interactions dataset can include [Contextual metadata](interactions-datasets.md#interactions-contextual-metadata)\. To get a personalized ranking, use the [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) API\. 

 After you create a solution version, make sure you keep your solution version and data up to date\. With Personalized\-Ranking, you must manually create a new solution version \(retrain the model\) to reflect updates to your catalog and update the model with your user’s most recent behavior\. Then you must update any campaign using the solution version\. For more information, see [Maintaining recommendation relevance](maintaining-relevance.md)\.  

**Note**  
 If you provide items without interactions data for ranking, Amazon Personalize will return these items without a recommendation score in the GetPersonalizedRanking API response\. 

This recipe has the following properties:
+  **Name** – `aws-personalized-ranking`
+  **Recipe Amazon Resource Name \(ARN\)** – `arn:aws:personalize:::recipe/aws-personalized-ranking`
+  **Algorithm ARN** – `arn:aws:personalize:::algorithm/aws-personalized-ranking`
+  **Feature transformation ARN** – `arn:aws:personalize:::feature-transformation/JSON-percentile-filtering`
+  **Recipe type** – `PERSONALIZED_RANKING`

## Hyperparameters<a name="personalized-ranking-hyperparameters"></a>

The following table describes the hyperparameters for the Personalize\-Ranking recipe\. A *hyperparameter* is an algorithm parameter that you can adjust to improve model performance\. Algorithm hyperparameters control how the model performs\. Featurization hyperparameters control how to filter the data to use in training\. The process of choosing the best value for a hyperparameter is called hyperparameter optimization \(HPO\)\. For more information, see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\. 

The table also provides the following information for each hyperparameter:
+ **Range**: \[lower bound, upper bound\]
+ **Value type**: Integer, Continuous \(float\), Categorical \(Boolean, list, string\)
+ **HPO tunable**: Can the parameter participate in hyperparameter optimization \(HPO\)?


| Name | Description | 
| --- | --- | 
| Algorithm hyperparameters | 
| hidden\_dimension |  The number of hidden variables used in the model\. *Hidden variables* recreate users' purchase history and item statistics to generate ranking scores\. Specify a greater number of hidden dimensions when your Interactions dataset includes more complicated patterns\. Using more hidden dimensions requires a larger dataset and more time to process\. To decide on the optimal value, use HPO\. To use HPO, set `performHPO` to `true` when you call [CreateSolution](API_CreateSolution.md) and [CreateSolutionVersion](API_CreateSolutionVersion.md) operations\. Default value: 149 Range: \[32, 256\] Value type: Integer HPO tunable: Yes  | 
| bptt |  Determines whether to use the back\-propagation through time technique\. *Back\-propagation through time* is a technique that updates weights in recurrent neural network\-based algorithms\. Use `bptt` for long\-term credits to connect delayed rewards to early events\. For example, a delayed reward can be a purchase made after several clicks\. An early event can be an initial click\. Even within the same event types, such as a click, it’s a good idea to consider long\-term effects and maximize the total rewards\. To consider long\-term effects, use larger `bptt` values\. Using a larger `bptt` value requires larger datasets and more time to process\. Default value: 32 Range: \[2, 32\] Value type: Integer HPO tunable: Yes  | 
| recency\_mask |  Determines whether the model should consider the latest popularity trends in the Interactions dataset\. Latest popularity trends might include sudden changes in the underlying patterns of interaction events\. To train a model that places more weight on recent events, set `recency_mask` to `true`\. To train a model that equally weighs all past interactions, set `recency_mask` to `false`\. To get good recommendations using an equal weight, you might need a larger training dataset\. Default value: `True` Range: `True` or `False` Value type: Boolean HPO tunable: Yes  | 
| Featurization hyperparameters | 
| min\_user\_history\_length\_percentile |  The minimum percentile of user history lengths to include in model training\. *History length* is the total amount of data about a user\. Use `min_user_history_length_percentile` to exclude a percentage of users with short history lengths\. Users with a short history often show patterns based on item popularity instead of the user's personal needs or wants\. Removing them can train models with more focus on underlying patterns in your data\. Choose an appropriate value after you review user history lengths, using a histogram or similar tool\. We recommend setting a value that retains the majority of users, but removes the edge cases\.  For example, setting `min__user_history_length_percentile to 0.05` and `max_user_history_length_percentile to 0.95` includes all users except those with history lengths at the bottom or top 5%\. Default value: 0\.0 Range: \[0\.0, 1\.0\] Value type: Float HPO tunable: No  | 
| max\_user\_history\_length\_percentile |  The maximum percentile of user history lengths to include in model training\. *History length* is the total amount of data about a user\. Use `max_user_history_length_percentile` to exclude a percentage of users with long history lengths because data for these users tend to contain noise\. For example, a robot might have a long list of automated interactions\. Removing these users limits noise in training\. Choose an appropriate value after you review user history lengths using a histogram or similar tool\. We recommend setting a value that retains the majority of users but removes the edge cases\. For example, setting `min__user_history_length_percentile to 0.05` and `max_user_history_length_percentile to 0.95` includes all users except those with history lengths at the bottom or top 5%\. Default value: 0\.99 Range: \[0\.0, 1\.0\] Value type: Float HPO tunable: No  | 

## Personalized\-Ranking sample notebook<a name="personalized-ranking-sample-notebook"></a>

 For a sample Jupyter notebook that shows how to use the Personalized\-Ranking recipe, see [Personalize Ranking Example](https://github.com/aws-samples/amazon-personalize-samples/blob/master/next_steps/core_use_cases/personalized_ranking/personalize_ranking_example.ipynb)\. 