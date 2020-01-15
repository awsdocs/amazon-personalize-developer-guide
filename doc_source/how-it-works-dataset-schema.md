# Datasets and Schemas<a name="how-it-works-dataset-schema"></a>

Amazon Personalize recognizes three types of historical datasets\. Each type has an associated schema with a *name* key whose value matches the dataset type\. The three types are:
+ **Users** – This dataset is intended to provide metadata about your users\. This might include information such as age, gender, or loyalty membership, which can be important signals in personalization systems\.
+ **Items** – This dataset is intended to provide metadata about your items\. This might include information such as price, SKU type, or availability\.
+ **Interactions** – This dataset is intended to provide historical interaction data between users and items\. It can also provide metadata on youe user's browsing context, such as their location or device \(mobile, tablet, desktop, and so on\)\.

The Users and Items dataset types are known as metadata types and are only used by certain recipes\. For more information, see [Using Predefined Recipes](working-with-predefined-recipes.md)\. For metadata datasets, all strings, except for `USER_ID` and `ITEM_ID`, must be marked as `categorical` in the schema, as shown in the following examples\.

**Note**  
A dataset group can contain only one of each type of dataset\.

Each dataset has a set of required fields, reserved keywords, and their required datatypes, as shown in the following table\.


| Dataset Type | Required Fields | Reserved Keywords | 
| --- | --- | --- | 
| Users |  USER\_ID \(`string`\) 1 metadata field  |  | 
| Items |  ITEM\_ID \(`string`\) 1 metadata field  |  | 
| Interactions |  USER\_ID \(`string`\) ITEM\_ID \(`string`\) TIMESTAMP \(`long`\)  |  EVENT\_TYPE \(`string`\) EVENT\_VALUE \(`float`\)  | 

Before you add a dataset to Amazon Personalize, you must define a schema for that dataset\. Each dataset type has specific requirements\. Schemas in Amazon Personalize are defined in the Avro format\. For more information, see [Apache Avro](https://avro.apache.org/docs/current/)\.

When you create a schema, you must follow these guidelines:
+ The schema fields can be in any order, but they must match the order of the corresponding column headers in the data file\.
+ Each dataset type requires specific fields in its schema\. You must define the required fields with their required data types\.
+ Some schemas have reserved keywords for field names\. If you use a reserved keyword for a field name in your schema, you must define it as its required datatype\.
+ The required fields and reserved keywords are not considered "metadata fields\."
+ Added fields that are not required or don't use a reserved keyword are metadata\. Metadata fields can be either a string or non\-string type\.
+ The users and items schemas require at least one metadata field\.
+ If you add your own metadata field of type `string`, it must include the `"categorical"` attribute\. Otherwise, you can’t use it to train a model\.
+ A schema, and its related dataset, can contain up to five metadata fields\.

The following example shows an Interactions schema\. The `EVENT_TYPE` and `EVENT_VALUE` fields are optional, and are reserved keywords recognized by Amazon Personalize\. `LOCATION` and `DEVICE` are optional contextual metadata fields\.

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
          "name": "EVENT_VALUE",
          "type": "float"
      },
      {
          "name": "LOCATION",
          "type": "string",
          "categorical": true
      },
      {
          "name": "DEVICE",
          "type": "string",
          "categorical": true
      },
      {
          "name": "TIMESTAMP",
          "type": "long"
      }
  ],
  "version": "1.0"
}
```

The following example shows a Users schema in Avro format\. Only the `USER_ID` field is required\. The `AGE` and `GENDER` fields are metadata\.

```
{
  "type": "record",
  "name": "Users",
  "namespace": "com.amazonaws.personalize.schema",
  "fields": [
      {
          "name": "USER_ID",
          "type": "string"
      },
      {
          "name": "AGE",
          "type": "int"
      },
      {
          "name": "GENDER",
          "type": "string",
          "categorical": true
      }
  ],
  "version": "1.0"
}
```

The following example shows an Items schema\. Only the `ITEM_ID` field is required\. The shown `GENRE` field is metadata\.

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
          "name": "GENRE",
          "type": "string",
          "categorical": true
      }
  ],
  "version": "1.0"
}
```

If you are using the AWS console, you create a new schema when you create a dataset for your input data\. You can also choose an existing schema\. For more information, see [Step 1: Import Training Data](getting-started-console.md#getting-started-console-import-dataset)\.

If you are using the AWS CLI, see [Step 1: Import Training Data](getting-started-cli.md#gs-create-ds) for an example\.

## Create a Schema Using the AWS Python SDK<a name="python-schema-ex"></a>

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