# Creating recommenders \(console\)<a name="creating-recommenders-console"></a>

**Important**  
A high `minRecommendationRequestsPerSecond` will increase your bill\. We recommend starting with 1 for `minRecommendationRequestsPerSecond` \(the default\)\. Track your usage using Amazon CloudWatch metrics, and increase the `minRecommendationRequestsPerSecond` as necessary\. For more information see [Minimum recommendation requests per second and auto\-scaling](creating-recommenders.md#min-rrps-auto-scaling)\.

 Create recommenders for each of your use cases with the Amazon Personalize console as follows\. If you just created your Domain dataset group and you are already on the **Overview** page, skip to step 3\.

**To create recommenders**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1.  On the **Dataset groups** page, choose your Domain dataset group\. 

1.  On the **Use <domain name> recommenders** tab of the middle card choose **Create recommenders**\. 

1. On the **Create recommenders** page, choose the use cases you want to create recommenders and give each a **Recommender name**\. Amazon Personalize creates a recommender for each use case that you choose\. The available use cases depend on your domain\. For information on choosing a use case see [Choosing recommender use cases](domain-use-cases.md)\.

1. To make changes to your recommender configuration, optionally toggle **Use default recommender configurations** to modify the following:
   + Optionally make any changes to the recommender's **Minimum recommendation requests per second**\. A high `minRecommendationRequestsPerSecond` will increase your bill\. We recommend starting with 1 for `minRecommendationRequestsPerSecond` \(the default\)\. Track your usage using Amazon CloudWatch metrics, and increase the `minRecommendationRequestsPerSecond` as necessary\. For more information see [Minimum recommendation requests per second and auto\-scaling](creating-recommenders.md#min-rrps-auto-scaling)\.
   + For `Top picks for your` or `Recommended for you` use cases, optionally make changes to exploration configuration\. Exploration involves testing different item recommendations to learn how users respond to items with very little interaction data\. Use the following fields to configure exploration: 
     + Emphasis on exploring less relevant items \(for APIs, this is called explorationWeight in the [RecommenderConfig](API_RecommenderConfig.md)\): Configure how much to explore\. Specify a decimal value between 0 to 1\. The default is 0\.3\. The closer the value is to 1, the more exploration\. With more exploration, recommendations include more items with less interactions data or relevance\. At zero, no exploration occurs and recommendations are based on current data \(relevance\)\.
     + Exploration item age cutoff: Specify the maximum item age in days since the latest interaction\. This defines the scope of item exploration\. For example, if you enter 10, Amazon Personalize exploration includes only items with interactions data from the 10 days since the latest interaction in the dataset\.

       To increase the items Amazon Personalize considers during exploration, enter a greater value\. The minimum is 1 day and the default is 30 days\. Recommendations might include items without interactions data from outside this time frame\. This is because these items are relevant to the user and exploration didn't identify them\.

1. For **Tags**, optionally add any tags\. For more information about tagging Amazon Personalize resources, see [Tagging Amazon Personalize resources](tagging-resources.md)\.

1. Choose **Create recommenders** to create recommenders for each of your use cases\.

   You can monitor the status of each recommender on the **Recommenders** page\. When your recommender status is Active, you can use it in your application to get recommendations\.