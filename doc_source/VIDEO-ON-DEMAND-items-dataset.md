# Items dataset requirements \(VIDEO\_ON\_DEMAND domain\)<a name="VIDEO-ON-DEMAND-items-dataset"></a>

 An *Items dataset* stores metadata about your items in your catalogue\. This might include information such as price, genre, and availability for each item\. For information about the types of item data you can import into Amazon Personalize, see [Item data](items-datasets.md)\. For information about general Amazon Personalize schema requirements, such as formatting requirements and available field data types, see [Datasets and schemas](how-it-works-dataset-schema.md)\. These requirements apply to all schemas, regardless of domain\. 

An Items dataset is required for some use cases \(see [VIDEO\_ON\_DEMAND use cases](VIDEO_ON_DEMAND-use-cases.md)\)\. When optional, we still recommend creating one to get the most relevant recommendations\. If you create an Items dataset, your schema must include the following fields:
+ ITEM\_ID
+ GENRES \(categorical `string`\)
+ CREATION\_TIMESTAMP \(in Unix epoch time format\)

 Your schema can also include the following reserved keywords:
+ PRICE \(float\)
+ DURATION \(float\)
+ GENRE\_L2 \(categorical `string`\)
+ GENRE\_L3 \(categorical `string`\)
+ AVERAGE\_RATING \(`float`\)
+ PRODUCT\_DESCRIPTION \(`textual`\)
+ CONTENT\_OWNER \(categorical `string`\): The company that owns the video\. For example, values might be HBO, Paramount, and NBC\.
+ CONTENT\_CLASSIFICATION \(categorical `string`\): The content's rating\. For example, values might be G, PG, PG\-13, R, NC\-17, and unrated\.

 To get the best recommendations, we recommend that you keep these as many of these fields in your schema as you have data\. The data you import must match your schema\. You are free to add additional fields depending on your use case and your data\. As long as the fields aren't listed as required or reserved, and the data types are listed in [Schema data types](how-it-works-dataset-schema.md#personalize-datatypes), the field names and data types are up to you\. 

 Use reserved keywords GENRE\_L2 and GENRE\_L3 for items with multiple multi\-level categories\. For more information, see [Using categorical data](#VIDEO-ON-DEMAND-items-categorical-data)\. For information on textual and categorical metadata see [Item data](items-datasets.md)\. For an example of the default schema for Items datasets for ECOMMERCE domains, see [Default Items schema \(VIDEO\_ON\_DEMAND domain\)](#VIDEO-ON-DEMAND-items-dataset-schema)\. 

## Using categorical data<a name="VIDEO-ON-DEMAND-items-categorical-data"></a>

 To use categorical data, add a field of type `string` and set the field's categorical attribute to `true` in your schema\. Then include the categorical data in your bulk CSV file and incremental item imports\. Categorical values can have at most 1000 characters\. If you have an item with a categorical value with more than 1000 characters, your dataset import job will fail\.

 For items with multiple categories, separate each value with the vertical bar, '\|'\. For example, for a GENRES field your data for an item might be `Action|Crime|Biopic`\. If you have a multiple levels of categorical data and some items have multiple categories for each level in the hierarchy, add a field for each level and append a level indicator after each field name: GENRES, GENRE\_L2, GENRE\_L3\. This allows you filter recommendations based on sub\-categories, even if an item belongs to multiple multi\-level categories \(for information on creating and using filters see [Filtering recommendations and user segments](filter.md)\)\. For example, a video might have the following data for each category level: 
+ GENRES: Action\|Adventure
+ GENRE\_L2: Crime\|Western
+ GENRE\_L3: biopic

In this example, the video is in the action > crime > biopic hierarchy *and* the adventure > western > biopic hierarchy\. We recommend only using up to L3 but you can use more levels if necessary\.

## Default Items schema \(VIDEO\_ON\_DEMAND domain\)<a name="VIDEO-ON-DEMAND-items-dataset-schema"></a>

 The following is the default schema for Items datasets for the VIDEO\_ON\_DEMAND domain\. 

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
      "name": "GENRES_L1",
      "type": [
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