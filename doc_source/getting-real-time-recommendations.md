# Getting real\-time recommendations<a name="getting-real-time-recommendations"></a>

You get real\-time recommendations from Amazon Personalize with a campaign\. For example, suppose you have a campaign that deploys a solution to give movie recommendations\. Depending on the recipe you used to create the solution version backing the campaign, you get recommendations for your users with the [GetRecommendations](API_RS_GetRecommendations.md) or [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) API operations or with the Amazon Personalize console\. For information on campaigns see [Creating a campaign](campaigns.md)\. If you don't need recommendations in real\-time, you can get batch recommendations without a campaign\. For more information see [Getting batch recommendations and user segments](recommendations-batch.md)\. 

 If your campaign recommends similar items, you get recommendations with the [GetRecommendations](API_RS_GetRecommendations.md) operation or with the Amazon Personalize console\. Amazon Personalize returns a list of related items based on the item ID you include in the request\. 

**Topics**
+ [Increasing recommendation relevance with contextual metadata](#contextual-metadata)
+ [Getting recommendations](recommendations.md)
+ [Getting a personalized ranking](rankings.md)

## Increasing recommendation relevance with contextual metadata<a name="contextual-metadata"></a>

To increase recommendation relevance, include contextual metadata for a user, such as their device type or the time of day, when you get recommendations or get a personalized ranking\. 

To use contextual metadata, you must meet the following requirements: 
+ The schema of the Interactions dataset must have contextual metadata fields \(see [Datasets and schemas](how-it-works-dataset-schema.md)\)\. 
+ The solution version backing the campaign must use a recipe of type USER\_PERSONALIZATION or PERSONALIZED\_RANKING \(see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\)\. 

 For more information about the benefits of including contextual metadata, see [Contextual metadata](interactions-datasets.md#interactions-contextual-metadata)\. 

 For examples that show how to include contextual metadata using the SDK for Python \(Boto3\)\. see [Getting recommendations using contextual metadata \(AWS Python SDK\)](recommendations.md#get-recommendations-metadata-sdk-example) and [Getting a personalized ranking using contextual metadata \(AWS Python SDK\)](rankings.md#personalized-ranking-contextual-metadata-example)\. 