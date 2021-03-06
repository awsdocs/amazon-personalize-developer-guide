# Getting real\-time recommendations<a name="getting-real-time-recommendations"></a>

You can get real\-time recommendations from Amazon Personalize with a campaign\. For example, suppose you have a campaign that is designed to give movie recommendations\. You can use the [GetRecommendations](API_RS_GetRecommendations.md) or [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) API operations to get real\-time movie recommendations for users signed into your application or website\. You can also get recommendations using Amazon Personalize console, where the top recommendations for the user appear in a table on the details page for the campaign\. 

**Topics**
+ [Increasing recommendation relevance with contextual metadata](#contextual-metadata)
+ [Getting recommendations](#recommendations)
+ [Getting a personalized ranking](#rankings)

## Increasing recommendation relevance with contextual metadata<a name="contextual-metadata"></a>

To increase recommendation relevance, include contextual metadata for a user, such as their device type or the time of day, when you get recommendations or get a personalized ranking\. 

To use contextual metadata, you must meet the following requirements: 
+ The schema of the Interactions dataset must have contextual metadata fields \(see [Datasets and schemas](how-it-works-dataset-schema.md)\)\. 
+ The solution version backing the campaign must use a recipe of type USER\_PERSONALIZATION or RELATED\_ITEMS \(see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\)\. 

 For more information about the benefits of including contextual metadata, see [Contextual metadata](interactions-datasets.md#interactions-contextual-metadata)\. 

 For examples that show how to include contextual metadata using the AWS SDK for Python see [Getting recommendations using contextual metadata \(AWS Python SDK\)](#get-recommendations-metadata-sdk-example) and [Getting a personalized ranking using contextual metadata \(AWS Python SDK\)](#personalized-ranking-contextual-metadata-example)\. 

## Getting recommendations<a name="recommendations"></a>

To get recommendations, call the [GetRecommendations](API_RS_GetRecommendations.md) API operation, or get recommendations for a user on the campaign details page in the console\. For an example using the AWS CLI, see [Step 4: Get recommendations](getting-started-cli.md#gs-test)\.

**How scoring works**

To make recommendations, Amazon Personalize generates scores for the items in your Items dataset based on a user's interaction data and metadata\. These scores represent the relative certainty that Amazon Personalize has in which item the user will select next\. Higher scores represent greater certainty\.

Models that are based on USER\_PERSONALIZATION recipes score all of the items in your Items dataset relative to each other on a scale from 0 to 1 \(both inclusive\), so that the total of all scores equals 1\. For example, if you're getting movie recommendations for a user and there are three movies in the Items dataset, their scores might be `0.6`, `0.3`, and `0.1`\. Similarly, if you have 1,000 movies in your inventory, the highest\-scoring movies might have very small scores \(the average score would be`.001`\), but, because scoring is relative, the recommendations are still valid\.

In mathematical terms, scores for each user\-item pair \(u,i\) are computed according to the following formula, where `exp` is the exponential function, w̅u and wi/j are user and item embeddings respectively, and the Greek letter sigma \(Σ\) represents summation over all items in the item dataset:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/get_recommendations_score.png)

**Note**  
Amazon Personalize doesn't show scores for SIMS or Popularity\-Count\-based models\.

### Getting recommendations \(console\)<a name="get-real-time-recommendations-console"></a>

To get recommendations for a user in the Amazon Personalize console, choose the campaign that you are using and then provide their User ID, optionally choose a filter, and optionally provide any context data\.

**To get recommendations for a user**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\. 

1. Choose the dataset group that contains the campaign you are using\.

1. In the navigation pane, choose **Campaigns**\.

1. On the **Campaigns** page, choose the target campaign\.

1.  Under **Test campaign results**, enter the **User ID** of the user that you want to get recommendations for\. 

1. Optionally choose a filter\. For more information, see [Filtering recommendations](filter.md)\. 

1. If your campaign uses contextual metadata \(for requirements see [Increasing recommendation relevance with contextual metadata](#contextual-metadata)\) optionally provide context data\. 

   For each context, for the **Key**, enter the metadata field, and for the **Value**, enter the context data\. 

1. Choose **Get recommendations**\. A table containing the user’s top 25 recommended items displays\. 

### Getting recommendations \(AWS SDKs\)<a name="get-recommendations-sdk-example"></a>

The following code shows how to get Amazon Personalize recommendations using the SDK for Python \(Boto3\) or SDK for Java 2\.x\.

------
#### [ SDK for Python \(Boto3\) ]

Use the following code to get recommendations\. Change the value of `userId` to a user ID that is in the data that you used to train the solution\. A list of the top 10 recommended items for the user displays\. To change the number of recommended items, change the value for `numResults`\. The default is 25 items\. The maximum is 500 items\. 

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_recommendations(
    campaignArn = 'Campaign ARN',
    userId = 'User ID',
    numResults = 10
)

print("Recommended items")
for item in response['itemList']:
    print (item['itemId'])
```

------
#### [ SDK for Java 2\.x ]

To get recommendations, use the following `getRecommendations` method and pass the following as parameters: A `PersonalizeRuntimeClient`, the Amazon Resource Name \(ARN\) of your campaign, and the `userId` of the user you want to get recommendations for\. 

```
    public static void getRecs(PersonalizeRuntimeClient personalizeRuntimeClient, String campaignArn, String userId){

        try {
            GetRecommendationsRequest recommendationsRequest = GetRecommendationsRequest.builder()
                .campaignArn(campaignArn)
                .numResults(20)
                .userId(userId)
                .build();

            GetRecommendationsResponse recommendationsResponse = personalizeRuntimeClient.getRecommendations(recommendationsRequest);
            List<PredictedItem> items = recommendationsResponse.itemList();

            for (PredictedItem item: items) {
                System.out.println("Item Id is : "+item.itemId());
                System.out.println("Item score is : "+item.score());
            }
        } catch (AwsServiceException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```

------

### Getting recommendations using contextual metadata \(AWS Python SDK\)<a name="get-recommendations-metadata-sdk-example"></a>

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

## Getting a personalized ranking<a name="rankings"></a>

A personalized ranking is a list of recommended items that are re\-ranked for a specific user\. To get personalized rankings, call the [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) API operation or get recommendations from a campaign in the console\.

**Note**  
The solution backing the campaign must have been created using a recipe of type PERSONALIZED\_RANKING\. For more information, see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\.

**How scoring works**

Like the scores returned by the `GetRecommendations` operation, `GetPersonalizedRanking` scores sum to 1, but because the list of considered items is much smaller than your full Items dataset, recommendation scores tend to be higher\.

Mathematically, the scoring function for GetPersonalizedRanking is identical to `GetRecommendations`, except that it only considers the input items\. This means that scores closer to 1 become more likely, as there are fewer other choices to divide up the score:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/get_personalized_ranking.png)

### Getting a personalized ranking \(console\)<a name="get-ranking-recommendations-console"></a>

To get a personalized ranking for a user from the Amazon Personalize console, choose the campaign that you are using and then provide their user ID, specify the list of items you want ranked for the user, optionally choose a filter, and optionally provide any context data\. 

**To get a personalized ranking for a user**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\. 

1. Choose the dataset group that contains the campaign you are using\.

1. In the navigation pane, choose **Campaigns**\.

1. On the **Campaigns** page, choose the target campaign\.

1.  Under **Test campaign results**, enter the **User ID** of the user that you want to get recommendations for\. 

1. For **Item IDs**, enter the list of items to be ranked for the user\.

1. Optionally choose a filter\. For more information, see [Filtering recommendations](filter.md)\. 

1. If your campaign uses contextual metadata \(for requirements see [Increasing recommendation relevance with contextual metadata](#contextual-metadata)\) optionally provide context data\. 

   For each context, for the **Key**, enter the metadata field, and for the **Value**, enter the context data\. 

1. Choose **Get personalized item rankings**\. A table containing the items ranked in order of predicted interest for the user appears\. 

### Getting a personalized ranking \(AWS SDKs\)<a name="get-personalized-rankings-sdk"></a>

The following code shows how to get a personalized ranking with the AWS SDK for Python \(Boto3\) or AWS SDK for Java 2\.x\.

------
#### [ SDK for Python \(Boto3\) ]

Use the following `get_personalized_ranking` method to get a personalized ranking with the SDK for Python \(Boto3\)\. Change the value of `userId` and `inputList` to a user ID and list of item IDs to be ranked for the user\. The item IDs must be in the data that you used to train the solution\. A list of ranked recommendations is displayed\. Amazon Personalize considers the first item in the list of most interest to the user\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_personalized_ranking(
    campaignArn = "Campaign arn",
    userId = "UserID",
    inputList = ['ItemID1','ItemID2']
)

print("Personalized Ranking")
for item in response['personalizedRanking']:
    print (item['itemId'])
```

------
#### [ SDK for Java 2\.x ]

Use the following `getRankedRecs` method to get a personalized ranking with the SDK for Java 2\.x\. Pass the following as parameters: an Amazon Personalize runtime client, your campaign's Amazon Resource Name \(ARN\), the user ID for the user, and a list of item IDs to be ranked for the user\. The item IDs must be in the data you used to train the solution\. The method returns the list of recommended items ranked from most relevant to least relevant\.

```
public static List<PredictedItem> getRankedRecs(PersonalizeRuntimeClient personalizeRuntimeClient,
                                                String campaignArn,
                                                String userId,
                                                ArrayList<String> items) {

    try {
        GetPersonalizedRankingRequest rankingRecommendationsRequest = GetPersonalizedRankingRequest.builder()
                .campaignArn(campaignArn)
                .userId(userId)
                .inputList(items)
                .build();
  
        GetPersonalizedRankingResponse recommendationsResponse =
                personalizeRuntimeClient.getPersonalizedRanking(rankingRecommendationsRequest);
        List<PredictedItem> rankedItems = recommendationsResponse.personalizedRanking();
        int rank = 1;
        for (PredictedItem item : rankedItems) {
            System.out.println("Item ranked at position " + rank + " details");
            System.out.println("Item Id is : " + item.itemId());
            System.out.println("Item score is : " + item.score());
            System.out.println("---------------------------------------------");
            rank++;
        }
        return rankedItems;
    } catch (PersonalizeRuntimeException e) {
        System.err.println(e.awsErrorDetails().errorMessage());
        System.exit(1);
    }
    return null;
}
```

------

### Getting a personalized ranking using contextual metadata \(AWS Python SDK\)<a name="personalized-ranking-contextual-metadata-example"></a>

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

### Personalized\-Ranking sample notebook<a name="real-time-recommendations-personalized-ranking-example"></a>

 For a sample Jupyter notebook that shows how to use the Personalized\-Ranking recipe see [Personalize Ranking Example](https://github.com/aws-samples/amazon-personalize-samples/blob/master/next_steps/core_use_cases/personalized_ranking/personalize_ranking_example.ipynb)\. 