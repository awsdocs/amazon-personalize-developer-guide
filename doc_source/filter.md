# Filtering recommendations and user segments<a name="filter"></a>

When getting recommendations with an Amazon Personalize Custom dataset group or Domain dataset group, you can filter results based on custom criteria\. For example, you might not want to recommend products that a user has already purchased or recommend only items for a particular age group\. Similarly, with USER\_SEGMENTATION recipes, you might not want to include certain types of users in user segments\. By filtering your results, you can control the items that will be recommended to users or the users that will be included in user segments\. 

When filtering items and users, Amazon Personalize recognizes event types that were present during solution version training or that were streamed using the [PutEvents](API_UBS_PutEvents.md) operation\. For item and user data imported incrementally, Amazon Personalize updates any filters you created in the dataset group with your new item and user data within 20 minutes from the last incremental import\. For more information, see [Importing records incrementally](incremental-data-updates.md)\. 

 To filter items from recommendations or users from user segments, you first create a filter, which consists of a filter name and a SQL\-like filter expression\. You can then either specify filter criteria when you create the filter or pass criteria as a parameter when you get recommendations\. 

You then apply the filter and specify filter parameter values when you call the [GetRecommendations](API_RS_GetRecommendations.md) or [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) operations, or when you get recommendations from a campaign or a recommender in the console\. 

For batch workflows, you include filter parameter values in your input JSON and apply the filter when you call the [CreateBatchInferenceJob](API_CreateBatchInferenceJob.md) or [CreateBatchSegmentJob](API_CreateBatchSegmentJob.md) operations or when you create a batch inference job or batch segment job in the console\. You can create, edit, delete, and apply filters using the Amazon Personalize console, the AWS Command Line Interface \(AWS CLI\), and the AWS SDKs\.

For information about the number of filters you can create and how many parameters you can use in filter expressions, see [Service quotas](limits.md#limits-table)\.

**Important**  
To filter recommendations using a filter with parameters and a campaign that you deployed before November 10, 2020, you must redeploy the campaign by using the [UpdateCampaign](API_UpdateCampaign.md) operation or create a new campaign\.

**Topics**
+ [Filter expressions](filter-expressions.md)
+ [Filtering real\-time recommendations](filter-real-time.md)
+ [Filtering batch recommendations and user segments](filter-batch.md)