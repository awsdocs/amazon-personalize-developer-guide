# Getting recommendations<a name="recommendations"></a>

 You get personalized recommendations or similar item recommendations from an Amazon Personalize campaign\. You can get recommendations with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\. 

**Note**  
 If you used a PERSONALIZED\_RANKING recipe, see [Getting a personalized ranking](rankings.md)\. 

**Topics**
+ [How User\-Personalization recommendation scoring works](#how-recommendation-scoring-works)
+ [Getting recommendations \(console\)](#get-real-time-recommendations-console)
+ [Getting recommendations \(AWS CLI\)](#get-recommendations-cli-example)
+ [Getting recommendations \(AWS SDKs\)](#get-recommendations-sdk-example)
+ [Getting recommendations using contextual metadata \(AWS Python SDK\)](#get-recommendations-metadata-sdk-example)

## How User\-Personalization recommendation scoring works<a name="how-recommendation-scoring-works"></a>

With the User\-Personalization recipe, Amazon Personalize generates scores for items based on on a user's interaction data and metadata\. These scores represent the relative certainty that Amazon Personalize has in whether the user will interact with the item next\. Higher scores represent greater certainty\.

Amazon Personalize scores all of the items in your catalog relative to each other on a scale from 0 to 1 \(both inclusive\), so that the total of all scores equals 1\. For example, if you're getting movie recommendations for a user and there are three movies in the Items dataset, their scores might be `0.6`, `0.3`, and `0.1`\. Similarly, if you have 1,000 movies in your inventory, the highest\-scoring movies might have very small scores \(the average score would be`.001`\), but, because scoring is relative, the recommendations are still valid\.

In mathematical terms, scores for each user\-item pair \(u,i\) are computed according to the following formula, where `exp` is the exponential function, w̅u and wi/j are user and item embeddings respectively, and the Greek letter sigma \(Σ\) represents summation over all items in the item dataset:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/get_recommendations_score.png)

**Note**  
Amazon Personalize doesn't show scores for Similar\-Items, SIMS or Popularity\-Count recipes\. For information on scores for Personalized\-Ranking recommendations, see [How personalized ranking scoring works](rankings.md#how-ranking-scoring-works)\.

## Getting recommendations \(console\)<a name="get-real-time-recommendations-console"></a>

 To get recommendations with the Amazon Personalize console, provide the recommendation request information on the details page for your Campaign\. 

**To get recommendations for a user**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\. 

1. Choose the dataset group that contains the campaign you are using\.

1. In the navigation pane, choose **Campaigns**\.

1. On the **Campaigns** page, choose the target campaign\.

1.  Under **Test campaign results**, enter your recommendation request details based on the recipe you used\. For USER\_PERSONALIZATION recipes, enter the **User ID** of the user that you want to get recommendations for\. For RELATED\_ITEMS recipes, enter the **Item ID** of the item you want Amazon Personalize to use to determine similar items\. 

    If you recorded events for a user before they logged in \(an anonymous user\), you can get recommendations for this user by providing the `sessionId` from those events instead of a `userId`\. For more information about recording events for anonymous users, see [Recording events with the PutEvents operation](recording-events.md#event-record-api)\. 

1. Optionally choose a filter\. For more information, see [Filtering recommendations and user segments](filter.md)\. 

1. If your campaign uses contextual metadata \(for requirements see [Increasing recommendation relevance with contextual metadata](getting-real-time-recommendations.md#contextual-metadata)\) optionally provide context data\. 

   For each context, for the **Key**, enter the metadata field, and for the **Value**, enter the context data\. 

1. Choose **Get recommendations**\. A table containing the user’s top 25 recommended items displays\. 

## Getting recommendations \(AWS CLI\)<a name="get-recommendations-cli-example"></a>

Use the following code to get recommendations\. Change the value of `userId` to a user ID that is in the data that you used to train the solution\. A list of the top 10 recommended items for the user displays\. To change the number of recommended items, change the value for `numResults`\. The default is 25 items\. The maximum is 500 items\. If you used a RELATED\_ITEMS recipe to train the solution version backing the campaign, replace the `user-id` parameter with `item-id` and specify the item ID\. 

 If you recorded events for a user before they logged in \(an anonymous user\), you can get recommendations for this user by providing the `sessionId` from those events instead of a `userId`\. For more information about recording events for anonymous users, see [Recording events with the PutEvents operation](recording-events.md#event-record-api)\. 

```
aws personalize-runtime get-recommendations \
--campaign-arn campaign arn \
--user-id User ID \
--num-results 10
```

## Getting recommendations \(AWS SDKs\)<a name="get-recommendations-sdk-example"></a>

The following code shows how to get Amazon Personalize recommendations using the SDK for Python \(Boto3\) or SDK for Java 2\.x\. Change the value of `userId` to a user ID that is in the data that you used to train the solution\. A list of the top 10 recommended items for the user displays\. To change the number of recommended items, change the value for `numResults`\. The default is 25 items\. The maximum is 500 items\. If you used a RELATED\_ITEMS recipe to train the solution version backing the campaign, replace the `userId` parameter with `itemId` and specify the item ID\. 

 If you recorded events for a user before they logged in \(an anonymous user\), you can get recommendations for this user by providing the `sessionId` from those events instead of a `userId`\. For more information about recording events for anonymous users, see [Recording events with the PutEvents operation](recording-events.md#event-record-api)\. 

------
#### [ SDK for Python \(Boto3\) ]

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

## Getting recommendations using contextual metadata \(AWS Python SDK\)<a name="get-recommendations-metadata-sdk-example"></a>

Use the following code to get a recommendation based on contextual metadata\. For `context`, for each key\-value pair, provide the metadata field as the key and the context data as the value\. In the following sample code, the key is `DEVICE` and the value is `mobile phone`\. Replace these values and the `Campaign ARN` and `User ID` with your own\. A list of recommended items for the user displays\.

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