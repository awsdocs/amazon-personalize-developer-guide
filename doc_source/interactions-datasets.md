# Interactions Dataset<a name="interactions-datasets"></a>

 An *Interactions dataset* stores historical and real\-time data from interactions between users and items\. To create a recommendation system using Amazon Personalize, you must at minimum create an Interactions dataset\. 

 In Amazon Personalize, an *interaction* is an *event* that you record and then import as training data\. You can record multiple event types, such as *click*, *watch* or *like*\. For example, if a user *clicks* a particular item and then *likes* the item, and you want Amazon Personalize to use these events as training data, for each event you would record the user's ID, the item's ID, the timestamp \(in Unix time epoch format\), and the event type \(*click* and *like*\)\. You would then add both interaction events to an Interactions dataset\. Once you have recorded enough events, you can train a model and use Amazon Personalize to generate recommendations for users\. For minimum requirements see [Service Quotas](limits.md#limits-table)\. 

 When you create an Interactions dataset, you must also create a schema for the dataset\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\.  For an example of a schema for an Interactions dataset see [Interactions Schema Example](#schema-examples-interactions)\. For information on schema requirements see [Dataset and Schema Requirements](how-it-works-dataset-schema.md#dataset-requirements)\. 

 This section provides information about the kinds of interactions data, including impressions data and contextual metadata, you can upload for training\. It also includes an [Interactions Schema Example](#schema-examples-interactions)\. For information about importing historical interactions data, see [Preparing and Importing Data](data-prep.md)\. For information about recording events in real\-time using the [PutEvents](API_UBS_PutEvents.md) API, see [Recording Events](recording-events.md)\. 

 Once you create an Interactions dataset and import interaction data, you can then filter recommendations to include or exclude items that a user has interacted with\. For more information see [Filtering Recommendations](filter.md)\. 

**Topics**
+ [Required Interaction Data](#interactions-dataset-requirements)
+ [Impressions Data](#interactions-impressions-metadata)
+ [Contextual Metadata](#interactions-contextual-metadata)
+ [Interactions Schema Example](#schema-examples-interactions)

## Required Interaction Data<a name="interactions-dataset-requirements"></a>

 The training data you provide for each interaction must match your schema\. Depending on your schema, interaction metadata can include empty/null values\. At minimum, you must provide the following for each interaction: 
+ User ID
+ Item ID
+ Timestamp \(in Unix epoch time format\)

The maximum total number of optional metadata fields you can add to an Interactions dataset, combined with total number of *distinct* event types in your data, is 10\. The metadata fields included in this count are EVENT\_TYPE, EVENT\_VALUE fields along with any custom metadata fields you add to your schema\. 

For more information on minimum requirements and maximum data limits for an Interactions dataset, see [Service Quotas](limits.md#limits-table)\.

## Impressions Data<a name="interactions-impressions-metadata"></a>

Impressions are lists of items that were visible to a user when they interacted with \(for example, clicked or watched\) a particular item\. Amazon Personalize can model two types of impressions: 
+ *Implicit impressions* are the recommendations, retrieved from Amazon Personalize, that you show the user\. You can integrate them into your recommendation workflow by including the `RecommendationId` \(returned by the [GetRecommendations](API_RS_GetRecommendations.md) and [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) operations\) as input for future [PutEvents](API_UBS_PutEvents.md) requests and Amazon Personalize will derive the implicit impressions based on your recommendation data\.
+ *Explicit impressions* are impressions that you manually record and send to Amazon Personalize\. Use explicit impressions to manipulate results from Amazon Personalize\. For example, use explicit impressions to filter out unavailable items and change the order of recommendations based on user interactions\.

## Contextual Metadata<a name="interactions-contextual-metadata"></a>

Interactions datasets can store contextual information for use in training\. Contextual metadata is interactions data you collect on the user's environment at the time of an event\. Including contextual metadata allows you to provide a more personalized experience for existing users\. For example, if customers shop differently when accessing your catalog from a phone compared to a computer, include contextual metadata about the user's device\. Recommendations will then be more relevant based on how they are browsing\. 

 Additionally, contextual metadata helps decrease the cold\-start phase for new or unidentified users\. The cold\-start phase refers to the period when your recommendation engine provides less relevant recommendations due to the lack of historical information regarding that user\. 

 For more information on contextual information, see the following AWS Machine Learning Blog post: [ Increasing the relevance of your Amazon Personalize recommendations by leveraging contextual information](http://aws.amazon.com/blogs/machine-learning/increasing-the-relevance-of-your-amazon-personalize-recommendations-by-leveraging-contextual-information/)\. 

## Interactions Schema Example<a name="schema-examples-interactions"></a>

The following example shows a schema for an Interactions dataset\. The `USER_ID`, `ITEM_ID`, and `TIMESTAMP` fields are required\. The `EVENT_TYPE`, `EVENT_VALUE`, and `IMPRESSION` fields are optional reserved keywords recognized by Amazon Personalize\. `LOCATION` and `DEVICE` are optional contextual metadata fields\. For information on schema requirements see [Dataset and Schema Requirements](how-it-works-dataset-schema.md#dataset-requirements)\. 

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