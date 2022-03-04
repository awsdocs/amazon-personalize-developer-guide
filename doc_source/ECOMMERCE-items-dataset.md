# Items dataset requirements \(ECOMMERCE domain\)<a name="ECOMMERCE-items-dataset"></a>

 An *Items dataset* stores metadata about your ECOMMERCE items\. This might include information such as price, category, and product description for each item\. For more information on the types of item data you can import into Amazon Personalize, see [Item data](items-datasets.md)\. 

 An Items dataset is optional for all ECOMMERCE use cases\. If you have items data, we recommend creating one to get the most relevant recommendations\. If you create an items dataset, your schema must include the following fields:
+ ITEM\_ID
+ PRICE \(`float`\)
+ CATEGORY\_L1 \(categorical `string`\)

 Your schema can also include the following reserved keywords\. For categorical fields, you can define your own range of values based on your use case\.
+ CATEGORY\_L2 \(categorical `string`\)
+ CATEGORY\_L3 \(categorical `string`\)
+ PRODUCT\_DESCRIPTION \(`textual`\)
+ CREATION\_TIMESTAMP \(`float`\)
+ AGE\_GROUP \(categorical `string`\): The age group the item is for\. Values might be newborns, infants, children, and adults\.
+ ADULT \(categorical `string`\): Whether the item is restricted to only adults, such as alcohol\. Values might be yes or no\.
+ GENDER \(categorical `string`\): The gender the item is for\. Values might be male, female, and unisex\.

 To get the best recommendations, we recommend that you keep these as many of these fields in your schema as you have data\. The data you import must match your schema\. Use reserved keywords CATEGORY\_L2 and CATEGORY\_L3 for items with multiple multi\-level categories\. For more information, see [Using categorical data](#ECOMMERCE-items-categorical-data)\. For information on textual and categorical metadata see [Unstructured text metadata](items-datasets.md#text-data)\. For an example of the default schema for Items datasets for ECOMMERCE domains, see [Default Items schema \(ECOMMERCE domain\)](#ECOMMERCE-items-dataset-schema)\. 

## Using categorical data<a name="ECOMMERCE-items-categorical-data"></a>

 To use categorical data, add a field of type `string` and set the field's categorical attribute to `true` in your schema\. Then include the categorical data in your bulk CSV file and incremental item imports\. You can define your own range of values based on your use case\. Categorical values can have at most 1000 characters\. If you have an item with a categorical value with more than 1000 characters, your dataset import job will fail\.

 For items with multiple categories, separate each value with the vertical bar, '\|'\. For example, for a CATEGORY\_L1 field your data for an item might be `Electronics|Productivity|Mouse`\. If you have a multiple levels of categorical data and some items have multiple categories for each level in the hierarchy, add a field for each level and append a level indicator after each field name: CATEGORY\_L1, CATEGORY\_L2, CATEGORY\_L3\. This allows you filter recommendations based on sub\-categories, even if an item belongs to multiple multi\-level categories \(for information on creating and using filters see [Filtering recommendations and user segments](filter.md)\)\. For example, an item might have the following data for each category level: 
+ CATEGORY\_L1: Electronics\|Productivity
+ CATEGORY\_L2: Productivity\|Computers
+ CATEGORY\_L3: Mouse

In this example, the item is in the electronics > productivity > mouse hierarchy *and* the productivity > computers > mouse hierarchy\. We recommend only using up to L3 but you can use more levels if necessary\.

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