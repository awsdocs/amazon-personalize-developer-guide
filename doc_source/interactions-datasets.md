# Interactions dataset<a name="interactions-datasets"></a>

 An *Interactions dataset* stores historical and real\-time data from interactions between users and items\. To create a recommendation system using Amazon Personalize, you must at minimum create an Interactions dataset\. 

 In Amazon Personalize, an *interaction* is an *event* that you record and then import as training data\. You can record multiple event types, such as *click*, *watch* or *like*\. For example, if a user *clicks* a particular item and then *likes* the item, and you want Amazon Personalize to use these events as training data, for each event you would record the user's ID, the item's ID, the timestamp \(in Unix time epoch format\), and the event type \(*click* and *like*\)\. You would then add both interaction events to an Interactions dataset\. Once you have recorded enough events, you can train a model and use Amazon Personalize to generate recommendations for users\. For minimum requirements see [Service quotas](limits.md#limits-table)\. 

 When you create an Interactions dataset, you must also create a schema for the dataset\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\.  For an example of a schema for an Interactions dataset see [Interactions schema example](#schema-examples-interactions)\. For information on schema requirements see [Dataset and schema requirements](how-it-works-dataset-schema.md#dataset-requirements)\. 

 This section provides information about the kinds of interactions data, including impressions data and contextual metadata, you can upload for training\. It also includes an [Interactions schema example](#schema-examples-interactions)\. For information about importing historical interactions data, see [Preparing and importing data](data-prep.md)\. For information about recording events in real\-time using the [PutEvents](API_UBS_PutEvents.md) API, see [Recording events](recording-events.md)\. 

 Once you create an Interactions dataset and import interaction data, you can then filter recommendations to include or exclude items that a user has interacted with\. For more information see [Filtering recommendations](filter.md)\. 

**Topics**
+ [Required interaction data](#interactions-dataset-requirements)
+ [Contextual metadata](#interactions-contextual-metadata)
+ [Impressions data](#interactions-impressions-data)
+ [Interactions schema example](#schema-examples-interactions)

## Required interaction data<a name="interactions-dataset-requirements"></a>

 The training data you provide for each interaction must match your schema\. Depending on your schema, interaction metadata can include empty/null values\. At minimum, you must provide the following for each interaction: 
+ User ID
+ Item ID
+ Timestamp \(in Unix epoch time format\)

The maximum total number of optional metadata fields you can add to an Interactions dataset, combined with total number of *distinct* event types in your data, is 10\. The metadata fields included in this count are EVENT\_TYPE, EVENT\_VALUE fields along with any custom metadata fields you add to your schema\. The maximum number of metadata fields excluding reserved fields, such as IMPRESSION, is 5\. Categorical values can have at most 1000 characters\. Any interaction with a categorical value with more than 1,000 characters is dropped during a dataset import job and is not used in training\. 

For more information on minimum requirements and maximum data limits for an Interactions dataset, see [Service quotas](limits.md#limits-table)\.

## Contextual metadata<a name="interactions-contextual-metadata"></a>

If you use the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) or the [Personalized\-Ranking](native-recipe-search.md) recipes, Interactions datasets can store contextual information for use in training\. Contextual metadata is interactions data you collect on the user's environment at the time of an event\. Including contextual metadata allows you to provide a more personalized experience for existing users\. For example, if customers shop differently when accessing your catalog from a phone compared to a computer, include contextual metadata about the user's device\. Recommendations will then be more relevant based on how they are browsing\. 

 Additionally, contextual metadata helps decrease the cold\-start phase for new or unidentified users\. The cold\-start phase refers to the period when your recommendation engine provides less relevant recommendations due to the lack of historical information regarding that user\. 

 For more information on contextual information, see the following AWS Machine Learning Blog post: [ Increasing the relevance of your Amazon Personalize recommendations by leveraging contextual information](http://aws.amazon.com/blogs/machine-learning/increasing-the-relevance-of-your-amazon-personalize-recommendations-by-leveraging-contextual-information/)\. 

## Impressions data<a name="interactions-impressions-data"></a>

If you use the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) recipe, Amazon Personalize can model impressions data that you upload to an Interactions dataset\. Impressions are lists of items that were visible to a user when they interacted with \(for example, clicked or watched\) a particular item\. Amazon Personalize uses impressions data to guide exploration, where recommendations include new items with less interactions data or relevance\. For information about the benefits of exploration see [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md)\. Amazon Personalize can model two types of impressions: [Implicit impressions](#implicit-impressions-info) and [Explicit impressions](#explicit-impressions-info)\. 

### Implicit impressions<a name="implicit-impressions-info"></a>

*Implicit impressions* are the recommendations, retrieved from Amazon Personalize, that you show the user\. You can integrate them into your recommendation workflow by including the `RecommendationId` \(returned by the [GetRecommendations](API_RS_GetRecommendations.md) and [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) operations\) as input for future [PutEvents](API_UBS_PutEvents.md) requests\. Amazon Personalize derives the implicit impressions based on your recommendation data\. 

 For example, you might have an application that provides recommendations for streaming video\. Your recommendation workflow using implicit impressions might be as follows:

1. You request video recommendations for one of your users using the Amazon Personalize [GetRecommendations](API_RS_GetRecommendations.md) API operation\.

1. Amazon Personalize generates recommendations for the user using your model \(solution version\) and returns them with a `recommendationId` in the API response\.

1. You show the video recommendations to your user in your application\.

1. When your user interacts with \(for example, clicks\) a video, record the choice in a call to the [PutEvents](API_UBS_PutEvents.md) API and include the `recommendationId` as a parameter\. For a code sample see [Recording impressions data](recording-events.md#putevents-including-impressions-data)\.

1. Amazon Personalize uses the `recommendationId` to derive the impression data from the previous video recommendations, and then uses the impression data to guide exploration, where future recommendations include new videos with less interactions data or relevance\. 

   For more information on recording events with implicit impression data, see [Recording impressions data](recording-events.md#putevents-including-impressions-data)\.

### Explicit impressions<a name="explicit-impressions-info"></a>

*Explicit impressions* are impressions that you manually record and send to Amazon Personalize\. Use explicit impressions to manipulate results from Amazon Personalize\. The order of the items has no impact\. 

 For example, you might have a shopping application that provides recommendations for shoes\. If you only recommend shoes that are currently in stock, you can specify these items using explicit impressions\. Your recommendation workflow using explicit impressions might be as follows:

1. You request recommendations for one of your users using the Amazon Personalize [GetRecommendations](API_RS_GetRecommendations.md) API\.

1. Amazon Personalize generates recommendations for the user using your model \(solution version\) and returns them in the API response\.

1. You show the user only the recommended shoes that are in stock\.

1. For real\-time incremental data import, when your user interacts with \(for example, clicks\) a pair of shoes, you record the choice in a call to the [PutEvents](API_UBS_PutEvents.md) API and list the recommended items that are in stock in the `impression` parameter\. For a code sample see [Recording impressions data](recording-events.md#putevents-including-impressions-data)\.

   For importing impressions in historical interactions data, you can list explicit impressions in your csv file and separate each item with a '\|' character\. See [Formatting explicit impressions](data-prep-formatting.md#data-prep-including-explicit-impressions)\.

1. Amazon Personalize uses the impression data to guide exploration, where future recommendations include new shoes with less interactions data or relevance\. 

## Interactions schema example<a name="schema-examples-interactions"></a>

The following example shows a schema for an Interactions dataset\. The `USER_ID`, `ITEM_ID`, and `TIMESTAMP` fields are required\. The `EVENT_TYPE`, `EVENT_VALUE`, and `IMPRESSION` fields are optional reserved keywords recognized by Amazon Personalize\. `LOCATION` and `DEVICE` are optional contextual metadata fields\. For information on schema requirements see [Dataset and schema requirements](how-it-works-dataset-schema.md#dataset-requirements)\. 

```
{

  "type": "record",
  "name": "Interactions",
  "namespace": "com.amazonaws.personalize.schema",
  "fields": [
      {
          "name": "USER_ID",
          "type": "string"
      },
      {
          "name": "ITEM_ID",
          "type": "string"
      },
      {
          "name": "EVENT_TYPE",
          "type": "string"
      },
      {
          "name": "EVENT_VALUE",
          "type": [
             "float",
             "null"
          ]
      },
      {
          "name": "LOCATION",
          "type": "string",
          "categorical": true
      },
      {
          "name": "DEVICE",
          "type": [
              "string",
              "null"
          ],
          "categorical": true
      },
      {
          "name": "TIMESTAMP",
          "type": "long"
      },
      {
          "name": "IMPRESSION",
          "type": "string"
      }
  ],
  "version": "1.0"
}
```

For this schema, the first few lines of historical data in a CSV file might look like the following\. Note that some values for EVENT\_VALUE are null\.

```
USER_ID,ITEM_ID,EVENT_TYPE,EVENT_VALUE,LOCATION,DEVICE,TIMESTAMP,IMPRESSION
35,73,click,,Ohio,Tablet,1586731606,73|70|17|95|96|92|55|45|16|97|56|54|33|94|36|10|5|43|19|13|51|90|65|59|38
54,35,watch,0.75,Indiana,Cellphone,1586735164,35|82|78|57|20|63|1|90|76|75|49|71|26|24|25|6|37|85|40|98|32|13|11|54|48
9,33,click,,Oregon,Cellphone,1586735158,68|33|62|6|15|57|45|24|78|89|90|40|26|91|66|31|47|17|99|29|27|41|77|75|14
23,10,watch,0.25,California,Tablet,1586735697,92|89|36|10|39|77|4|27|79|18|83|16|28|68|78|40|50|3|99|7|87|49|12|57|53
27,11,watch,0.55,Indiana,Tablet,1586735763,11|7|39|95|71|1|6|40|41|28|99|53|68|76|0|65|69|36|22|42|34|67|24|20|66
...
...
```