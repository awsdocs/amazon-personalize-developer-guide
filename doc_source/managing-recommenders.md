# Managing recommenders<a name="managing-recommenders"></a>

Amazon Personalize automatically retrains the models backing your recommenders every 7 days\. This is a full retraining that creates entirely new models based on the entirety of the data in your datasets\. For recommenders that create personalized recommendations for users, Amazon Personalize updates the existing models every two hours to include new items in recommendations with exploration\. For instructions, see [Creating recommenders](creating-recommenders.md)\. After you create a recommender, you can update the recommender's configuration in the Amazon Personalize console or using the [UpdateRecommender](API_UpdateRecommender.md) API operation\. 

If you want to pause billing for an active recommender, you can stop the recommender and restart it later\. For more information, see [Stopping and starting a recommender](stopping-starting-recommender.md)\. 

For all use cases, you can update the recommender's minimum recommendation requests per second\. The minimum recommendation requests per second \(`minRecommendationRequestsPerSecond`\) parameter specifies the baseline recommendation request throughput that's provisioned by Amazon Personalize\. The default minRecommendationRequestsPerSecond is `1`\. For more information, see [Minimum recommendation requests per second and auto\-scaling](creating-recommenders.md#min-rrps-auto-scaling)\. 

For *Top picks for you* and *Recommended for you* use cases, you can update exploration configuration with the following fields: 
+ Emphasis on exploring less relevant items \(for APIs, this is called explorationWeight in the [RecommenderConfig](API_RecommenderConfig.md)\): Configure how much to explore\. Specify a decimal value between 0 to 1\. The default is 0\.3\. The closer the value is to 1, the more exploration\. With more exploration, recommendations include more items with less interactions data or relevance\. At zero, no exploration occurs and recommendations are based on current data \(relevance\)\.
+ Exploration item age cutoff: Specify the maximum item age in days since the latest interaction\. This defines the scope of item exploration\. For example, if you enter 10, Amazon Personalize exploration includes only items with interactions data from the 10 days since the latest interaction in the dataset\.

  To increase the items Amazon Personalize considers during exploration, enter a greater value\. The minimum is 1 day and the default is 30 days\. Recommendations might include items without interactions data from outside this time frame\. This is because these items are relevant to the user and exploration didn't identify them\.

## Updating a recommender \(Amazon Personalize console\)<a name="updating-recommender-console"></a>

 After you create a recommender, you can update the recommender's minimum recommendation requests per second\. For *Top picks for you* and *Recommended for you* use cases, you can update exploration configuration\. To update a recommender with the console, do the following\. 

**To update a recommender's configuration \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1.  On the **Dataset groups** page, choose your Domain dataset group\. 

1. From the navigation pane, choose **Recommenders**\.

1. On the **Recommenders** page, choose the recommender that you want to update\.

1. In **Recommender configuration** choose **Edit**\.

1. Change the recommender's configuration to what you want, and then choose **Update**\.