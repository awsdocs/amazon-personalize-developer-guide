# Custom datasets and schemas<a name="custom-datasets-and-schemas"></a>

When you create a Custom dataset group, you create your own schemas from scratch\. Custom dataset group datasets and schemas have fewer required fields and more flexibility\. The following topics explain the schema and data requirements for Interactions, Items, and Users datasets for a Custom dataset group\. Each dataset section lists the required data for the dataset type and provides a JSON example of a schema\. 

For information on the types of data you can import into Amazon Personalize see [Types of data you can import into Amazon Personalize](data.md)\. For information about general Amazon Personalize schema requirements, such as formatting requirements and available field data types, see [Datasets and schemas](how-it-works-dataset-schema.md)\. These requirements apply to all Amazon Personalize schemas\.

**Topics**
+ [Custom dataset and schema requirements](#dataset-requirements)
+ [Interactions dataset requirements \(custom\)](interactions-dataset-requirements.md)
+ [Users dataset requirements \(custom\)](user-dataset-requirements.md)
+ [Items dataset requirements \(custom\)](item-dataset-requirements.md)
+ [Creating a schema with SDK for Python \(Boto3\)](python-schema-ex.md)

## Custom dataset and schema requirements<a name="dataset-requirements"></a>

When you create a dataset for a Custom dataset group, each dataset type has the following required fields and reserved keywords with required data types\.


| Dataset type | Required fields | Reserved keywords | 
| --- | --- | --- | 
| Interactions \([schema example](interactions-dataset-requirements.md#schema-examples-interactions)\) |  USER\_ID \(`string`\) ITEM\_ID \(`string`\) TIMESTAMP \(`long`\)  |  EVENT\_TYPE \(`string`\) EVENT\_VALUE \(`float`, `null`\) IMPRESSION \(`string`, `null`\) RECOMMENDATION\_ID \(`string`, `null`\)  | 
| Users \([schema example](user-dataset-requirements.md#schema-examples-users)\) |  USER\_ID \(`string`\) 1 metadata field \(categorical `string` or numerical\)  |  | 
| Items |  ITEM\_ID \([schema example](item-dataset-requirements.md#schema-examples-items)\) 1 metadata field \(categorical or textual `string` field or numerical field\)  |  CREATION\_TIMESTAMP \(`long`\)  | 

### Metadata fields<a name="metadata-fields"></a>

Metadata includes string or non\-string fields that aren't required or don't use a reserved keyword\. Metadata schemas have the following restrictions: 
+ Users and Items schemas require at least one metadata field\.
+ You can add at most 5 metadata fields for a Users schema and 50 metadata fields for an Items schema\.
+ If you add your own metadata field of type `string`, it must include the `categorical` attribute or the `textual` attribute \(only Items schemas support fields with the textual attribute\)\. Otherwise, Amazon Personalize won't use the field when training a model\.

### Reserved keywords<a name="reserved-keywords"></a>

Reserved keywords are optional, non\-metadata fields\. These fields are considered reserved because you must define the fields as their required data type when you use them, and the keywords can't be used as values in your data\. Reserved categorical string fields must have `categorical` set to `true`, while reserved string fields can't be categorical\. The following are reserved keywords:
+ EVENT\_TYPE: For Interactions datasets with one or more event types, such as both *click* and *download*, use an `EVENT_TYPE` field\. You must define an EVENT\_TYPE field as a `string` and can't be set as categorical\.
+ EVENT\_VALUE: For Interactions datasets that include value data for events, such as the percentage of a video a user watched, use an `EVENT_VALUE` field with type `float` and optionally `null`\.
+  CREATION\_TIMESTAMP: For Items datasets with a timestamp for each item’s creation date, use a `CREATION_TIMESTAMP` field with a type `long`\. Amazon Personalize uses `CREATION_TIMESTAMP` data to calculate the age of an item and adjust recommendations accordingly\. See [Creation timestamp data](items-datasets.md#creation-timestamp-data)\. 
+  IMPRESSION: For Interactions datasets with explicit impressions data, use an `IMPRESSION` field with type `String` and optionally type `null`\. Impressions are lists of items that were visible to a user when they interacted with \(for example, clicked or watched\) a particular item\. For more information see [Impressions data](interactions-datasets.md#interactions-impressions-data)\. 
+  RECOMMENDATION\_ID: For Interactions datasets that use previous recommendations as implicit impressions data, optionally use a `RECOMMENDATION_ID` field with type `String` and optionally type `null`\. 

  You don't need to add a `RECOMMENDATION_ID` field for Amazon Personalize to use implicit impressions when generating recommendations\. You can pass a `recommendationId` in a [PutEvents](API_UBS_PutEvents.md) operation without it\. For more information see [Impressions data](interactions-datasets.md#interactions-impressions-data)\. 