# Getting Recommendations<a name="getting-recommendations"></a>

Amazon Personalize can supply recommendations and personalized rankings from a campaign\. For example, suppose you have a campaign that is designed to give movie recommendations\. You can use the following operations to give movie recommendations to users signed into your application or website\. For an example using the AWS CLI, see [Step 4: Get Recommendations](getting-started-cli.md#gs-test)\.

**Topics**
+ [Get Recommendations](#recommendations)
+ [Get Personalized Rankings](#rankings)

## Get Recommendations<a name="recommendations"></a>

After you have created a campaign, you can use it in your applications to get recommendations\.

To get recommendations, call the [GetRecommendations](API_RS_GetRecommendations.md) API\. Supply either the user ID or item ID, dependent on the recipe type used to create the solution the campaign is based on\.

**Note**  
The solution backing the campaign must have been created using a recipe of type USER\_PERSONALIZATION or RELATED\_ITEMS\. For more information, see [Using Predefined Recipes](working-with-predefined-recipes.md)\.

**Get recommendations using the AWS Python SDK**

Use the following code to get a recommendation\. Change the value of `userId` to a user ID that is in the data you used to train the solution\. A list of recommended items for the user is displayed\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_recommendations(
    campaignArn = "Campaign ARN",
    userId = 'User ID')

print("Recommended items")
for item in response['itemList']:
    print (item['itemId'])
```

## Get Personalized Rankings<a name="rankings"></a>

A personalized ranking is a list of recommended items that are re\-ranked for a specific user\. To get personalized rankings, call the [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) API\.

**Note**  
The solution backing the campaign must have been created using a recipe of type PERSONALIZED\_RANKING\. For more information, see [Using Predefined Recipes](working-with-predefined-recipes.md)\.

**Get a personalized rankings using the AWS Python SDK**

Use the following code to get a personalized ranking\. Change the value of `userId` and `inputList` to a user ID and list of item IDs that are in the data you used to train the solution\. A list of ranked recommendations is displayed\. The first item in the list is considered by Amazon Personalize to be of most interest to the user\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_personalized_ranking(
    campaignArn = "Campaign arn",
    userId = 'UserID',
    inputList = ['ItemID1','ItemID2'])

print("Personalized Ranking")
for item in response['personalizedRanking']:
    print (item['itemId'])
```