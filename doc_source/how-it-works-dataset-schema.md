# Datasets and Schemas<a name="how-it-works-dataset-schema"></a>

Amazon Personalize *datasets* are containers for data\. There are three types of datasets:
+ **Users** – This dataset stores metadata about your users\. This might include information such as age, gender, or loyalty membership, which can be important signals in personalization systems\.
+ **Items** – This dataset stores metadata about your items\. This might include information such as price, SKU type, or availability\.
+ **Interactions** – This dataset stores historical and real\-time data from interactions between users and items\. This data can include impressions data and contextual metadata on your user's browsing context, such as their location or device \(mobile, tablet, desktop, and so on\)\. You must at minimum create an Interactions dataset\.

The Users and Items dataset types are known as metadata types and are used only by certain recipes\. For more information, see [Step 1: Choosing a Recipe](working-with-predefined-recipes.md)\.

Datasets are organized within Amazon Personalize dataset groups\. A dataset group can only have one of each type of dataset\. Each dataset must have an associated schema\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\. A schema has a name key whose value must match the dataset type\. 

 You create a dataset and a schema when you import your training data into Amazon Personalize\. For more information see [Preparing and Importing Data](data-prep.md)\. 

**Topics**
+ [Dataset and Schema Requirements](#dataset-requirements)
+ [Interactions Dataset](interactions-datasets.md)
+ [Users Dataset](users-datasets.md)
+ [Items Dataset](items-datasets.md)
+ [Schema Examples](#schema-examples)
+ [Creating a Schema Using the AWS Python SDK](#python-schema-ex)

## Dataset and Schema Requirements<a name="dataset-requirements"></a>

Each dataset has a set of required fields, reserved keywords, and their required data types, as shown in the following table\.


| Dataset Type | Required Fields | Reserved Keywords | 
| --- | --- | --- | 
| Users |  USER\_ID \(`string`\) 1 metadata field  |  | 
| Items |  ITEM\_ID \(`string`\) 1 metadata field  |  CREATION\_TIMESTAMP \(`long`\)  | 
| Interactions |  USER\_ID \(`string`\) ITEM\_ID \(`string`\) TIMESTAMP \(`long`\)  |  EVENT\_TYPE \(`string`\) EVENT\_VALUE \(`float`, `null`\) IMPRESSION \(`string`\) RECOMMENDATION\_ID \(`string`\)  | 

Before you add a dataset to Amazon Personalize, you must define a schema for that dataset\. Once you define the schema and create the dataset, you can't make changes to the schema\. Schemas in Amazon Personalize are defined in the Avro format\. For more information, see [Apache Avro](https://avro.apache.org/docs/current/)\. 

When you create a schema, you must follow these guidelines:
+ The schema fields can appear in any order, but they must match the order of the corresponding column headers in the data file\.
+ Each dataset type requires specific non\-metadata fields in its schema \(see the preceding table\)\. You must define required fields as their required data types\.
+ `EVENT_VALUE` data and Interactions, User, and Item metadata can be a `null` type\. Adding a `null` type to a field in your schema allows you to use imperfect data \(for example, metadata with blank values\), to generate personalized recommendations\.

### Metadata Fields<a name="metadata-fields"></a>

 Metadata includes string or non\-string fields that aren't required or don't use a reserved keyword\. Metadata schemas have the following restrictions: 
+ Users and Items schemas require at least one metadata field,
+ Users and Interactions datasets can contain up to five metadata fields\. An Items dataset can contain up to 50 metadata fields\. 
+ If you add your own metadata field of type `string`, it must include the `categorical` attribute\. Otherwise, Amazon Personalize won't use the field when training a model\. 

### Reserved Keywords<a name="reserved-keywords"></a>

Reserved keywords are optional, non\-metadata fields\. You must define reserved keywords as their required data type\. The following are reserved keywords:
+ EVENT\_TYPE: Use an `EVENT_TYPE` field for Interactions datasets with one or more event types, such as Click and Download\. You must define an EVENT\_TYPE field as a `string`\.
+ EVENT\_VALUE: Use an `EVENT_VALUE` field for Interactions datasets that include value data for events, such as `PERCENT_WATCHED`\. You must define an `EVENT_VALUE` field only as a `float` or `null`\.
+  CREATION\_TIMESTAMP: Use a `CREATION_TIMESTAMP` field for Items datasets with a timestamp for each item’s creation date\. Amazon Personalize uses `CREATION_TIMESTAMP` data to calculate the age of an item and adjust recommendations accordingly\. See [Creation Timestamp Data](creation-timestamp-data.md)\. 
+  IMPRESSION: Use an `IMPRESSION` field for Interactions datasets with impressions data\. Impressions are lists of items that were visible to a user when they interacted with \(for example, clicked or watched\) a particular item\. For more information see [Impressions Data](interactions-impressions-metadata.md)\. 
+  RECOMMENDATION\_ID: Use a `RECOMMENDATION_ID` field for Interactions datasets that use previous recommendations as implicit impressions data\. For more information see [Impressions Data](interactions-impressions-metadata.md)\. 

## Schema Examples<a name="schema-examples"></a>

For examples of schemas for each dataset type, see the following sections:
+  [Interactions Schema Example](schema-examples-interactions.md) 
+  [Users Schema Example](schema-examples-users.md) 
+  [Items Schema Example](schema-examples-items.md) 

## Creating a Schema Using the AWS Python SDK<a name="python-schema-ex"></a>

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

After you create a schema, use it with datasets that match the schema\. For more information, see [Formatting Your Input Data](data-prep-formatting.md)\. 