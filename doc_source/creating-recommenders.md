# Creating recommenders<a name="creating-recommenders"></a>

After you create a Domain dataset group, you can create recommenders for your domain use cases\. A *recommender* is a Domain dataset group resource that generates recommendations\. You create a recommender for a Domain dataset group and use in your application to get real\-time recommendations with the [GetRecommendations](API_RS_GetRecommendations.md) operation\. 

When you create a recommender, you specify a use case and Amazon Personalize trains the models backing the recommender with the best configurations for the use case\. Each use case has different API requirements for getting recommendations\. You can create at most 5 recommenders per AWS account\.

 Amazon Personalize automatically retrains the models backing your recommenders every 7 days\. This is a full retraining that creates entirely new models based on the entirety of the data in your datasets\. With Top picks for you and Recommended for you use cases, Amazon Personalize updates the existing models every two hours to include new items in recommendations with exploration\. 

 You can create recommenders with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\. 

**Status**

A recommender can be in one of the following states:
+ CREATE PENDING > CREATE IN\_PROGRESS > ACTIVE \-or\- CREATE FAILED
+ DELETE PENDING > DELETE IN\_PROGRESS

To get the recommender status, navigate to the Recommenders page in the Amazon Personalize console or use the the [DescribeRecommender](API_DescribeRecommender.md) operation\.

**Topics**
+ [Choosing recommender use cases](domain-use-cases.md)
+ [Creating recommenders \(console\)](creating-recommenders-console.md)
+ [Creating recommenders \(AWS CLI\)](creating-recommenders-cli.md)
+ [Creating recommenders \(AWS SDKs\)](creating-recommenders-sdk.md)

 