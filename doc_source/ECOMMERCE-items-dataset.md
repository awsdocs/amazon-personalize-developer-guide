# Items dataset requirements \(ECOMMERCE domain\)<a name="ECOMMERCE-items-dataset"></a>

 An *Items dataset* stores metadata about your ECOMMERCE items\. This might include information such as price, category, and product description for each item\. An Items dataset is optional but we recommend creating one to get the most relevant recommendations for the ECOMMERCE domain\. If you create an items dataset, your schema must include the following fields:
+ ITEM\_ID
+ PRICE \(`float`\)
+ CATEGORY\_L1 \(categorical `string`\)

 Your schema can also include the following reserved keywords:
+ CATEGORY\_L2 \(categorical `string`\)
+ CATEGORY\_L3 \(categorical `string`\)
+ PRODUCT\_DESCRIPTION \(`textual`\)
+ CREATION\_TIMESTAMP \(`float`\)
+ AGE\_GROUP \(categorical `string`\)
+ ADULT \(categorical `string`\)
+ GENDER \(categorical `string`\)

Use reserved keywords CATEGORY\_L2 and CATEGORY\_L3 for items with multiple sub\-categories under the CATEGORY\_L1 level\. For example, if `Motor vehicles` is the item's CATEGORY\_L1 value, `tire` might be a L2 category\. and `winter` might be a L3 category\. For an example of the default schema for Items datasets for ECOMMERCE domains, see [Default Items schema \(ECOMMERCE domain\)](#ECOMMERCE-items-dataset-schema)\.

 To get the best recommendations, we recommend that you keep these as many of these fields in your schema as you have data\. The data you import must match your schema\. For information on textual and categorical metadata see [Item data](items-datasets.md)\. 

 To use categorical data, add a field of type `string` and set the field's categorical attribute to `true` in your schema\. Then include the categorical data in your bulk CSV file and incremental item imports\. For items with multiple categories, separate each value using the vertical bar, '\|'\. For example, for a GENRES field your data for an item might be action\|adventure\|comedy\. If you have a multiple levels of categorical data, add a field for each level and append a level indicator after each field name\. For example, CATEGORY\_L1, CATEGORY\_L2, CATEGORY\_L3\. 

Categorical values can have at most 1,000 characters\. If you have a user with a categorical value with more than 1,000 characters, your dataset import job will fail\. 

## Default Items schema \(ECOMMERCE domain\)<a name="ECOMMERCE-items-dataset-schema"></a>

 The following is the default schema for Items datasets for the ECOMMERCE domain with only the required fields\. 

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
      "name": "PRICE",
      "type": "float"
    },
    {
      "name": "CATEGORY_L1",
      "type": [
        "string"
      ],
      "categorical": true
    }
  ],
  "version": "1.0"
}
```