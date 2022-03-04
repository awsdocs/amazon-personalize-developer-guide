# VIDEO\_ON\_DEMAND use cases<a name="VIDEO_ON_DEMAND-use-cases"></a>

The following sections list the requirements and Amazon Resource Name \(ARN\) for each VIDEO\_ON\_DEMAND use case\. 

**Note**  
If you use the [CreateRecommender](API_CreateRecommender.md) API, provide the ARN listed here for the recipe ARN\.

**Topics**
+ [Most popular](#hot-picks-use-case)
+ [Because you watched X](#because-you-watched-use-case)
+ [More like X](#more-like-y-use-case)
+ [Top picks for you](#top-picks-use-case)

## Most popular<a name="hot-picks-use-case"></a>

Get recommendations for videos that have been watched by the most users\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-most\-popular
+ **GetRecommendations requirements:**

  `userId`: Required

  `itemId`: Not used
+ **Required datasets:** Interactions dataset
+ **Required event types:** At minimum, 1000 `Watch` events\.

## Because you watched X<a name="because-you-watched-use-case"></a>

Get recommendations for videos that other users also watched based on a video that you specify\. With this use case, Amazon Personalize automatically filters videos the user watched based on the userId you specify\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-because\-you\-watched\-x
+ **GetRecommendations API requirements: **

  `userId`: Required

  `itemId`: Required
+ **Required datasets:** Interactions dataset
+ **Required event types:** At minimum, 1000 `Watch` events\.

## More like X<a name="more-like-y-use-case"></a>

Get recommendations for videos that are similar to a video that you specify\. With this use case, Amazon Personalize automatically filters videos the user watched based on the userId that you specify and `Watch` events\. For better performance, record `Click` events in addition to the required `Watch` events
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-more\-like\-x
+ **GetRecommendations API requirements: **

  `userId`: Required

  `itemId`: Required
+ **Required datasets:** Interactions dataset and Items dataset
+ **Required event types:** At minimum, 1000 `Watch` events\.

  **Recommended additional event types:** `Click` events\.

## Top picks for you<a name="top-picks-use-case"></a>

Get personalized content recommendations for a user that you specify\. With this use case, Amazon Personalize automatically filters videos the user watched based on the userId that you specify and `Watch` events\.
+ **Recipe ARN:** arn:aws:personalize:::recipe/aws\-vod\-top\-picks
+ **GetRecommendations requirements: **

  `userId`: Required

  `itemId`: Not used
+ **Required datasets:** Interactions dataset and Items dataset
+ **Required event types:** At minimum, 1000 `Watch` events\.

  **Recommended additional event types:** `Click` events\.