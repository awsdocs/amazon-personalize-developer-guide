# Getting Real\-Time Recommendations<a name="getting-real-time-recommendations"></a>

You can get real\-time recommendations from Amazon Personalize with a campaign\. For example, suppose you have a campaign that is designed to give movie recommendations\. You can use the [GetRecommendations](API_RS_GetRecommendations.md) or [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) API operations to get real\-time movie recommendations for users signed into your application or website\. You can also get recommendations using Amazon Personalize console, where the top recommendations for the user appear in a table on the details page for the campaign\. 

**Topics**
+ [Increasing Recommendation Relevance with Contextual Metadata](#contextual-metadata)
+ [Getting Recommendations](#recommendations)
+ [Getting a Personalized Ranking](#rankings)

## Increasing Recommendation Relevance with Contextual Metadata<a name="contextual-metadata"></a>

To increase recommendation relevance, include contextual metadata for a user, such as their device type or the time of day, when you get recommendations or get a personalized ranking\. 

To use contextual metadata, you must meet the following requirements: 
+ The schema of the Interactions dataset must have contextual metadata fields \(see [Datasets and Schemas](how-it-works-dataset-schema.md)\)\. 
+ The solution version backing the campaign must use a recipe of type USER\_PERSONALIZATION or RELATED\_ITEMS \(see [Step 1: Choosing a Recipe](working-with-predefined-recipes.md)\)\. 

 For more information about the benefits of including contextual metadata, see [Contextual Metadata](interactions-datasets.md#interactions-contextual-metadata)\. 

 For examples that show how to include contextual metadata using the AWS SDK for Python see [Getting Recommendations using Contextual Metadata \(AWS Python SDK\)](#get-recommendations-metadata-sdk-example) and [Getting a Personalized Ranking using Contextual Metadata \(AWS Python SDK\)](#personalized-ranking-contextual-metadata-example)\. 

## Getting Recommendations<a name="recommendations"></a>

To get recommendations, call the [GetRecommendations](API_RS_GetRecommendations.md) API operation, or get recommendations for a user on the campaign details page in the console\. For an example using the AWS CLI, see [Step 4: Get Recommendations](getting-started-cli.md#gs-test)\.

**How Scoring Works**

To make recommendations, Amazon Personalize generates scores for the items in your Items dataset based on a user's interaction data and metadata\. These scores represent the relative certainty that Amazon Personalize has in which item the user will select next\. Higher scores represent greater certainty\.

Models that are based on USER\_PERSONALIZATION recipes score all of the items in your Items dataset relative to each other on a scale from 0 to 1 \(both inclusive\), so that the total of all scores equals 1\. For example, if you're getting movie recommendations for a user and there are three movies in the Items dataset, their scores might be `0.6`, `0.3`, and `0.1`\. Similarly, if you have 1,000 movies in your inventory, the highest\-scoring movies might have very small scores \(the average score would be`.001`\), but, because scoring is relative, the recommendations are still valid\.

In mathematical terms, scores for each user\-item pair \(u,i\) are computed according to the following formula, where “exp” is the exponential function, w̅ u and wi/j are user and item embeddings respectively, and the Greek letter sigma \(Σ\) represents summation over all items in the item dataset:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/get_recommendations_score.png)

**Note**  
Amazon Personalize doesn't show scores for SIMS or Popularity\-Count\-based models\.

### Getting Recommendations \(Console\)<a name="get-recommendations-console"></a>

To get recommendations for a user in the Amazon Personalize console, choose the campaign that you are using and then provide their User ID, optionally choose a filter, and optionally provide any context data\.

**To get recommendations for a user**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/](https://console.aws.amazon.com/personalize/) and sign into your account\. 

1. Choose the dataset group that contains the campaign you are using\.

1. In the navigation pane, choose **Campaigns**\.

1. On the **Campaigns** page, choose the target campaign\.

1.  Under **Test campaign results**, enter the **User ID** of the user that you want to get recommendations for\. 

1. Optionally choose a filter\. For more information, see [Filtering Recommendations](filter.md)\. 

1. If your campaign uses contextual metadata \(for requirements see [Contextual Metadata](#contextual-metadata)\) optionally provide context data\. 

   For each context, for the **Key**, enter the metadata field, and for the **Value**, enter the context data\. 

1. Choose **Get recommendations**\. A table containing the user’s top recommendations appears\. 

### Getting Recommendations \(AWS Python SDK\)<a name="get-recommendations-sdk-example"></a>

Use the following code to get a recommendation\. Change the value of `userId` to a user ID that is in the data that you used to train the solution\. A list of recommended items for the user is displayed\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_recommendations(
    campaignArn = 'Campaign ARN',
    userId = 'User ID')

print("Recommended items")
for item in response['itemList']:
    print (item['itemId'])
```

### Getting Recommendations using Contextual Metadata \(AWS Python SDK\)<a name="get-recommendations-metadata-sdk-example"></a>

Use the following code to get a recommendation based on contextual metadata\. For `context`, for each key\-value pair, provide the metadata field as the key and the context data as the value\. In the following sample code, the key is `DEVICE` and the value is `mobile phone`\. Replace these values and the `Campaign ARN` and `User ID` with your own\. A list of recommended items for the user is displayed\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_recommendations(
    campaignArn = 'Campaign ARN',
    userId = 'User ID',
    context = {
      'DEVICE': 'mobile phone'
    }
)

print("Recommended items")
for item in response['itemList']:
    print (item['itemId'])
```

## Getting a Personalized Ranking<a name="rankings"></a>

A personalized ranking is a list of recommended items that are re\-ranked for a specific user\. To get personalized rankings, call the [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) API operation or get recommendations from a campaign in the console\.

**Note**  
The solution backing the campaign must have been created using a recipe of type PERSONALIZED\_RANKING\. For more information, see [Step 1: Choosing a Recipe](working-with-predefined-recipes.md)\.

**How Scoring Works**

Like the scores returned by the `GetRecommendations` operation, `GetPersonalizedRanking` scores sum to 1, but because the list of considered items is much smaller than your full Items dataset, recommendation scores tend to be higher\.

Mathematically, the scoring function for GetPersonalizedRanking is identical to `GetRecommendations`, except that it only considers the input items\. This means that scores closer to 1 become more likely, as there are fewer other choices to divide up the score:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/get_personalized_ranking.png)

### Getting a Personalized Ranking \(Console\)<a name="get-recommendations-console"></a>

To get a personalized ranking for a user from the Amazon Personalize console, choose the campaign that you are using and then provide their user ID, specify the list of items you want ranked for the user, optionally choose a filter, and optionally provide any context data\. 

**To get a personalized ranking for a user**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/](https://console.aws.amazon.com/personalize/) and sign into your account\. 

1. Choose the dataset group that contains the campaign you are using\.

1. In the navigation pane, choose **Campaigns**\.

1. On the **Campaigns** page, choose the target campaign\.

1.  Under **Test campaign results**, enter the **User ID** of the user that you want to get recommendations for\. 

1. For **Item IDs**, enter the list of items to be ranked for the user\.

1. Optionally choose a filter\. For more information, see [Filtering Recommendations](filter.md)\. 

1. If your campaign uses contextual metadata \(for requirements see [Contextual Metadata](#contextual-metadata)\) optionally provide context data\. 

   For each context, for the **Key**, enter the metadata field, and for the **Value**, enter the context data\. 

1. Choose **Get personalized item rankings**\. A table containing the items ranked in order of predicted interest for the user appears\. 

### Getting a Personalized Ranking \(AWS Python SDK\)<a name="get-personalized-rankings-sdk"></a>

Use the following code to get a personalized ranking\. Change the value of `userId` and `inputList` to a user ID and list of item IDs that are in the data that you used to train the solution\. A list of ranked recommendations is displayed\. Amazon Personalize considers the first item in the list of most interest to the user\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_personalized_ranking(
    campaignArn = "Campaign arn",
    userId = "UserID",
    inputList = ['ItemID1','ItemID2'])

print("Personalized Ranking")
for item in response['personalizedRanking']:
    print (item['itemId'])
```

### Getting a Personalized Ranking using Contextual Metadata \(AWS Python SDK\)<a name="personalized-ranking-contextual-metadata-example"></a>

Use the following code to get a personalized ranking based on contextual metadata\. For `context`, for each key\-value pair, provide the metadata field as the key and the context data as the value\. In the following sample code, the key is `DEVICE` and the value is `mobile phone`\. Replace these values and the `Campaign ARN` and `User ID` with your own\. Also change `inputList` to a list of item IDs that are in the data that you used to train the solution\. Amazon Personalize considers the first item in the list of most interest to the user\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_personalized_ranking(
    campaignArn = "Campaign ARN",
    userId = "User ID",
    inputList = ['ItemID1', 'ItemID2'],
    context = {
      'DEVICE': 'mobile phone'
    }
)

print("Personalized Ranking")
for item in response['personalizedRanking']:
  print(item['itemId'])
```

### Personalized\-Ranking Sample Notebook<a name="real-time-recommendations-personalized-ranking-example"></a>

 For a sample Jupyter notebook that shows how to use the Personalized\-Ranking recipe see [Personalize Ranking Example](https://github.com/aws-samples/amazon-personalize-samples/blob/master/next_steps/core_use_cases/personalized_ranking/personalize_ranking_example.ipynb)\. 