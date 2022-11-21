# Promoting items in recommendations \(Custom dataset group\)<a name="promoting-items"></a>

 If you use a [USER\_PERSONALIZATION](user-personalization-recipes.md) or [RELATED\_ITEMS](related-items-recipes.md) recipe, you can specify a promotion when you get recommendations\. A *promotion* defines additional business rules that apply to a configurable subset of recommended items\. For example, you might have a streaming app and want to promote your own shows and movies but also recommend relevant titles\. You could use a promotion to specify that a certain percentage of recommended items must come from the category *in\-house*\. The remaining recommended items would continue to be relevant recommendations based on your recipe and any request filters\. 

 To apply a promotion, you specify the following in your recommendation request: 
+ The percentage of recommended items to apply the promotion to\.
+ A filter that specifies the promotion criteria\. For more information, see [Promotion filters](#promotion-filters)\. 

In the recommendation response, promoted items are positioned randomly relative to other recommended items, but in sorted order relative to other promoted items\. Depending on your recipe, recommended items that aren't part of a promotion are sorted by relevance to the user, popularity, or similarity\. If there aren't enough items that meet the promotion criteria, the result will contain as many promoted items as possible\. 

You can apply a promotion to recommendations with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\.

**Topics**
+ [Promotion filters](#promotion-filters)
+ [Promoting items \(console\)](#promoting-items-console)
+ [Promoting items \(AWS CLI\)](#promoting-items-cli)
+ [Promoting items \(AWS SDKs\)](#promoting-items-sdk)

## Promotion filters<a name="promotion-filters"></a>

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

## Promoting items \(console\)<a name="promoting-items-console"></a>

 To promote certain items in recommendations with the Amazon Personalize console, create a filter and then provide the promotion details in the recommendation request\. For information on other fields, see [Getting recommendations \(console\)](recommendations.md#get-real-time-recommendations-console)\. 

**To promote items in recommendations**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\. 

1. Choose the dataset group that contains the campaign you are using\.

1. If you haven't already, create a filter that specifies the promotion criteria\. You create filters for promotions the same way that you create request filters\. For information on creating and managing filters, see [Filtering results](filter.md)\.

1. In the navigation pane, choose **Campaigns**\.

1. On the **Campaigns** page, choose the target campaign\.

1.  Under **Test campaign results**, enter your recommendation request details based on the recipe you used\. For USER\_PERSONALIZATION recipes, enter the **User ID** of the user that you want to get recommendations for\. For RELATED\_ITEMS recipes, enter the **Item ID** of the item you want Amazon Personalize to use to determine similar items\. 

1. Optionally choose a filter for the request\. This filter applies to only non\-promoted items\. For information on creating and managing filters, see see [Filtering results](filter.md)\. 

1. If your campaign uses contextual metadata, provide context data\. For each context, for the **Key** enter the metadata field\. For the **Value** enter the context data\. For more information, see [Increasing recommendation relevance with contextual metadata](contextual-metadata.md)\. 

1. For **Promotion** specify the following\.
   + **Percent promoted items**: Enter the percentage of recommended items to apply the promotion to\.
   + **Filter**: Choose a filter that specifies the promotion criteria\. This filter applies to the promoted items instead of any request filter you may have specified in step 7\.
   + **Filter parameter**: If your promotion uses a filter with placeholder parameters, for each parameter, enter the value to set the filter criteria\. To use multiple values for one parameter, separate each value with a comma\.

1. Choose **Get recommendations**\. A table containing the user’s top 25 recommended items displays\. The **Promoted item** column indicates if the item was included because of your promotion\. Promoted items are positioned randomly relative to other recommended items, but in sorted order relative to other promoted items\. Depending on your recipe, recommended items that aren't part of a promotion are sorted by relevance to the user, popularity, or similarity\. If there aren't enough items that meet the promotion criteria, the result will contain as many promoted items as possible\. 

## Promoting items \(AWS CLI\)<a name="promoting-items-cli"></a>

The following code shows how to promote items in recommendations with the AWS CLI\. For the promotion fields, specify the following: 
+ name: Give the promotion a name\. The recommendation response uses the name to identify promoted items\.
+ percent\-promoted\-items: The percentage of recommended items to apply the promotion to\. In this example, 50% of items will be promoted items\.
+ filterArn: Specify the Amazon Resource Name \(ARN\) of the filter that defines the promotion criteria\. For more information, see [Promotion filters](#promotion-filters)\.
+ parameter names and values: If your filter expression has any parameters, provide the parameter names \(case sensitive\) and the values\. For example, if your filter expression has a `$GENRE` parameter, provide GENRE as the key, and a genre or genres, such as *Comedy*, as the value\. Separate multiple values with a comma\. When you use the AWS CLI, for each value you must use the `/` character to escape both quotes and the `/` character\. The follow code sample shows how to format the values\.

The code below shows how to use both a request filter and a promotion filter\. A promotion filter applies to only promoted items, while a request filter applies to only the remaining recommended items\. For more information, see [Promotion filters](#promotion-filters)\. 

For information about additional fields, see [Getting recommendations \(AWS SDKs\)](recommendations.md#get-recommendations-sdk-example) and [Getting a personalized ranking using contextual metadata \(AWS Python SDK\)](rankings.md#personalized-ranking-contextual-metadata-example)\.

```
aws personalize-runtime get-recommendations \
--campaign-arn CampaignArn \
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

 A list of recommended items displays\. Promoted items are positioned randomly relative to other recommended items, but in sorted order relative to other promoted items\. Depending on your recipe, recommended items that aren't part of a promotion are sorted by relevance to the user, popularity, or similarity\. If there aren't enough items that meet the promotion criteria, the result will contain as many promoted items as possible\. 

```
{
  "itemList": [
      { 
          "itemId1": "123",
          "score": .0117211,
          "promotionName": "promotionName"
      },
      { 
         "itemId2": "456",
         "score": .0077976
      },
      { 
         "itemId3": "789",
         "score": .0067171
      },
      .....
 ]
```

## Promoting items \(AWS SDKs\)<a name="promoting-items-sdk"></a>

The following code shows how to promote items in recommendations with the SDK for Python \(Boto3\) and the SDK for Java 2\.x\. For the promotion fields, specify the following: 
+ name: Specify the name of the promotion\. The recommendation response includes the name to identify promoted items\.
+ percentPromotedItems: The percentage of recommended items to apply the promotion to\.
+ promotionFilterARN: The Amazon Resource Name \(ARN\) of the filter that defines the promotion criteria\. For more information, see [Promotion filters](#promotion-filters)\.
+ Any parameter names and values: If your filter expression has any parameters, for each parameter in your filter expression, provide the parameter name \(case sensitive\) and the values\. For example, if your filter expression has a `$GENRE` parameter, provide `"GENRE"` as the key, and a genre or genres, such as "\\"Comedy"\\", as the value\. Separate multiple values with a comma\. For example, `"\"comedy\",\"drama\",\"horror"\"`\. 

The code below shows how to use both a request filter and a promotion filter\. A promotion filter applies to only promoted items, while a request filter applies to only the remaining recommended items\. For more information, see [Promotion filters](#promotion-filters)\. 

For information about additional fields, see [Getting recommendations \(AWS SDKs\)](recommendations.md#get-recommendations-sdk-example) and [Getting a personalized ranking using contextual metadata \(AWS Python SDK\)](rankings.md#personalized-ranking-contextual-metadata-example)\.

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_recommendations(
  campaignArn = "CampaignARN",
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
                                       String campaignArn,
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
              .campaignArn(campaignArn)
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
          System.out.println("Item score is : "+item.score());
          System.out.println("Promotion name is : "+item.promotionName());
      }
  } catch (PersonalizeRuntimeException e) {
      System.err.println(e.awsErrorDetails().errorMessage());
      System.exit(1);
  }
}
```

------

 A list of recommended items displays\. Promoted items are positioned randomly relative to other recommended items, but in sorted order relative to other promoted items\. Depending on your recipe, recommended items that aren't part of a promotion are sorted by relevance to the user, popularity, or similarity\. If there aren't enough items that meet the promotion criteria, the result will contain as many promoted items as possible\. 

```
{
  "itemList": [
      { 
          "itemId1": "123",
          "score": .0117211,
          "promotionName": "promotionName"
      },
      { 
         "itemId2": "456",
         "score": .0077976
      },
      { 
         "itemId3": "789",
         "score": .0067171
      },
      .....
 ]
```