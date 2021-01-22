# Items Dataset<a name="items-datasets"></a>

 An *Items dataset* stores metadata about your items\. This might include information such as price, genre, or availability\. 

 When you create an Items dataset, you must also create a schema for the dataset\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\. For an example of a schema for an Items dataset see [Items Schema Example](#schema-examples-items)\.  For information on schema requirements see [Dataset and Schema Requirements](how-it-works-dataset-schema.md#dataset-requirements)\. 

 This section provides information about required item data and the kinds of item data, including Creation Timestamp data, you can upload for training\. It also includes an [Items Schema Example](#schema-examples-items)\. For information about importing item data into an Items dataset, see [Preparing and Importing Data](data-prep.md)\. 

 Once you create an Items dataset and import item data, you can then filter recommendations to include or exclude items based on specific item conditions\. For more information see [Filtering Recommendations](filter.md)\. 

**Topics**
+ [Required Item Data](#item-dataset-requirements)
+ [Creation Timestamp Data](#creation-timestamp-data)
+ [Items Schema Example](#schema-examples-items)

## Required Item Data<a name="item-dataset-requirements"></a>

 The training data you provide for each item must match your schema\. At minimum, you must provide an Item ID for each item\. Depending on your schema, item metadata can include empty/null values\. 

During model training, Amazon Personalize considers a maximum of 750 thousand items\. If you import more than 750 thousand items, Amazon Personalize decides which items to include in training, with an emphasis on including new items \(items you recently added with no interactions\) and existing items with recent interactions data\.

 For more information on minimum requirements and maximum data limits for an Items dataset, see [Service Quotas](limits.md#limits-table)\.

## Creation Timestamp Data<a name="creation-timestamp-data"></a>

Amazon Personalize uses creation timestamp data \(in UNIX epoch time format, in seconds\) to calculate the age of an item and adjust recommendations accordingly\.

If creation timestamp data is missing for one or more items, Amazon Personalize infers this information from interaction data, if any, and uses the timestamp of the item’s oldest interaction data as the item's creation date\. If an item has no interaction data, its creation date is set as the timestamp of the latest interaction in the training set and is considered a new item\. 

## Items Schema Example<a name="schema-examples-items"></a>

The following example shows an Items schema\. The `ITEM_ID` field is required\. The `GENRE` field is metadata\. At least one metadata field is required and you can add at most 50 metadata fields\. The `CREATION_TIMESTAMP` field is a reserved keyword\. For information on schema requirements see [Dataset and Schema Requirements](how-it-works-dataset-schema.md#dataset-requirements)\.

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
    }
  ],
  "version": "1.0"
}
```