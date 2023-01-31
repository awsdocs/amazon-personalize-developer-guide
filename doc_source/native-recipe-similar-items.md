# Similar\-Items recipe<a name="native-recipe-similar-items"></a>

**Note**  
 All RELATED\_ITEMS recipes use interactions data\. Choose Similar\-Items if you have also have item metadata and want Amazon Personalize to use it to find similar items\. Or choose the [SIMS recipe](native-recipe-sims.md) if you want to configure more hyperparameters for the model, such as the popularity discount factor\. 

 The Similar\-Items \(aws\-similar\-items\) recipe generates recommendations for items that are similar to an item you specify\. Use Similar\-Items to help customers discover new items in your catalog based on their previous behavior and item metadata\. Recommending similar items can increase user engagement, click\-through rate, and conversion rate for your application\. 

Similar\-Items calculates similarity based on interactions data and any item metadata you provide\. It takes into account the co\-occurrence of the item in user histories in your Interaction dataset, and any item metadata similarities\. For example, with Similar\-Items Amazon Personalize could recommend items customers frequently bought together with a similar style \([Categorical metadata](items-datasets.md#item-categorical-data)\), or movies that different users also watched with a similar description \([Unstructured text metadata](items-datasets.md#text-data)\)\.

With Similar\-Items, you provide an item ID in a [GetRecommendations](API_RS_GetRecommendations.md) operation \(or the Amazon Personalize console\) and Amazon Personalize returns a list of similar items\. Or you can use a batch workflow to get similar items for all of the items in your inventory \(see [Getting batch recommendations and user segments](recommendations-batch.md)\)\. 

 To use Similar\-Items, you must create an Interactions dataset with at least 1000 unique historical and event interactions \(combined\)\. For more accurate predictions, we recommend that you also create an Items dataset and import metadata about items in your catalog\. You can get recommendations for items that are similar to a cold item \(an item with fewer than five interactions\)\. If Amazon Personalize can't find the item ID that you specify in your recommendation request or batch input file, the recipe returns popular items as recommendations\. 

 After you create a solution version, make sure you keep your solution version and data up to date\. With Similar\-Items, you must manually create a new solution version \(retrain the model\) to reflect updates to your catalog and update the model with your user’s most recent behavior\. Then you must update any campaign using the solution version\. For more information, see [Maintaining recommendation relevance](maintaining-relevance.md)\. 

## Properties and hyperparameters<a name="similar-items-hyperparameters"></a>

The Similar\-Items recipe has the following properties:
+  **Name** – `aws-similar-items`
+  **Recipe Amazon Resource Name \(ARN\)** – `arn:aws:personalize:::recipe/aws-similar-items`
+  **Algorithm ARN** – `arn:aws:personalize:::algorithm/aws-similar-items`

For more information, see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\.

The following table describes the hyperparameters for the Similar\-Items recipe\. A *hyperparameter* is an algorithm parameter that you can adjust to improve model performance\. Algorithm hyperparameters control how the model performs\. The process of choosing the best value for a hyperparameter is called hyperparameter optimization \(HPO\)\. For more information, see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\. 

The table also provides the following information for each hyperparameter:
+ **Range**: \[lower bound, upper bound\]
+ **Value type**: Integer, Continuous \(float\), Categorical \(Boolean, list, string\)
+ **HPO tunable**: Can the parameter participate in HPO?


| Name | Description | 
| --- | --- | 
| Algorithm hyperparameters | 
| item\_id\_hidden\_dim |  The number of hidden variables Amazon Personalize uses to model item ID embeddings based on interactions data\. *Hidden variables* recreate users' purchase history and item statistics to generate ranking scores\. To use `item_id_hidden_dim`, you must use HPO and provide minimum and maximum range values\. Amazon Personalize uses HPO to find the best value within the range you specify\. Specify a greater maximum value when you have a large Interactions dataset\. Using a greater maximum value requires more time to process\.   To use HPO, set `performHPO` to `true` when you call the [CreateSolution](API_CreateSolution.md) operation\. Default value: 100 Range: \[30, 200\] Value type: Integer HPO tunable: Yes  | 
| item\_metadata\_hidden\_dim |  The number of hidden variables Amazon Personalize uses to model item metadata\. To use `item_metadata_hidden_dim`, you must use HPO and provide minimum and maximum range values\. Amazon Personalize uses HPO to find the best value within the range you specify\. Specify a greater maximum value when you have a large Interactions dataset\. Using a greater maximum requires more time to process\.   To use HPO, set `performHPO` to `true` when you call the [CreateSolution](API_CreateSolution.md) operation\. Default value: 100 Range: \[30, 200\] Value type: Integer HPO tunable: Yes  | 