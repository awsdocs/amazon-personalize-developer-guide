# Choosing recommender use cases<a name="domain-use-cases"></a>

 Each domain has different use cases\. When you create a recommender you create it for a specific use case, and each use case has different requirements for getting recommendations\. 

**Topics**
+ [VIDEO\_ON\_DEMAND use cases](#VIDEO_ON_DEMAND-use-cases)
+ [ECOMMERCE use cases](#ECOMMERCE-use-cases)

## VIDEO\_ON\_DEMAND use cases<a name="VIDEO_ON_DEMAND-use-cases"></a>

The following sections list the requirements and Amazon Resource Name \(ARN\) for each VIDEO\_ON\_DEMAND use case\. 

**Note**  
If you use the [CreateRecommender](API_CreateRecommender.md) API, provide the ARN listed here for the recipe ARN\.

**Topics**
+ [Most popular](#hot-picks-use-case)
+ [Because you watched X](#because-you-watched-use-case)
+ [More like X](#more-like-y-use-case)
+ [Top picks for you](#top-picks-use-case)

### Most popular<a name="hot-picks-use-case"></a>

Get recommendations for streaming content that is trending\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-most\-popular
+ **GetRecommendations requirements:**

  `userId`: Required

  `itemId`: Not used
+ **Required datasets:** Interactions dataset
+ **Required event types:** `Watch` events\.

### Because you watched X<a name="because-you-watched-use-case"></a>

Get recommendations for videos that are similar to a video a user watched\. With this use case, Amazon Personalize automatically filters videos the user watched based on the userId you specify\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-because\-you\-watched\-x
+ **GetRecommendations API requirements: **

  `userId`: Required

  `itemId`: Required
+ **Required datasets:** Interactions dataset
+ **Required event types:** `Watch` events\.

### More like X<a name="more-like-y-use-case"></a>

Get recommendations for videos that are similar to a video that you specify\. With this use case, Amazon Personalize automatically filters videos the user watched based on the userId that you specify and `Watch` events\. For better performance, record `Click` events in addition to the required `Watch` events
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-more\-like\-x
+ **GetRecommendations API requirements: **

  `userId`: Required

  `itemId`: Required
+ **Required datasets:** Interactions dataset and Items dataset
+ **Required event types:** `Watch` events\.

  **Recommended additional event types:** `Click` events\.

### Top picks for you<a name="top-picks-use-case"></a>

Get personalized streaming content recommendations for a user that you specify\. With this use case, Amazon Personalize automatically filters videos the user watched based on the userId that you specify and `Watch` events\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-top\-picks
+ **GetRecommendations requirements: **

  `userId`: Required

  `itemId`: Not used
+ **Required datasets:** Interactions dataset and Items dataset
+ **Required event types:** `Watch` events\.

  **Recommended additional event types:** `Click` events\.

## ECOMMERCE use cases<a name="ECOMMERCE-use-cases"></a>

The following sections list the requirements and Amazon Resource Name \(ARN\) for each ECOMMERCE use case\.

**Note**  
If you use the [CreateRecommender](API_CreateRecommender.md) API, provide the ARN listed here for the recipe ARN\.

**Topics**
+ [Most viewed](#most-viewed-use-case)
+ [Best sellers](#best-sellers-use-case)
+ [Frequently bought together](#frequently-bought-together-use-case)
+ [Customers who viewed X also viewed](#customers-also-viewed-use-case)
+ [Recommended for you](#recommended-for-you-use-case)

### Most viewed<a name="most-viewed-use-case"></a>

Get recommendations for popular items based on how many times your customers viewed an item\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-ecomm\-popular\-items\-by\-views
+ **GetRecommendations requirements: **

  `userId`: Required

  `itemId`: Not used

  `inputList`: NA
+ **Required datasets:** Interactions dataset
+ **Required event types:** `View` events\.

### Best sellers<a name="best-sellers-use-case"></a>

Get recommendations for popular items based on how many times your customers purchased an item\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-ecomm\-popular\-items\-by\-purchases
+ **GetRecommendations requirements: **

  `userId`: Required

  `itemId`: Not used

  `inputList`: NA
+ **Required datasets:** Interactions dataset
+ **Required event types:** `Purchase` events\.

### Frequently bought together<a name="frequently-bought-together-use-case"></a>

Get recommendations for items that customers frequently buy together based on an item that you specify\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-ecomm\-frequently\-bought\-together
+ **GetRecommendations requirements: **

  `userId`: Not used

  `itemId`: Required

  `inputList`: NA
+ **Required datasets:** Interactions dataset
+ **Required event types:** `Purchase` events\.

### Customers who viewed X also viewed<a name="customers-also-viewed-use-case"></a>

Get recommendations for items that customers also viewed based on an item that you specify\. With this use case, Amazon Personalize automatically filters items the user purchased based on the userId that you specify and `Purchase` events\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-ecomm\-customers\-who\-viewed\-x\-also\-viewed
+ **GetRecommendations requirements: **

  `userId`: Required

  `itemId`: Required

  `inputList`: NA
+ **Required datasets:** Interactions dataset
+ **Required event types:** `View` events\.

  **Recommended additional event types:** `Purchase` events\.

### Recommended for you<a name="recommended-for-you-use-case"></a>

Get personalized recommendations for items based on a user that you specify\. With this use case, Amazon Personalize automatically filters items the user purchased based on the userId that you specify and `Purchase` events\. For better performance, include `Purchase` events along with the required `View` events\. 
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-ecomm\-recommended\-for\-you
+ **GetRecommendations requirements: **

  `userId`: Required

  `itemId`: Not used

  `inputList`: NA
+ **Required datasets:** Interactions dataset
+ **Required event types:** `View` events\.

  **Recommended additional event types:** `Purchase` events\.