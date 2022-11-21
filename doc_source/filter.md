# Filtering recommendations and user segments<a name="filter"></a>

When getting recommendations with an Amazon Personalize Custom dataset group or Domain dataset group, you can filter results based on custom criteria\. For example, you might not want to recommend products that a user has already purchased or recommend only items for a particular age group\. Similarly, with USER\_SEGMENTATION recipes, you might not want to include certain types of users in user segments\. By filtering your results, you can control the items that will be recommended to users or the users that will be included in user segments\. 

 When you get personalized recommendations or similar items, you can specify a promotion in your request\. A *promotion* uses a filter to define additional business rules that apply to a configurable subset of recommended items\. For more information see [Promoting items in recommendations \(Custom dataset group\)](promoting-items.md)\. 

When filtering items and users, Amazon Personalize recognizes event types that were present during solution version training or event types that were streamed using the PutEvents operation\. For item and user data imported individually, Amazon Personalize updates any filters you created in the dataset group with your new item and user data within 20 minutes from the last individual import\. For more information, see [Importing individual records](incremental-data-updates.md)\. 

You apply a filter and specify any filter parameter values when you call the [GetRecommendations](API_RS_GetRecommendations.md) or [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) operations, or when you get recommendations from a campaign or a recommender in the console\. 

For batch workflows, you include any filter parameter values in your input JSON\. Then you specify the filter's Amazon Resource Name \(ARN\) when you create a batch inference job or batch segment job\. For more information see [Filtering batch recommendations and user segments](filter-batch.md)\.

You can create, edit, delete, and apply filters using the Amazon Personalize console, the AWS Command Line Interface \(AWS CLI\), and the AWS SDKs\. For information about the number of filters you can create and how many parameters you can use in filter expressions, see [Service quotas](limits.md#limits-table)\.

**Important**  
To filter recommendations using a filter with parameters and a campaign that you deployed before November 10, 2020, you must redeploy the campaign by using the [UpdateCampaign](API_UpdateCampaign.md) operation or create a new campaign\.

**Topics**
+ [Filter expressions](filter-expressions.md)
+ [Filtering real\-time recommendations](filter-real-time.md)
+ [Filtering batch recommendations and user segments](filter-batch.md)