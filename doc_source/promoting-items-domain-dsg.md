# Promoting items in recommendations \(Domain dataset group\)<a name="promoting-items-domain-dsg"></a>

 For all use cases except [Trending now](VIDEO_ON_DEMAND-use-cases.md#trending-now-use-case), you can specify a promotion when you get recommendations\. A *promotion* defines additional business rules that apply to a configurable subset of recommended items\. For example, you might have a streaming app and want to promote your own shows and movies but also recommend relevant titles\. You could use a promotion to specify that a certain percentage of recommended items must come from the category *in\-house*\. The remaining recommended items would continue to be relevant recommendations based on your use case and any request filters\. 

 To apply a promotion, you specify the following in your recommendation request: 
+ The percentage of recommended items to apply the promotion to\.
+ A filter that specifies the promotion criteria\. For more information, see [Promotion filters](#promotion-filters-domain)\. 

In the recommendation response, promoted items are positioned randomly relative to other recommended items, but in sorted order relative to other promoted items\. Depending on your use case, recommended items that aren't part of a promotion are sorted by relevance to the user, popularity, or similarity\. If there aren't enough items that meet the promotion criteria, the result will contain as many promoted items as possible\. 

You can apply a promotion to recommendations with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\.

**Topics**
+ [Promotion filters](#promotion-filters-domain)
+ [Promoting items \(console\)](#promoting-items-console-domain-dsg)
+ [Promoting items \(AWS CLI\)](#promoting-items-cli-domain-dsg)
+ [Promoting items \(AWS SDKs\)](#promoting-items-sdk-domain-dsg)

## Promotion filters<a name="promotion-filters-domain"></a>

 When you apply a promotion to a recommendation request, you choose a filter that specifies the promotion criteria\. You can use an existing filter or create a new one\. You create and manage filters for promotions just as you would other filters in Amazon Personalize\. For information about creating and managing filters, see [Filtering results](filter.md)\. 

 The only difference between a promotion filter and a filter you choose outside the promotion \(the *request filter*\) is how Amazon Personalize applies them\. A promotion filter applies to only promoted items, while a request filter applies to only the remaining recommended items\. If you specify a request filter and promotion filter, and to apply both filters to promoted items, your promotion filter's expression must include both expressions\. The way you combine two expressions depends on the datasets you use\. For more information on filter expressions, their rules, and how to create them, see [Filter expressions](filter-expressions.md)\. 

 **Filter expression examples** 

The following expression includes only items from the category "in\-house"\. You might use this expression if you want to promote your own content in your recommendations\. 

```
INCLUDE ItemID WHERE Items.OWNER IN ("in-house")
```

The following expression includes only items created earlier than a timestamp you specify\. You might use this expression to promote items created recently\.

```
INCLUDE ItemID WHERE Items.CREATION_TIMESTAMP < $DATE
```

The following expression shows how you might apply a request filter to promoted items\. It includes only available clothing items as promoted items\. In this scenario, the `Items.AVAILABLE IN ("True")` would also be used in the request filter expression, so that all recommendations are for items that are available\. 

```
INCLUDE ItemID WHERE Items.CATEGORY IN ("clothing") AND Items.AVAILABLE IN ("True")
```

For a more complete list of filter examples, see [Filter expression examples](filter-expressions.md#filter-expression-examples)\.

## Promoting items \(console\)<a name="promoting-items-console-domain-dsg"></a>

 To promote certain items in recommendations with the Amazon Personalize console, create a filter and then provide the promotion details in the recommendation request\. For information on other fields, see [Getting recommendations with a recommender \(console\)](domain-dsg-recommendations.md#get-domain-rec-console)\. 

**To promote items in recommendations**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\. 

1. Choose the Domain dataset group that contains the recommender you are using\.

1. If you haven't already, create a filter that specifies the promotion criteria\. You create filters for promotions the same way that you create request filters\. For information on creating and managing filters, see [Filtering results](filter.md)\.

1. Navigate to **Recommenders** page by doing one of the following:
   + In the navigation pane, choose **Recommenders**
   + On the Overview page, choose the recommenders tab and choose **Get recommendations**\.

1.  On the **Recommenders** page, choose your recommender\.

1.  At the top right, choose **Test recommender**\. 

1. In **Recommendation parameters**, enter your recommendation request details based on your use case\. For information on different use case recommendation requirements, see [Choosing recommender use cases](domain-use-cases.md)\. 

    If you recorded events for a user before they logged in \(an anonymous user\), you can get recommendations for this user by providing the `sessionId` from those events instead of a `userId`\. For more information about recording events for anonymous users, see [Recording events with the PutEvents operation](recording-events.md#event-record-api)\. 

1. Optionally choose a filter to filter your recommendations\. To create a filter choose **Create new filter**\. For more information, see [Filtering recommendations and user segments](filter.md)\. If your use case includes automatic filtering \(such as filtering already purchased items for the [Recommended for you](ECOMMERCE-use-cases.md#recommended-for-you-use-case) use case\), the automatic filter is applied in addition your filter\.

1. If your recommender uses contextual metadata, provide context data\. For each context, for the **Key** enter the metadata field\. For the **Value** enter the context data\. For more information, see [Increasing recommendation relevance with contextual metadata](contextual-metadata.md)\. 

1. For **Promotion** specify the following\.
   + **Percent promoted items**: Enter the percentage of recommended items to apply the promotion to\.
   + **Filter**: Choose a filter that specifies the promotion criteria\. This filter applies to the promoted items instead of any request filter you may have specified in step 7\.
   + **Filter parameter**: If your promotion uses a filter with placeholder parameters, for each parameter, enter the value to set the filter criteria\. To use multiple values for one parameter, separate each value with a comma\.

1. Choose **Get recommendations**\. A table containing the user’s top 25 recommended items displays\. The **Promoted item** column indicates if the item was included because of your promotion\. Promoted items are positioned randomly relative to other recommended items, but in sorted order relative to other promoted items\. Depending on your use case, recommended items that aren't part of a promotion are sorted by relevance to the user, popularity, or similarity\. If there aren't enough items that meet the promotion criteria, the result will contain as many promoted items as possible\. 

## Promoting items \(AWS CLI\)<a name="promoting-items-cli-domain-dsg"></a>

The following code shows how to promote items in recommendations with the AWS CLI\. For the promotion fields, specify the following: 
+ name: Give the promotion a name\. The recommendation response uses the name to identify promoted items\.
+ percent\-promoted\-items: The percentage of recommended items to apply the promotion to\. In this example, 50% of items will be promoted items\.
+ filterArn: Specify the Amazon Resource Name \(ARN\) of the filter that defines the promotion criteria\. For more information, see [Promotion filters](#promotion-filters-domain)\.
+ parameter names and values: If your filter expression has any parameters, provide the parameter names \(case sensitive\) and the values\. For example, if your filter expression has a `$GENRE` parameter, provide GENRE as the key, and a genre or genres, such as *Comedy*, as the value\. Separate multiple values with a comma\. When you use the AWS CLI, for each value you must use the `/` character to escape both quotes and the `/` character\. The following code sample shows how to format the values\.

The code below shows how to use both a request filter and a promotion filter\. A promotion filter applies to only promoted items, while a request filter applies to only the remaining recommended items\. For more information, see [Promotion filters](#promotion-filters-domain)\. 

For information about additional fields, see [Getting recommendations with a recommender \(AWS CLI\)](domain-dsg-recommendations.md#get-domain-rec-cli)\.

```
aws personalize-runtime get-recommendations \
--recommender-arn RecommenderArn \
--user-id 1 \
--num-results 10  \
--filter-arn RequestFilterArn \
--filter-values '{
    "RequestFilterParameterName": "\"value\"",
    "RequestFilterParameterName": "\"value1\",\"value2\",\"value3\""
  }' \
--promotions "[{
  \"name\": \"promotionName\",
  \"percentPromotedItems\": 50,
  \"filterArn\": \"PromotionFilterARN\",
  \"filterValues\": {\"PromotionParameterName\":\"\\\"value1, value2\\\"\"}
}]"
```

 A list of recommended items displays\. Promoted items are positioned randomly relative to other recommended items, but in sorted order relative to other promoted items\. Depending on your use case, recommended items that aren't part of a promotion are sorted by relevance to the user, popularity, or similarity\. If there aren't enough items that meet the promotion criteria, the result will contain as many promoted items as possible\. 

```
{
  "itemList": [
      { 
          "itemId1": "123",
          "promotionName": "promotionName"
      },
      { 
         "itemId2": "456"
      },
      { 
         "itemId3": "789"
      },
      .....
 ]
```

## Promoting items \(AWS SDKs\)<a name="promoting-items-sdk-domain-dsg"></a>

The following code shows how to promote items in recommendations with the SDK for Python \(Boto3\) and the SDK for Java 2\.x\. For the promotion fields, specify the following: 
+ name: Specify the name of the promotion\. The recommendation response includes the name to identify promoted items\.
+ percentPromotedItems: The percentage of recommended items to apply the promotion to\.
+ promotionFilterARN: The Amazon Resource Name \(ARN\) of the filter that defines the promotion criteria\. For more information, see [Promotion filters](#promotion-filters-domain)\.
+ Any parameter names and values: If your filter expression has any parameters, for each parameter in your filter expression, provide the parameter name \(case sensitive\) and the values\. For example, if your filter expression has a `$GENRE` parameter, provide `"GENRE"` as the key, and a genre or genres, such as "\\"Comedy"\\", as the value\. Separate multiple values with a comma\. For example, `"\"comedy\",\"drama\",\"horror"\"`\. 

The code below shows how to use both a request filter and a promotion filter\. A promotion filter applies to only promoted items, while a request filter applies to only the remaining recommended items\. For more information, see [Promotion filters](#promotion-filters-domain)\. 

For information about additional fields, see [Getting recommendations with a recommender \(AWS SDKs\)](domain-dsg-recommendations.md#get-domain-rec-sdk)\.

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_recommendations(
  recommenderArn = "RecommenderARN",
  userId = '1',
  numResults = 10,
  filterArn = 'RequestFilterARN',
  filterValues = {
      "RequestFilterParameterName": "\"value1\"",
      "RequestFilterParameterName": "\"value1\",\"value2\",\"value3\""
      ....
  },
  promotions = [{
    "name" : "promotionName",
    "percentPromotedItems" : 50,
    "filterArn": "promotionFilterARN",
    "filterValues": {
      "PromotionParameterName": "\"Value1\",\"Value2\""
      ...
    } 
  }]
)

print("Recommended items")
for item in response['itemList']:
    print (item['itemId'])
    if ("promotionName" in item):
        print(item['promotionName'])
```

------
#### [ SDK for Java 2\.x ]

```
public static void getRecommendationsWithPromotedItems(PersonalizeRuntimeClient personalizeRuntimeClient,
                                       String recommenderArn,
                                       String userId,
                                       String requestFilterArn,
                                       String requestParameterName,
                                       String requestParameterValue1,
                                       String requestParameterValue2,
                                       String promotionName,
                                       int percentPromotedItems,
                                       String promotionFilterArn,
                                       String promotionParameterName,
                                       String promotionParameterValue1,
                                       String promotionParameterValue2) {

  try {
        
      Map<String, String> promotionFilterValues = new HashMap<>();

      promotionFilterValues.put(promotionParameterName, String.format("\"%1$s\",\"%2$s\"",
              promotionParameterValue1, promotionParameterValue2));
              
      Promotion newPromotion = Promotion.builder()
              .name(promotionName)
              .percentPromotedItems(percentPromotedItems)
              .filterArn(promotionFilterArn)
              .filterValues(promotionFilterValues)
              .build();
              
      List<Promotion> promotionList = new List<>();
      
      promotionsList.add(newPromotion);
      
      Map<String, String> requestfilterValues = new HashMap<>();

      requestfilterValues.put(requestParameterName, String.format("\"%1$s\",\"%2$s\"",
              requestParameterValue1, requestParameterValue2));
              
      GetRecommendationsRequest recommendationsRequest = GetRecommendationsRequest.builder()
              .recommenderArn(recommenderArn)
              .numResults(20)
              .userId(userId)
              .filterArn(requestFilterArn)
              .fitlerValues(requestFilterValues)
              .promotions(promotionList)
              .build();

      GetRecommendationsResponse recommendationsResponse = personalizeRuntimeClient.getRecommendations(recommendationsRequest);
      List<PredictedItem> items = recommendationsResponse.itemList();

      for (PredictedItem item: items) {
          System.out.println("Item Id is : "+item.itemId());
          System.out.println("Promotion name is : "+item.promotionName());
      }
  } catch (PersonalizeRuntimeException e) {
      System.err.println(e.awsErrorDetails().errorMessage());
      System.exit(1);
  }
}
```

------

 A list of recommended items displays\. Promoted items are positioned randomly relative to other recommended items, but in sorted order relative to other promoted items\. Depending on your use case, recommended items that aren't part of a promotion are sorted by relevance to the user, popularity, or similarity\. If there aren't enough items that meet the promotion criteria, the result will contain as many promoted items as possible\. 

```
{
  "itemList": [
      { 
          "itemId1": "123",
          "promotionName": "promotionName"
      },
      { 
         "itemId2": "456"
      },
      { 
         "itemId3": "789"
      },
      .....
 ]
```