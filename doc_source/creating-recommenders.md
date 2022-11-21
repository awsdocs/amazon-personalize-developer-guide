# Creating recommenders<a name="creating-recommenders"></a>

After you create a Domain dataset group and import interactions data into an Interactions dataset, you can create recommenders for your domain use cases\. A *recommender* is a Domain dataset group resource that generates recommendations\. You create a recommender for a Domain dataset group and use in your application to get real\-time recommendations with the [GetRecommendations](API_RS_GetRecommendations.md) operation\. 

When you create a recommender, you specify a use case and Amazon Personalize trains the models backing the recommender with the best configurations for the use case\. Each use case has different API requirements for getting recommendations\. You can create at most 15 recommenders per region\.

 Amazon Personalize automatically retrains the models backing your recommenders every 7 days\. This is a full retraining that creates entirely new models based on the entirety of the data in your datasets\. With *Top picks for you* and *Recommended for you* use cases, Amazon Personalize updates the existing models every two hours to include new items in recommendations with exploration\. 

 You can create recommenders with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\. 

**Recommender statuses**

A recommender can be in one of the following states:
+ CREATE PENDING > CREATE IN\_PROGRESS > ACTIVE \-or\- CREATE FAILED
+ DELETE PENDING > DELETE IN\_PROGRESS

To get the recommender status, navigate to the Recommenders page in the Amazon Personalize console or use the the [DescribeRecommender](API_DescribeRecommender.md) operation\.

**Topics**
+ [Choosing recommender use cases](domain-use-cases.md)
+ [Minimum recommendation requests per second and auto\-scaling](#min-rrps-auto-scaling)
+ [Creating recommenders \(console\)](creating-recommenders-console.md)
+ [Creating recommenders \(AWS CLI\)](creating-recommenders-cli.md)
+ [Creating recommenders \(AWS SDKs\)](creating-recommenders-sdk.md)

## Minimum recommendation requests per second and auto\-scaling<a name="min-rrps-auto-scaling"></a>

When you create a recommender, you can configure the recommender's minimum recommendation requests per second\. The minimum recommendation requests per second \(`minRecommendationRequestsPerSecond`\) specifies the baseline recommendation request throughput provisioned by Amazon Personalize\. The default minRecommendationRequestsPerSecond is `1`\. A recommendation request is a single `GetRecommendations` operation\. Request throughput is measured in requests per second and Amazon Personalize uses your requests per second to derive your requests per hour and the price of your recommender usage\. 

 If your requests per second increases beyond `minRecommendationRequestsPerSecond`, Amazon Personalize auto\-scales the provisioned capacity up and down, but never below `minRecommendationRequestsPerSecond`\. There's a short time delay while the capacity is increased that might cause loss of requests\.

 Your bill is the greater of either the minimum requests per hour \(based on minRecommendationRequestsPerSecond\) or the actual number of requests\. The actual request throughput used is calculated as the average requests/second within a one\-hour window\. We recommend starting with the default `minRecommendationRequestsPerSecond`, track your usage using Amazon CloudWatch metrics, and then increase the `minRecommendationRequestsPerSecond` as necessary\. 

 