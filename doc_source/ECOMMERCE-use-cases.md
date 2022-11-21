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
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-ecomm\-recommended\-for\-you
+ **GetRecommendations requirements: **

  `userId`: Required

  `itemId`: Not used

  `inputList`: NA
+ **Required datasets:** Interactions 
+ **Required number of events:** At minimum, 1000 events\.
+ **Recommended event types:** `View` and `Purchase` events\.