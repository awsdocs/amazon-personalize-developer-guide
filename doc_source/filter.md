# Filtering recommendations<a name="filter"></a>

When getting recommendations with Amazon Personalize, you can filter results based on custom criteria\. For example, you might not want to recommend products that a user has already purchased, or recommend movies that a user has already watched\. By filtering your recommendations, you can control the items that will be recommended to users\.

When filtering items, Amazon Personalize recognizes event types that were present during solution version training or that were streamed using the [ PutEvents ](API_UBS_PutEvents.md) operation\. For item and user data imported incrementally, Amazon Personalize updates any filters you created in the dataset group with your new item and user data within 20 minutes from the last incremental import\. For more information, see [Importing records incrementally](incremental-data-updates.md)\. 

You create filters for a dataset group and apply filters to real\-time and batch recommendations at the campaign level\. To filter items, you first create a filter, which consists of a filter name and a SQL\-like filter expression\. You can either specify filter criteria when you create the filter or pass criteria as a parameter when you get recommendations\. 

You then apply the filter and specify filter parameter values when you call the [ GetRecommendations ](API_RS_GetRecommendations.md) or [ GetPersonalizedRanking ](API_RS_GetPersonalizedRanking.md) operations, or when you get recommendations from a campaign in the console\. 

For batch workflows, you include filter parameter values in your input JSON and apply the filter when you call the [ CreateBatchInferenceJob ](API_CreateBatchInferenceJob.md) operation or create a batch inference job in the console\. You can create, edit, delete, and apply filters using the Amazon Personalize console, the AWS Command Line Interface \(AWS CLI\), and the AWS SDKs\.

For information about the number of filters you can create and how many parameters you can use in filter expressions, see [Service quotas](limits.md#limits-table)\.

**Important**  
To filter recommendations using a filter with parameters and a campaign that you deployed before November 10, 2020, you must redeploy the campaign by using the [ UpdateCampaign ](API_UpdateCampaign.md) operation or create a new campaign\.

**Topics**
+ [Filter expressions](filter-expressions.md)
+ [Filtering real\-time recommendations](filter-real-time.md)
+ [Filtering batch recommendations](filter-batch.md)