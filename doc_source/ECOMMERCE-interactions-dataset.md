# Interactions dataset requirements \(ECOMMERCE domain\)<a name="ECOMMERCE-interactions-dataset"></a>

 An *Interactions dataset* stores historical and real\-time data from interactions between users and items in your ECOMMERCE catalog\. For more information about the types of data you can store in an interactions dataset, see [Interactions data](interactions-datasets.md)\. For information about general Amazon Personalize schema requirements, such as formatting requirements and available field data types, see [Datasets and schemas](how-it-works-dataset-schema.md)\. These requirements apply to all schemas, regardless of domain\. 

 You must at minimum create an Interactions dataset and your schema must have the following fields: 
+ USER\_ID \(`string`\)
+ ITEM\_ID `string`
+ TIMESTAMP \(`long`\)
+ EVENT\_TYPE \(`string` and depending on [use case](domain-use-cases.md), `Purchase` and `View` event types\)

 Your schema can also include the following reserved keywords:
+ EVENT\_VALUE \(`float`, `null`\)
+ IMPRESSION \(`string`, `null`\)
+ RECOMMENDATION\_ID \(`string`, `null`\)

 The data you import must match your schema\. You are free to add additional fields depending on your use case and your data\. As long as the fields aren't listed as required or reserved, and the data types are listed in [Schema data types](how-it-works-dataset-schema.md#personalize-datatypes), the field names and data types are up to you\. For an example of the default schema for Interactions datasets for ECOMMERCE domains, see [Default Interactions schema \(ECOMMERCE domain\)](#ECOMMERCE-interactions-schema)\. 

 Optionally add the reserved keyword EVENT\_VALUE if you have value data for events\. Optionally add the reserved keyword IMPRESSION if you want to include explicit and implicit impressions data\. For more information about recording impressions data see [Impressions data](interactions-datasets.md#interactions-impressions-data)\. 

 The maximum total number of optional metadata fields you can add to an Interactions dataset, combined with total number of *distinct* event types in your data, is 10\. The metadata fields included in this count are EVENT\_TYPE, EVENT\_VALUE fields along with any custom metadata fields you add to your schema\. The maximum number of metadata fields excluding reserved fields, such as IMPRESSION, is 5\. Categorical values can have at most 1000 characters\. If you have an interaction with a categorical value with more than 1000, your dataset import job will fail\. 

For more information on minimum requirements and maximum data limits for an Interactions dataset for the ECOMMERCE domain, see [Service quotas](limits.md#limits-table)\.

## Default Interactions schema \(ECOMMERCE domain\)<a name="ECOMMERCE-interactions-schema"></a>

 The following is the default ECOMMERCE domain schema for Interactions datasets\. 

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
          "name": "TIMESTAMP",
          "type": "long"
      }
  ],
  "version": "1.0"
}
```