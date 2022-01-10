# Getting recommendations from a recommender<a name="domain-dsg-recommendations"></a>

 With a Domain dataset group, after you create a recommender you can use it in your application to get real\-time recommendations with the [GetRecommendations](API_RS_GetRecommendations.md) operation\. Or you can test the recommender with the Amazon Personalize console\. For more information about recommenders see [Creating recommenders](creating-recommenders.md) 

**Topics**
+ [Getting recommendations with a recommender \(console\)](#get-domain-rec-console)
+ [Getting recommendations with a recommender \(AWS CLI\)](#get-domain-rec-cli)
+ [Getting recommendations with a recommender \(AWS SDKs\)](#get-domain-rec-sdk)

## Getting recommendations with a recommender \(console\)<a name="get-domain-rec-console"></a>

Use your recommender to get recommendations with the Amazon Personalize console as follows\.

**Get recommendations \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. From the navigation pane, choose **Dataset groups** and choose your dataset group\.

1. Navigate to **Recommenders** page by doing one of the following:
   + In the navigation pane, choose **Recommenders**
   + On the Overview page, choose the recommenders tab and choose **Get recommendations**\.

1.  On the **Recommenders** page, choose your use case\. 

1.  Under **Test campaign results**, enter your recommendation request details based on your use case\. For information on different use case recommendation requirements, see [Choosing recommender use cases](domain-use-cases.md)\. 

    If you recorded events for a user before they logged in \(an anonymous user\), you can get recommendations for this user by providing the `sessionId` from those events instead of a `userId`\. For more information about recording events for anonymous users, see [PutEvents operation](recording-events.md#event-record-api)\. 

1. Optionally choose a filter to filter your recommendations\. To create a filter choose **Create filters**\. For more information, see [Filtering recommendations and user segments](filter.md)\. 

1. Choose **Get recommendations**\. A table containing the user’s top 25 recommended items appears\. 

## Getting recommendations with a recommender \(AWS CLI\)<a name="get-domain-rec-cli"></a>

Use the following code to get recommendations from your recommender\. Change the value of `User ID` to a user ID that is in the data that you imported\. A list of the top 10 recommended items for the user displays\. To change the number of recommended items, change the value for `numResults`\. The default is 25 items\. The maximum is 500 items\. If your recommender's use case requires an itemId instead of a userId, replace the `user-id` parameter with `item-id` and specify the item ID\. 

 If you recorded events for a user before they logged in \(an anonymous user\), you can get recommendations for this user by providing the `sessionId` from those events instead of a `userId`\. For more information about recording events for anonymous users, see [PutEvents operation](recording-events.md#event-record-api)\. 

```
aws personalize-runtime get-recommendations \
--recommender-arn recommender arn \
--user-id User ID \
--num-results 10
```

## Getting recommendations with a recommender \(AWS SDKs\)<a name="get-domain-rec-sdk"></a>

The following code shows how to get Amazon Personalize recommendations from your recommender with the AWS SDKs\. Change the value of `userId` to a user ID that is in the data that you imported\. A list of the top 10 recommended items for the user displays\. To change the number of recommended items, change the value for `numResults`\. The default is 25 items\. The maximum is 500 items\. If your recommender's use case requires an itemId, replace the `userId` parameter with `itemId` and specify the item ID\. 

 If you recorded events for a user before they logged in \(an anonymous user\), you can get recommendations for this user by providing the `sessionId` from those events instead of a `userId`\. For more information about recording events for anonymous users, see [PutEvents operation](recording-events.md#event-record-api)\. 

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_recommendations(
    recommenderArn = 'Recommender ARN',
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
public static void getRecs(PersonalizeRuntimeClient personalizeRuntimeClient, String recommenderArn, String userId){

      try {
          GetRecommendationsRequest recommendationsRequest = GetRecommendationsRequest.builder()
                  .recommenderArn(recommenderArn)
                  .numResults(10)
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