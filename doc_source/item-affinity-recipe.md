# Item\-Affinity recipe<a name="item-affinity-recipe"></a>

The Item\-Affinity \(aws\-item\-affinity\) recipe is a USER\_SEGMENTATION recipe that creates a user segment \(group of users\) for each item that you specify\. These are the users Amazon Personalize predicts will most likely interact with each item\. Use Item\-Affinity to learn more about your users and take actions based on their respective user segments\. 

For example, you might want to create a marketing campaign for your retail application based on user preferences for items in your catalog\. Item\-Affinity would create a user segment for each item based on data in your Interactions and Items datasets\. You could use this to promote different items to different user segments based on the likelihood that they will take an action \(for example, click an item or purchase an item\)\. Other uses might include cross\-selling products to different sets of users or identifying prospective job applicants\. 

 To get user segments based on items, you create a solution and a solution version with the Item\-Affinity recipe, then add a list of items in JSON format to an Amazon S3 bucket and create a [batch segment job](creating-batch-seg-job.md)\. Amazon Personalize outputs a user segment for each item to your output location in Amazon S3\. Your input data can have a maximum of 500 items to get user segments for\. For information about preparing input data for a batch segment job, see [Preparing and importing batch input data](batch-data-upload.md)\. 

You must have an Interactions dataset to use Item\-Affinity\. Items and Users datasets are optional\.  You can get user segments with batch segment jobs\. For more information, see [Getting batch recommendations and user segments](recommendations-batch.md)\.

 After you create a solution version, make sure you keep your solution version and data up to date\. With Item\-Affinity, you must manually create a new solution version \(retrain the model\) to reflect updates to your catalog and update the model with your user’s most recent behavior\. For more information, see [Maintaining recommendation relevance](maintaining-relevance.md)\. To get a user segment for an item, the item must have been present when you created the solution version\.

The Item\-Affinity recipe has the following properties:
+  **Name** – `aws-item-affinity`
+  **Recipe Amazon Resource Name \(ARN\)** – `arn:aws:personalize:::recipe/aws-item-affinity`
+  **Algorithm ARN** – `arn:aws:personalize:::algorithm/aws-item-affinity`
+  **Feature transformation ARN** – `arn:aws:personalize:::feature-transformation/item-affinity`
+  **Recipe type** – `USER_SEGMENTATION`

The following table describes the hyperparameters for the Item\-Affinity recipe\. A *hyperparameter* is an algorithm parameter that you adjust to improve model performance\. Algorithm hyperparameters control how the model performs\. You can't use hyperparameter optimization \(HPO\) with the Item\-Affinity recipe\.  

The table also provides the following information for each hyperparameter:
+ **Range**: \[lower bound, upper bound\]
+ **Value type**: Integer, Continuous \(float\), Categorical \(Boolean, list, string\)


| Name | Description | 
| --- | --- | 
| Algorithm hyperparameters | 
| hidden\_dimension |  The number of hidden variables used in the model\. *Hidden variables* recreate users' purchase history and item statistics to generate ranking scores\. Specify a greater number of hidden dimensions when your Interactions dataset includes more complicated patterns\. Using more hidden dimensions requires a larger dataset and more time to process\.  Default value: 149 Range: \[32, 256\] Value type: Integer   | 