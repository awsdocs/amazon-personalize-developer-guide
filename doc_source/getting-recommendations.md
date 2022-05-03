# Getting recommendations \(Custom dataset group\)<a name="getting-recommendations"></a>

 With Amazon Personalize Custom dataset groups, you can get recommendations in real\-time or you can get batch recommendations\. For real\-time recommendations, you must create a campaign before you get recommendations\. For batch recommendations, you don't need to create a campaign\. For information on campaigns see [Creating a campaign](campaigns.md)\. 

 The following topics explain how and when to use each recommendation type\. 

**Topics**
+ [Recommendation scores](#how-scores-work)
+ [Getting real\-time recommendations](getting-real-time-recommendations.md)
+ [Getting batch recommendations and user segments](recommendations-batch.md)

## Recommendation scores<a name="how-scores-work"></a>

 With the User\-Personalization and Personalized\-Ranking recipes, Amazon Personalize includes a score for each item in recommendations\. These scores represent the relative certainty that Amazon Personalize has in which item the user will select next\. Higher scores represent greater certainty\. 

For information on scores for User\-Personalization, see [How User\-Personalization recommendation scoring works](recommendations.md#how-recommendation-scoring-works)\. For information on scores for Personalized\-Ranking recommendations, see [How personalized ranking scoring works](rankings.md#how-ranking-scoring-works)\. 

 For batch inference jobs, item scores are calculated just as described in [How User\-Personalization recommendation scoring works](recommendations.md#how-recommendation-scoring-works) and [How personalized ranking scoring works](rankings.md#how-ranking-scoring-works)\. You can view scores in the batch inference job's output JSON file\. 