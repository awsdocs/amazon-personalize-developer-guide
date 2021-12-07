# Items dataset requirements \(VIDEO\_ON\_DEMAND domain\)<a name="VIDEO-ON-DEMAND-items-dataset"></a>

 An *Items dataset* stores metadata about your VIDEO\_ON\_DEMAND items\. This might include information such as price, genre, and availability for each item\. An Items dataset is optional but we recommend creating one to get the most relevant recommendations for the VIDEO\_ON\_DEMAND domain\. If you create an items dataset, your schema must include the following fields:
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
+ CONTENT\_OWNER \(categorical `string`\)
+ CONTENT\_CLASSIFICATION \(categorical `string`\)

Use reserved keywords GENRE\_L2 and GENRE\_L3 for more specific sub\-genres\. For example, if `History` is the video's GENRES value, `Biopic` might be a L2 genre\. For an example of the default schema for Items datasets for VIDEO\_ON\_DEMAND domains, see [Default Items schema \(VIDEO\_ON\_DEMAND domain\)](#VIDEO-ON-DEMAND-items-dataset-schema)\.

 To get the best recommendations, we recommend that you keep these as many of these fields in your schema as you have data\. The data you import must match your schema\. For information on textual and categorical metadata see [Item data](items-datasets.md)\. 

 To use categorical data, add a field of type `string` and set the field's categorical attribute to `true` in your schema\. Then include the categorical data in your bulk CSV file and incremental item imports\. For items with multiple categories, separate each value using the vertical bar, '\|'\. For example, for a GENRES field your data for an item might be action\|adventure\|comedy\. If you have a multiple levels of categorical data, add a field for each level and append a level indicator after each field name\. For example, CATEGORY\_L1, CATEGORY\_L2, CATEGORY\_L3\. 

Categorical values can have at most 1,000 characters\. If you have a user with a categorical value with more than 1,000 characters, your dataset import job will fail\. 

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