# VIDEO\_ON\_DEMAND use cases<a name="VIDEO_ON_DEMAND-use-cases"></a>

The following sections list the requirements and Amazon Resource Name \(ARN\) for each VIDEO\_ON\_DEMAND use case\. For all use cases, your interactions data must have the following: 
+ 1000 records of interaction data from bulk or individual imports or both\.
+ 25 unique users with at least 2 interactions each\.

**Note**  
If you use the [CreateRecommender](API_CreateRecommender.md) API, provide the ARN listed here for the recipe ARN\.

**Topics**
+ [Because you watched X](#because-you-watched-use-case)
+ [More like X](#more-like-y-use-case)
+ [Most popular](#hot-picks-use-case)
+ [Trending now](#trending-now-use-case)
+ [Top picks for you](#top-picks-use-case)

## Because you watched X<a name="because-you-watched-use-case"></a>

Get recommendations for videos that other users also watched based on a video that you specify\. With this use case, Amazon Personalize automatically filters videos the user watched based on the userId you specify\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-because\-you\-watched\-x
+ **GetRecommendations API requirements: **

  `userId`: Required

  `itemId`: Required
+ **Required dataset:** Interactions 
+ **Required event types:** At minimum, 1000 `Watch` events\.

## More like X<a name="more-like-y-use-case"></a>

Get recommendations for videos that are similar to a video that you specify\. With this use case, Amazon Personalize automatically filters videos the user watched based on the userId that you specify and `Watch` events\. For better performance, record `Click` events in addition to the required `Watch` events
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-more\-like\-x
+ **GetRecommendations API requirements: **

  `userId`: Required

  `itemId`: Required
+ **Required datasets:** Interactions and Items
+ **Required number of events:** At minimum, 1000 events of any type\.
+ **Recommended event types:** `Watch` and `Click` events\.

## Most popular<a name="hot-picks-use-case"></a>

Get recommendations for videos that have been watched by the most users\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-most\-popular
+ **GetRecommendations requirements:**

  `userId`: Required

  `itemId`: Not used
+ **Required dataset:** Interactions 
+ **Required event types:** At minimum, 1000 `Watch` events\.

## Trending now<a name="trending-now-use-case"></a>

Get recommendations for videos that are currently trending\. Trending videos are items that are rapidly becoming more popular with your users\. Every two hours, Amazon Personalize automatically evaluates your interactions data and identifies trending items\. 
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-trending\-now
+ **GetRecommendations API requirements: **

  `userId`: Required only if you filter by CurrentUser or by items a user has interacted with

  `itemId`: Not used
+ **Required dataset:** Interactions
+ **Required number of events:** At minimum, 1000 events of any type\.

## Top picks for you<a name="top-picks-use-case"></a>

Get personalized content recommendations for a user that you specify\. With this use case, Amazon Personalize automatically filters videos the user watched based on the userId that you specify and `Watch` events\.

When recommending items, this use case uses exploration\. Exploration involves testing different item recommendations to learn how users respond to items with very little interaction data\. You can configure exploration when you create your recommender\. 
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-top\-picks
+ **GetRecommendations requirements: **

  `userId`: Required

  `itemId`: Not used
+ **Required datasets:** Interactions and Items
+ **Required number of events:** At minimum, 1000 events\.
+ **Recommended event types:** `Click` and `Watch` events\.
+ **Exploration configuration parameters:** When you create a recommender, you can configure exploration with the following\.
  + Emphasis on exploring less relevant items \(for APIs, this is called explorationWeight in the [RecommenderConfig](API_RecommenderConfig.md)\): Configure how much to explore\. Specify a decimal value between 0 to 1\. The default is 0\.3\. The closer the value is to 1, the more exploration\. With more exploration, recommendations include more items with less interactions data or relevance\. At zero, no exploration occurs and recommendations are based on current data \(relevance\)\.
  + Exploration item age cutoff: Specify the maximum item age in days since the latest interaction\. This defines the scope of item exploration\. For example, if you enter 10, Amazon Personalize exploration includes only items with interactions data from the 10 days since the latest interaction in the dataset\.

    To increase the items Amazon Personalize considers during exploration, enter a greater value\. The minimum is 1 day and the default is 30 days\. Recommendations might include items without interactions data from outside this time frame\. This is because these items are relevant to the user and exploration didn't identify them\.