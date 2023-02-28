# ECOMMERCE use cases<a name="ECOMMERCE-use-cases"></a>

The following sections list the requirements and Amazon Resource Name \(ARN\) for each ECOMMERCE use case\. For all use cases, your interactions data must have the following: 
+ 1000 records of interaction data from bulk or individual imports or both\.
+ 25 unique users with at least 2 interactions each\.

**Note**  
If you use the [CreateRecommender](API_CreateRecommender.md) API, provide the ARN listed here for the recipe ARN\.

**Topics**
+ [Most viewed](#most-viewed-use-case)
+ [Best sellers](#best-sellers-use-case)
+ [Frequently bought together](#frequently-bought-together-use-case)
+ [Customers who viewed X also viewed](#customers-also-viewed-use-case)
+ [Recommended for you](#recommended-for-you-use-case)

## Most viewed<a name="most-viewed-use-case"></a>

Get recommendations for popular items based on how many times that your customers viewed an item\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-ecomm\-popular\-items\-by\-views
+ **GetRecommendations requirements: **

  `userId`: Required

  `itemId`: Not used

  `inputList`: NA
+ **Required datasets:** Interactions 
+ **Required event types:** At minimum, 1000 `View` events\.

## Best sellers<a name="best-sellers-use-case"></a>

Get recommendations for popular items based on how many times that your customers purchased an item\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-ecomm\-popular\-items\-by\-purchases
+ **GetRecommendations requirements: **

  `userId`: Required

  `itemId`: Not used

  `inputList`: NA
+ **Required datasets:** Interactions 
+ **Required event types:** At minimum, 1000 `Purchase` events\.

## Frequently bought together<a name="frequently-bought-together-use-case"></a>

Get recommendations for items that customers frequently buy together along with an item that you specify\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-ecomm\-frequently\-bought\-together
+ **GetRecommendations requirements: **

  `userId`: Required only if you filter by CurrentUser

  `itemId`: Required

  `inputList`: NA
+ **Required datasets:** Interactions 
+ **Required event types:** At minimum, 1000 `Purchase` events\.

## Customers who viewed X also viewed<a name="customers-also-viewed-use-case"></a>

Get recommendations for items that customers also viewed based on an item that you specify\. With this use case, Amazon Personalize automatically filters items the user purchased based on the userId that you specify and `Purchase` events\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-ecomm\-customers\-who\-viewed\-x\-also\-viewed
+ **GetRecommendations requirements: **

  `userId`: Required

  `itemId`: Required

  `inputList`: NA
+ **Required datasets:** Interactions 
+ **Required event types:** At minimum, 1000 `View` events\.
+ **Recommended event types:** `Purchase` events\.

## Recommended for you<a name="recommended-for-you-use-case"></a>

Get personalized recommendations for items based on a user that you specify\. With this use case, Amazon Personalize automatically filters items the user purchased based on the userId that you specify and `Purchase` events\. For better performance, include `Purchase` events along with the required `View` events\. 

 When recommending items, this use case uses exploration\. Exploration involves testing different item recommendations to learn how users respond to items with very little interaction data\. You can configure exploration when you create your recommender\. 
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-ecomm\-recommended\-for\-you
+ **GetRecommendations requirements: **

  `userId`: Required

  `itemId`: Not used

  `inputList`: NA
+ **Required datasets:** Interactions 
+ **Required number of events:** At minimum, 1000 events\.
+ **Recommended event types:** `View` and `Purchase` events\.
+ **Exploration configuration parameters:** When you create a recommender, you can configure exploration with the following\.
  + Emphasis on exploring less relevant items \(for APIs, this is called explorationWeight in the [RecommenderConfig](API_RecommenderConfig.md)\): Configure how much to explore\. Specify a decimal value between 0 to 1\. The default is 0\.3\. The closer the value is to 1, the more exploration\. With more exploration, recommendations include more items with less interactions data or relevance\. At zero, no exploration occurs and recommendations are based on current data \(relevance\)\.
  + Exploration item age cutoff: Specify the maximum item age in days since the latest interaction\. This defines the scope of item exploration\. For example, if you enter 10, Amazon Personalize exploration includes only items with interactions data from the 10 days since the latest interaction in the dataset\.

    To increase the items Amazon Personalize considers during exploration, enter a greater value\. The minimum is 1 day and the default is 30 days\. Recommendations might include items without interactions data from outside this time frame\. This is because these items are relevant to the user and exploration didn't identify them\.