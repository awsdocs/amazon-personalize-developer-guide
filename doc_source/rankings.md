# Getting a personalized ranking<a name="rankings"></a>

A personalized ranking is a list of recommended items that are re\-ranked for a specific user\. To get personalized rankings, call the [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) API operation or get recommendations from a campaign in the console\.

**Note**  
The solution backing the campaign must have been created using a recipe of type PERSONALIZED\_RANKING\. For more information, see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\.

**Topics**
+ [How personalized ranking scoring works](#how-ranking-scoring-works)
+ [Getting a personalized ranking \(console\)](#get-ranking-recommendations-console)
+ [Getting a personalized ranking \(AWS CLI\)](#get-personalized-rankings-cli)
+ [Getting a personalized ranking \(AWS SDKs\)](#get-personalized-rankings-sdk)
+ [Getting a personalized ranking using contextual metadata \(AWS Python SDK\)](#personalized-ranking-contextual-metadata-example)
+ [Personalized\-Ranking sample notebook](#real-time-recommendations-personalized-ranking-example)

## How personalized ranking scoring works<a name="how-ranking-scoring-works"></a>

Like the scores returned by the `GetRecommendations` operation for solutions created with the [User\-Personalization](recommendations.md#how-recommendation-scoring-works) recipe, `GetPersonalizedRanking` scores sum to 1, but because the list of considered items is much smaller than your full catalog, recommendation scores tend to be higher\.

Mathematically, the scoring function for GetPersonalizedRanking is identical to `GetRecommendations`, except that it only considers the input items\. This means that scores closer to 1 become more likely, as there are fewer other choices to divide up the score:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/get_personalized_ranking.png)

## Getting a personalized ranking \(console\)<a name="get-ranking-recommendations-console"></a>

To get a personalized ranking for a user from the Amazon Personalize console, choose the campaign that you are using and then provide their user ID, specify the list of items you want ranked for the user, optionally choose a filter, and optionally provide any context data\. 

**To get a personalized ranking for a user**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\. 

1. Choose the dataset group that contains the campaign you are using\.

1. In the navigation pane, choose **Campaigns**\.

1. On the **Campaigns** page, choose the target campaign\.

1.  Under **Test campaign results**, enter the **User ID** of the user that you want to get recommendations for\. 

1. For **Item IDs**, enter the list of items to be ranked for the user\.

1. Optionally choose a filter\. For more information, see [Filtering recommendations and user segments](filter.md)\. 

1. If your campaign uses contextual metadata \(for requirements see [Increasing recommendation relevance with contextual metadata](contextual-metadata.md)\) optionally provide context data\. 

   For each context, for the **Key**, enter the metadata field, and for the **Value**, enter the context data\. 

1. Choose **Get personalized item rankings**\. A table containing the items ranked in order of predicted interest for the user appears\. 

## Getting a personalized ranking \(AWS CLI\)<a name="get-personalized-rankings-cli"></a>

 Use the following `get-personalized-ranking` command to get a personalized ranking with the AWS CLI\. Specify the Amazon Resource Name \(ARN\) for your campaign, the User ID for the user, and provide a list of item IDs for the items to be ranked for the user \(each separated by a space\)\. The items to be ranked must be in the data that you used to train the solution version\. A list of ranked recommendations displays\. Amazon Personalize considers the first item in the list of most interest to the user\. 

```
aws personalize-runtime get-personalized-ranking \
--campaign-arn Campaign ARN \
--user-id 12 \
--input-list 3 4 10 8 12 7
```

## Getting a personalized ranking \(AWS SDKs\)<a name="get-personalized-rankings-sdk"></a>

The following code shows how to get a personalized ranking for a user\. Specify the user's ID and a list of item IDs to be ranked for the user\. The item IDs must be in the data that you used to train the solution version\. A list of ranked recommendations is returned\. Amazon Personalize considers the first item in the list of most interest to the user\.

------
#### [ SDK for Python \(Boto3\) ]

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
#### [ SDK for JavaScript v3 ]

```
// Get service clients module and commands using ES6 syntax.
import { GetPersonalizedRankingCommand } from
  "@aws-sdk/client-personalize-runtime";
import { personalizeRuntimeClient } from "./libs/personalizeClients.js";
// Or, create the client here.
// const personalizeRuntimeClient = new PersonalizeRuntimeClient({ region: "REGION"});

// Set the ranking request parameters.
export const getPersonalizedRankingParam = {
  campaignArn: "CAMPAIGN_ARN", /* required */
  userId: 'USER_ID',      /* required */
  inputList: ["ITEM_ID_1", "ITEM_ID_2", "ITEM_ID_3", "ITEM_ID_4"]
}

export const run = async () => {
  try {
    const response = await personalizeRuntimeClient.send(new GetPersonalizedRankingCommand(getPersonalizedRankingParam));
    console.log("Success!", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

------

## Getting a personalized ranking using contextual metadata \(AWS Python SDK\)<a name="personalized-ranking-contextual-metadata-example"></a>

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

## Personalized\-Ranking sample notebook<a name="real-time-recommendations-personalized-ranking-example"></a>

 For a sample Jupyter notebook that shows how to use the Personalized\-Ranking recipe see [Personalize Ranking Example](https://github.com/aws-samples/amazon-personalize-samples/blob/master/next_steps/core_use_cases/personalized_ranking/personalize_ranking_example.ipynb)\. 