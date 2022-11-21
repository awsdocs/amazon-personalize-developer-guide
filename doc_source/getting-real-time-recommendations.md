# Getting real\-time recommendations<a name="getting-real-time-recommendations"></a>

You get real\-time recommendations from Amazon Personalize with a campaign\. For example, suppose you have a campaign that deploys a solution to give movie recommendations\. Depending on the recipe you used to create the solution version backing the campaign, you get recommendations for your users with the [GetRecommendations](API_RS_GetRecommendations.md) or [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) API operations or with the Amazon Personalize console\. For information on campaigns see [Creating a campaign](campaigns.md)\. If your campaign recommends similar items, you get recommendations with the [GetRecommendations](API_RS_GetRecommendations.md) operation or with the Amazon Personalize console\. Amazon Personalize returns a list of related items based on the item ID you include in the request\.

 When you get personalized recommendations or similar items, you can specify a promotion in your request\. A *promotion* defines additional business rules that apply to a configurable subset of recommended items\. For more information see [Promoting items in recommendations \(Custom dataset group\)](promoting-items.md)\. 

 If you don't need recommendations in real\-time, you can get batch recommendations without a campaign\. For more information see [Getting batch recommendations and user segments](recommendations-batch.md)\. 

 

**Topics**
+ [Getting recommendations](recommendations.md)
+ [Getting a personalized ranking](rankings.md)
+ [Increasing recommendation relevance with contextual metadata](contextual-metadata.md)