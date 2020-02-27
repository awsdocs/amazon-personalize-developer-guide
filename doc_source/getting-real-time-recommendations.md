# Getting Real\-Time Recommendations<a name="getting-real-time-recommendations"></a>

Amazon Personalize can supply real\-time recommendations and personalized rankings from a campaign\. For example, suppose you have a campaign that is designed to give movie recommendations\. You can use the following operations to give real\-time movie recommendations to users signed into your application or website\. For an example using the AWS CLI, see [Step 4: Get Recommendations](getting-started-cli.md#gs-test)\.

**Topics**
+ [GetRecommendations](#recommendations)
+ [GetPersonalizedRankings](#rankings)

## GetRecommendations<a name="recommendations"></a>

To get recommendations, call the [GetRecommendations](API_RS_GetRecommendations.md) API\. Supply either the user ID or item ID, dependent on the recipe type used to create the solution the campaign is based on\.

To get situational recommendations, you can also include contextual metadata on your user\. For instance, you might include information on the user's current location or device \(desktop, mobile, tablet\) so that Amazon Personalize can get recommendations based on that user's previous situational behavior\. Any metadata context fields must be included in the schema of the campaign's user\-item interaction dataset\.

**Note**  
The solution backing the campaign must have been created using a recipe of type USER\_PERSONALIZATION or RELATED\_ITEMS\. For more information, see [Using Predefined Recipes](working-with-predefined-recipes.md)\.

**Get the recommendations using the AWS Python SDK**

Use the following code to get a recommendation\. Change the value of `userId` to a user ID that is in the data you used to train the solution\. A list of recommended items for the user is displayed\.

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

Use the following code to get a recommendation based on contextual metadata\. Change the value of the context key\-value pair to that of a metadata field that is your training data\. A list of recommended items for the user is displayed\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_recommendations(
    campaignArn = 'Campaign ARN',
    userId = 'User ID',
    context = {
      'key': 'value'
    }
)

print("Recommended items")
for item in response['itemList']:
    print (item['itemId'])
```

## GetPersonalizedRankings<a name="rankings"></a>

A personalized ranking is a list of recommended items that are re\-ranked for a specific user\. To get personalized rankings, call the [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) API\.

**Note**  
The solution backing the campaign must have been created using a recipe of type PERSONALIZED\_RANKING\. For more information, see [Using Predefined Recipes](working-with-predefined-recipes.md)\.

**Get personalized rankings using the AWS Python SDK**

Use the following code to get a personalized ranking\. Change the value of `userId` and `inputList` to a user ID and list of item IDs that are in the data you used to train the solution\. A list of ranked recommendations is displayed\. The first item in the list is considered by Amazon Personalize to be of most interest to the user\.

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

Use the following code to get a personalized ranking based on contextual metadata\. Change the value of the context key\-value pair to that of a metadata field that is in your training data\. The first item in the list is considered by Amazon Personalize to be of most interest to the user\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_personalized_ranking(
    campaignArn = "Campaign arn",
    userId = "UserID",
    inputList = ['ItemID1', 'ItemID2'],
    context = {
      'key': 'value'
    }
)

print("Personalized Ranking")
for item in response['personalizedRanking']:
  print(item['itemId'])
```