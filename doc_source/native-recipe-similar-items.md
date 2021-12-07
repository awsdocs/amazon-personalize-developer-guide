# Similar\-Items recipe<a name="native-recipe-similar-items"></a>

 The Similar\-Items \(aws\-similar\-items\) generates recommendations for items that are similar to an item you specify\. Similar\-Items is optimized for similar item recommendation scenarios with item metadata\. To use Similar\-Items, you must create an Interactions dataset and an Items dataset\. Use Similar\-Items when your catalog has item metadata and items with little to no interactions, but your Interactions dataset still has at minimum 1000 unique historical and event interactions \(combined\)\. 

Similar\-Items calculates similarity based on both the co\-occurrence of the item in user histories in your Interaction dataset, and the item metadata, including categorical and unstructured text metadata, in your Items dataset\. For example, with Similar\-Items Amazon Personalize could recommend items customers frequently bought together with a similar style, or movies that different users also watched with a similar description\. 

With Similar\-Items, you provide an item ID in a [ GetRecommendations ](API_RS_GetRecommendations.md) operation \(or the Amazon Personalize console\) and Amazon Personalize returns a list of similar items\. Or you can use a batch workflow to get similar items for all of the items in your inventory \(see [Getting batch recommendations and user segments](recommendations-batch.md)\)\. You can get recommendations for items that are similar to a cold item \(an item with fewer than five interactions\)\. If Amazon Personalize can't find the item ID that you specify in your recommendation request or batch input file, the recipe returns popular items as recommendations\. 

 For information on formatting categorical and unstructured text metadata in your Items dataset, see [Item data](items-datasets.md)\. If you don't have item metadata and want to recommend similar items, use the [SIMS recipe](native-recipe-sims.md)\. 

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
| Algorithm hyperparameters \(HPO only\) | 
| item\_id\_hidden\_dimension |  The number of hidden variables Amazon Personalize uses to model item ID embeddings based on interactions data\. *Hidden variables* recreate users' purchase history and item statistics to generate ranking scores\. To use `item_id_hidden_dimension`, you must use HPO and provide minimum and maximum range values\. Amazon Personalize uses HPO to find the best value within the range you specify\. Specify a greater maximum value when you have a large Interactions dataset\. Using a greater maximum value requires more time to process\.   To use HPO, set `performHPO` to `true` when you call the [ CreateSolution ](API_CreateSolution.md) operation\. Default value: 100 Range: \[30, 200\] Value type: Integer HPO tunable: HPO only  | 
| item\_metadata\_hidden\_dimension |  The number of hidden variables Amazon Personalize uses to model item metadata\. To use `item_metadata_hidden_dimension`, you must use HPO and provide minimum and maximum range values\. Amazon Personalize uses HPO to find the best value within the range you specify\. Specify a greater maximum value when you have a large Interactions dataset\. Using a greater maximum requires more time to process\.   To use HPO, set `performHPO` to `true` when you call the [ CreateSolution ](API_CreateSolution.md) operation\. Default value: 100 Range: \[30, 200\] Value type: Integer HPO tunable: HPO only  | 