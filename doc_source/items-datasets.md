# Items dataset<a name="items-datasets"></a>

 An *Items dataset* stores metadata about your items\. This might include information such as price, genre, or availability\. An Items dataset is optional\. You must at minimum create an [Interactions dataset](interactions-datasets.md)\. 

 When you create an Items dataset, you must also create a schema for the dataset\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\. For an example of an Items dataset schema see [Items schema example](#schema-examples-items)\.  For information on schema requirements see [Dataset and schema requirements](how-it-works-dataset-schema.md#dataset-requirements)\. 

 This section provides information about required item data and the kinds of item data you can upload for training\. For information about importing item data into an Items dataset, see [Preparing and importing data](data-prep.md)\. 

 Once you create an Items dataset and import item data, you can then filter recommendations to include or exclude items based on specific item conditions\. For more information see [Filtering recommendations](filter.md)\. 

**Topics**
+ [Required item data](#item-dataset-requirements)
+ [Creation timestamp data](#creation-timestamp-data)
+ [Categorical metadata](#item-categorical-data)
+ [Unstructured text metadata](#text-data)
+ [Items schema example](#schema-examples-items)

## Required item data<a name="item-dataset-requirements"></a>

 The data you provide for each item must match your Items dataset schema\. At minimum, you must provide an Item ID for each item\. Depending on your schema, item metadata can include empty/null values\. 

During model training, Amazon Personalize considers a maximum of 750,000 items\. If you import more than 750,000 items, Amazon Personalize decides which items to include in training, with an emphasis on including new items \(items you recently added with no interactions\) and existing items with recent interactions data\.

 For more information on minimum requirements and maximum data limits for an Items dataset, see [Service quotas](limits.md#limits-table)\.

## Creation timestamp data<a name="creation-timestamp-data"></a>

Amazon Personalize uses creation timestamp data \(in Unix epoch time format, in seconds\) to calculate the age of an item and adjust recommendations accordingly\.

If creation timestamp data is missing for one or more items, Amazon Personalize infers this information from interaction data, if any, and uses the timestamp of the item’s oldest interaction data as the item's creation timestamp\. If an item has no interaction data, its creation timestamp is set as the timestamp of the latest interaction in the training set and Amazon Personalize considers it a new item\. 

## Categorical metadata<a name="item-categorical-data"></a>

With the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) or [Personalized\-Ranking](native-recipe-search.md) recipes, Amazon Personalize uses categorical data, such as an item's genre or color, when identifying underlying patterns that reveal the most relevant items for your users\. 

 With all recipes, you can import categorical data and use it to filter recommendations based on an item's attributes\. For information about filtering recommendations, see [Filtering recommendations](filter.md)\. 

To use categorical data, add a field of type `string` to your schema and set the field's categorical attribute to `true`\. Then include the categorical data in your bulk CSV file and incremental item imports\. For items with multiple categories, separate each value with the vertical bar, '\|'\. For an example of a schema with a categorical field see [Items schema example](#schema-examples-items)\. 

Categorical values can have a maximum of 1,000 characters\. Any item with a categorical value with more than 1,000 characters is dropped during a dataset import job and is not used in training\. 

## Unstructured text metadata<a name="text-data"></a>

With the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) or [Personalized\-Ranking](native-recipe-search.md) recipes, Amazon Personalize can extract meaningful information from unstructured text metadata, such as product descriptions, product reviews, or movie synopses\. Amazon Personalize uses unstructured text to identify relevant items for your users, particularly when items are new or have less interactions data\. Include unstructured text data in your Items dataset to increase click\-through rates and conversation rates for new items in your catalog\. 

To use unstructured data, add a field with type `string` to your Items schema and set the field's `textual` attribute to `true`\. Then include the text data in your bulk CSV file and incremental item imports\. For bulk CSV files, wrap the text in double quotes\. For an example of an Items schema with a field for unstructured text data, see [Items schema example](#schema-examples-items)\. For information about importing data into Amazon Personalize, see [Preparing and importing data](data-prep.md)\.

Unstructured text values can have at most 20,000 characters and text must be in English\. Amazon Personalize truncates values that exceed the character limit to 20,000 characters\.

## Items schema example<a name="schema-examples-items"></a>

The following example shows how to structure an Items schema\. The `ITEM_ID` field is required\. The `GENRE` field is categorical metadata and the `DESCRIPTION` field is textual metadata\. At least one metadata field is required\. You can add a maximum of 50 metadata fields\. The `CREATION_TIMESTAMP` field is a reserved keyword\. For information about schema requirements, see [Dataset and schema requirements](how-it-works-dataset-schema.md#dataset-requirements)\.

```
{
  "type": "record",
  "name": "Items",
  "namespace": "com.amazonaws.personalize.schema",
  "fields": [
    {
      "name": "ITEM_ID",
      "type": "string"
    },
    {
      "name": "GENRES",
      "type": [
        "null",
        "string"
      ],
      "categorical": true
    },
    {
      "name": "CREATION_TIMESTAMP",
      "type": "long"
    },
    {
      "name": "DESCRIPTION",
      "type": [
        "null",
        "string"
      ],
      "textual": true
    },
  ],
  "version": "1.0"
}
```

For this schema, the first few lines of historical data in a CSV file might look like the following\.

```
ITEM_ID,GENRES,CREATION_TIMESTAMP,DESCRIPTION
1,Adventure|Animation|Children|Comedy|Fantasy,1570003267,"This is an animated movie that features action, comedy, and fantasy. Audience is children. This movie was released in 2004."
2,Adventure|Children|Fantasy,1571730101,"This is an adventure movie with elements of fantasy. Audience is children. This movie was release in 2010."
3,Comedy|Romance,1560515629,"This is a romantic comedy. The movie was released in 1999. Audience is young women."
4,Comedy|Drama|Romance,1581670067,"This movie includes elements of both comedy and drama as well as romance. This movie was released in 2020."
...
...
```