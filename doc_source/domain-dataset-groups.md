# Domain dataset groups<a name="domain-dataset-groups"></a>

 A *Domain dataset group* is a Amazon Personalize container for domain specific pre\-configured resources, including datasets, recommenders, and filters\. Use a Domain dataset group if you have a streaming video or e\-commerce application and want to let Amazon Personalize find the best configurations for your recommenders\. 

A *recommender* is a resource that generates recommendations for specific use cases, such as recommending similar movies\. You can create at most recommenders per AWS account\. The domain you choose when you create a Domain dataset group determines what data you import and the use cases available for your recommenders\. 

 Amazon Personalize automatically retrains the models backing your recommenders every 7 days\. This is a full retraining that creates entirely new models based on the entirety of the data in your datasets\. With *Top picks for you* and *Recommended for you* use cases, Amazon Personalize updates the existing models every two hours to include new items in recommendations with exploration\. 

You can create a Domain dataset group with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\. To get recommendations with a Domain dataset group, do the following: 

1. Create a dataset group and choose your business domain\.

1.  Create a schema that includes your domain's required fields\. You must create a schema before you import data\. 

1.  Format any historical input data to match your schema and upload the data into an Amazon S3 bucket\. 

1.  Import your data into an Amazon Personalize dataset and record real\-time event data from user interactions with items\. 

1. When you have collected enough data, create recommenders for each of your use cases\.

    The minimum data requirements to train a model are as follows: 
   +  1000 records of combined interaction data \(after filtering by `eventType` and `eventValueThreshold`, if provided\)\.
   +  25 unique users with at least 2 interactions each\. 

1. Use your recommenders in your application to get recommendations with the GetRecommendations operation\.

1. As your catalog grows, maintain recommendation relevance by keeping your data current with bulk or incremental data import operations\. Amazon Personalize manages the life cycle of your recommenders and updates them as your datasets change\. 

 If you start with a Domain dataset group, you can still add custom resources such as solutions and solution versions trained with recipes for custom use cases\. In the Amazon Personalize console, you create and manage these resources in the **Custom resources** section of the navigation pane\. For more information about custom resources see [Custom dataset groups](custom-dataset-groups.md)\. 

**Topics**
+ [Creating a Domain dataset group](create-domain-dataset-group.md)
+ [Creating recommenders](creating-recommenders.md)
+ [Getting recommendations from a recommender](domain-dsg-recommendations.md)
+ [Managing Domain dataset group resources](managing-domain-resources.md)
+ [Maintaining recommendation relevance \(Domain dataset group\)](maintaining-relevance-domain.md)