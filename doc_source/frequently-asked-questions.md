# Frequently asked questions<a name="frequently-asked-questions"></a>

 The following are answers to frequently asked questions related to importing data, training, model deployment, recommendations, and filters in Amazon Personalize\. 

 For more questions and answers, see the [Amazon Personalize Cheat Sheet](https://github.com/aws-samples/amazon-personalize-samples/blob/master/PersonalizeCheatSheet2.0.md) in the [Amazon Personalize samples](https://github.com/aws-samples/amazon-personalize-samples) repository\. 

**Topics**
+ [Data import and management](#data-import-questions)
+ [Creating a solution and solution version \(Custom dataset group\)](#training-questions)
+ [Model deployment \(Custom dataset group campaigns\)](#deployment-questions)
+ [Recommendations](#recommendations-questions)
+ [Filtering recommendations](#filters-questions)

## Data import and management<a name="data-import-questions"></a>

*What format should my bulk data be in?*

Your bulk data must be in comma\-separated values \(CSV\) format\. The first row of your CSV file must contain column headers\. The column headers in your CSV file need to map to the schema to create the dataset\. If your data includes any non\-ASCII encoded characters, your CSV file must be encoded in UTF\-8 format\. Don't enclose headers in quotation marks \("\)\. `TIMESTAMP` and `CREATION_TIMESTAMP` data must be in *UNIX epoch* time format\. For more information on timestamp data, see [Timestamp data](data-prep-formatting.md#timestamp-data)\. For more information about schemas, see [Datasets and schemas](how-it-works-dataset-schema.md)\. 

*How much training data do I need?*

 For all use cases \(Domain dataset groups\) and recipes \(Custom dataset groups\), your interactions data must have the following: 
+ At minimum 1000 interactions records from users interacting with items in your catalog\. These interactions can be from bulk imports, or streamed events, or both\.
+ At minimum 25 unique user IDs with at least 2 interactions for each\.

You can start out with an empty Interactions dataset and, when you have recorded enough data, create your recommender \(Domain dataset group\) or solution version \(Custom dataset group\) using only new recorded events\. Some recipes and use cases may have additional data requirements\. For information on use case requirements, see [Choosing recommender use cases](domain-use-cases.md)\. For information on recipe requirements, see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\. 

*How do I update an item or user's attributes?*

 Use the Amazon Personalize console or the [PutItems](API_UBS_PutItems.md) or [PutUsers](API_UBS_PutUsers.md) operations to import an item or user with the same item ID but with the modified attributes\.

*How do I delete an item or user?*

 Amazon Personalize doesn't support deleting a specific item or user\. To make sure that an item or user doesn't appear in recommendations, use a filter to exclude items\. For more information, see [Filtering recommendations and user segments](filter.md)\. 

*How do I delete a schema?*

 You can delete a schema only with the [DeleteSchema](API_DeleteSchema.md) operation\. You can't use the Amazon Personalize console to delete a schema\. 

## Creating a solution and solution version \(Custom dataset group\)<a name="training-questions"></a>

*What recipe should I use?*

 The Amazon Personalize recipe that you use depends on your use case\. For information on matching use cases to recipes, see [Determining your use case](determining-use-case.md)\. The [Amazon Personalize Cheat Sheet](https://github.com/aws-samples/amazon-personalize-samples/blob/master/PersonalizeCheatSheet2.0.md) also includes use case and recipe information\. 

*How often should I retrain my model?*

 Retraining helps keep your recommendations relevant as your catalog grows and users interact with items\. Retraining frequency depends on your business requirements and the recipe that you use\. For most workloads, we recommend creating a new solution version weekly with training mode set to `FULL`\. This creates a new solution version based on the entirety of the training data from the datasets in your dataset group\. 

 For more information, see [Maintaining recommendation relevance](maintaining-relevance.md)\. 

*Should I use AutoML?*

 No, instead we recommend that you match your use case to different Amazon Personalize recipes and choose a recipe\. For information on matching use cases to recipes, see [Determining your use case](determining-use-case.md)\. For a complete list of recipes, see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\. 

## Model deployment \(Custom dataset group campaigns\)<a name="deployment-questions"></a>

*How do I set a maximum transaction throughput for a campaign?*

 You can only set the minimum throughput for a campaign\. When you create an Amazon Personalize campaign, you specify a dedicated transaction capacity for creating real\-time recommendations for your application users\. If your TPS increases beyond `minProvisionedTPS`, Amazon Personalize auto\-scales the provisioned capacity up and down, but never below the `minProvisionedTPS`\. For more information, see [Minimum provisioned transactions per second and auto\-scaling](campaigns.md#min-tps-auto-scaling)\. 

*How do I monitor the cost of my campaigns?*

 The Amazon Personalize Monitor project provides a CloudWatch dashboard, custom metrics, utilization alarms, and cost optimization functions for Amazon Personalize campaigns\. See the [Amazon Personalize Monitor](https://github.com/aws-samples/amazon-personalize-monitor) in the [Amazon Personalize samples](https://github.com/aws-samples/amazon-personalize-samples) repository\. 

## Recommendations<a name="recommendations-questions"></a>

*How can I tell if my Amazon Personalize model is generating quality recommendations?*

 Evaluate the performance of your solution version with offline and online metrics \(see [Step 4: Evaluating a solution version with metrics](working-with-training-metrics.md)\) and online testing \(such as A/B testing\)\. For more information about testing, see [ Using A/B testing to measure the efficacy of recommendations generated by Amazon Personalize](https://aws.amazon.com/blogs/machine-learning/using-a-b-testing-to-measure-the-efficacy-of-recommendations-generated-by-amazon-personalize/) 

*How do I delete my batch inference job and why is its status "active"?*

You can't delete batch inference jobs\. When a batch inference job's status is *active*, the job is complete\. You can access your recommendations in the output Amazon S3 bucket or folder\. You won't incur additional cost from the batch inference job once the job is complete\. However you may incur additional charges from other services such as Amazon S3 for input and output data storage\. 

*Why does my SIMS\-backed campaign recommend items that are not similar based on metadata?*

SIMS uses your Interactions dataset to determine similarity; not item metadata such as color or price\. SIMS identifies the co\-occurrence of the item in user histories in your Interaction dataset to recommend similar items\. For more information, see [SIMS recipe](native-recipe-sims.md)\. 

*Can I get more than 500 items from a single GetRecommendations API operation?*

500 is the maximum number of items that you can retrieve in a single [GetRecommendations](API_RS_GetRecommendations.md)\. This value cannot be increased\. 

## Filtering recommendations<a name="filters-questions"></a>

*Why aren't my recommendations filtered as expected?*

 This can occur for a variety of reasons: 
+  There may be issue with the format or syntax of your filter expression\. For examples of correctly formatted filter expressions, see [Filter expression examples](filter-expressions.md#filter-expression-examples)\. 
+ Amazon Personalize considers up to 100 of the most recent interactions per user per event type\. This is an adjustable quota\. You can request a quota increase using the [Service Quotas console](https://console.aws.amazon.com/servicequotas/)\. 

For more information, see [Filtering recommendations and user segments](filter.md)\.

*How can I remove already purchased items from recommendations?*

For ECOMMERCE Domain dataset groups, if you create a recommender with the [Recommended for you](ECOMMERCE-use-cases.md#recommended-for-you-use-case) or [Customers who viewed X also viewed](ECOMMERCE-use-cases.md#customers-also-viewed-use-case) use case, Amazon Personalize automatically filters items the user purchased based on the userId that you specify and `Purchase` events\. 

For Custom dataset groups or other Domain dataset group use cases, use a filter to remove purchased items\. Add a `Purchased` event type attribute to your data, record *Purchase* events with the `PutItems` operation, and create a filter that removes purchased items from recommendations\. For example:

```
EXCLUDE ItemID WHERE Interactions.EVENT_TYPE IN ("purchased")
```

For more information, see [Filtering recommendations and user segments](filter.md)\.