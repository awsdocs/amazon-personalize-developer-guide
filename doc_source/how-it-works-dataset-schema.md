# Datasets and schemas<a name="how-it-works-dataset-schema"></a>

Amazon Personalize *datasets* are containers for data\. There are three types of datasets:
+ [**Users**](users-datasets.md) – This dataset stores metadata about your users\. This might include information such as age, gender, or loyalty membership, which can be important signals in personalization systems\.
+ [**Items**](items-datasets.md) – This dataset stores metadata about your items\. This might include information such as price, SKU type, or availability\.
+ [**Interactions**](interactions-datasets.md) – This dataset stores historical and real\-time data from interactions between users and items\. This data can include impressions data and contextual metadata on your user's browsing context, such as their location or device \(mobile, tablet, desktop, and so on\)\. You must at minimum create an Interactions dataset\.

The Users and Items dataset types are known as metadata types and are used only by certain recipes\. For more information, see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\.

Datasets are organized within Amazon Personalize dataset groups\. A dataset group can only have one of each type of dataset\. Each dataset must have an associated schema\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\. A schema has a name key whose value must match the dataset type\. 

 You create a dataset and a schema when you import your training data into Amazon Personalize\. For more information see [Preparing and importing data](data-prep.md)\. 

**Topics**
+ [Dataset and schema requirements](#dataset-requirements)
+ [Interactions dataset](interactions-datasets.md)
+ [Users dataset](users-datasets.md)
+ [Items dataset](items-datasets.md)
+ [Schema examples](#schema-examples)
+ [Creating a schema using the AWS Python SDK](#python-schema-ex)

## Dataset and schema requirements<a name="dataset-requirements"></a>

Each dataset has a set of required fields, reserved keywords, and their required data types, as shown in the following table\.


| Dataset type | Required fields | Reserved keywords | 
| --- | --- | --- | 
| Users |  USER\_ID \(`string`\) 1 metadata field \(categorical `string` or numerical\)  |  | 
| Items |  ITEM\_ID \(`string`\) 1 metadata field \(categorical or textual `string` field or numerical field\)  |  CREATION\_TIMESTAMP \(`long`\)  | 
| Interactions |  USER\_ID \(`string`\) ITEM\_ID \(`string`\) TIMESTAMP \(`long`\)  |  EVENT\_TYPE \(`string`\) EVENT\_VALUE \(`float`, `null`\) IMPRESSION \(`string`\) RECOMMENDATION\_ID \(`string`, `null`\)  | 

Before you add a dataset to Amazon Personalize, you must define a schema for that dataset\. Once you define the schema and create the dataset, you can't make changes to the schema\. 

When you create a schema, you must follow these guidelines:
+  You must define the schema in [Avro format](https://docs.oracle.com/database/nosql-12.1.3.0/GettingStartedGuide/avroschemas.html)\. For information on the Avro data types we support, see [Schema data types](#personalize-datatypes)\.
+ The schema fields can appear in any order, but they must match the order of the corresponding column headers in the data file\.
+ Each dataset type requires specific non\-metadata fields in its schema \(see the preceding table\)\. You must define required fields as their required data types\.
+  Schemas must be flat JSON files without nested structures\. For example, a field cannot be the parent of multiple sub\-fields\. 
+  Schema fields must have unique alphanumeric names\. For example, you can't add both a `GENRES_FIELD_1` field and a `GENRESFIELD1` field\. 

### Schema data types<a name="personalize-datatypes"></a>

 You must define your schema in [Avro format](https://docs.oracle.com/database/nosql-12.1.3.0/GettingStartedGuide/avroschemas.html)\. Amazon Personalize doesn't support complex types such as arrays and maps\. For fields with multiple values, including categorical metadata and impressions data, use the data type *string*\. When you format your data, separate each value using the vertical bar, '\|', character\. 

We support only the following Avro types for fields \([Reserved keywords](#reserved-keywords) have additional type requirements\.\):
+ float
+ double
+ int
+ long
+ string
+ boolean \(values `true` and `false` must be lower case in your data\)
+ null

 You can use *null* for `EVENT_VALUE` and `RECOMMENDATION_ID` reserved keywords, and interaction, user, and item metadata fields\. Adding a `null` type to a field allows you to use imperfect data \(for example, metadata with blank values\), to generate personalized recommendations\. The following example shows how to add a null type for a GENRES field\.

```
{
  "name": "GENRES",
  "type": [
    "null",
    "string"
  ],
  "categorical": true
}
```

### Metadata fields<a name="metadata-fields"></a>

Metadata includes string or non\-string fields that aren't required or don't use a reserved keyword\. Metadata schemas have the following restrictions: 
+ Users and Items schemas require at least one metadata field\.
+ You can add at most 5 metadata fields for a Users schema and 50 metadata fields for an Items schema\.
+ If you add your own metadata field of type `string`, it must include the `categorical` attribute or the `textual` attribute \(only Items schemas support fields with the textual attribute\)\. Otherwise, Amazon Personalize won't use the field when training a model\.

### Reserved keywords<a name="reserved-keywords"></a>

Reserved keywords are optional, non\-metadata fields\. These fields are considered reserved because you must define the fields as their required data type when you use them\. The following are reserved keywords:
+ EVENT\_TYPE: For Interactions datasets with one or more event types, such as both *click* and *download*, use an `EVENT_TYPE` field\. You must define an EVENT\_TYPE field as a `string`\.
+ EVENT\_VALUE: For Interactions datasets that include value data for events, such as the percentage of a video a user watched, use an `EVENT_VALUE` field with type `float` and optionally `null`\.
+  CREATION\_TIMESTAMP: For Items datasets with a timestamp for each item’s creation date, use a `CREATION_TIMESTAMP` field with a type `long`\. Amazon Personalize uses `CREATION_TIMESTAMP` data to calculate the age of an item and adjust recommendations accordingly\. See [Creation timestamp data](items-datasets.md#creation-timestamp-data)\. 
+  IMPRESSION: For Interactions datasets with explicit impressions data, use an `IMPRESSION` field with type `String`\. Impressions are lists of items that were visible to a user when they interacted with \(for example, clicked or watched\) a particular item\. For more information see [Impressions data](interactions-datasets.md#interactions-impressions-data)\. 
+  RECOMMENDATION\_ID: For Interactions datasets that use previous recommendations as implicit impressions data, optionally use a `RECOMMENDATION_ID` field with type `String` and optionally type `null`\. 

  You don't need to add a `RECOMMENDATION_ID` field for Amazon Personalize to use implicit impressions when generating recommendations\. You can pass a `recommendationId` in a [PutEvents](API_UBS_PutEvents.md) operation without it\. For more information see [Impressions data](interactions-datasets.md#interactions-impressions-data)\. 

## Schema examples<a name="schema-examples"></a>

For examples of schemas for each dataset type, see the following sections:
+  [Interactions schema example](interactions-datasets.md#schema-examples-interactions) 
+  [Users schema example](users-datasets.md#schema-examples-users) 
+  [Items schema example](items-datasets.md#schema-examples-items) 

## Creating a schema using the AWS Python SDK<a name="python-schema-ex"></a>

1. Define the Avro format schema that you want to use\.

1. Save the schema in a JSON file in the default Python folder\.

1. Create the schema using the following code\.

   ```
   import boto3
   
   personalize = boto3.client('personalize')
   
   with open('schema.json') as f:
       createSchemaResponse = personalize.create_schema(
           name = 'YourSchema',
           schema = f.read()
       )
   
   schema_arn = createSchemaResponse['schemaArn']
   
   print('Schema ARN:' + schema_arn )
   ```

1. Amazon Personalize returns the ARN of the new schema\. Store it for later use\.

Amazon Personalize provides operations for managing schemas\. For example, you can use the [ListSchemas](API_ListSchemas.md) API to get a list of the available schemas\.

After you create a schema, use it with datasets that match the schema\. For more information, see [Formatting your input data](data-prep-formatting.md)\. 