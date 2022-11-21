# Datasets and schemas<a name="how-it-works-dataset-schema"></a>

Amazon Personalize *datasets* are containers for data\. There are three types of datasets:
+ [**Users**](users-datasets.md) – This dataset stores metadata about your users\. This might include information such as age, gender, or loyalty membership, which can be important signals in personalization systems\.
+ [**Items**](items-datasets.md) – This dataset stores metadata about your items\. This might include information such as price, SKU type, or availability\.
+ [**Interactions**](interactions-datasets.md) – This dataset stores historical and real\-time data from interactions between users and items\. In Amazon Personalize, an *interaction* is an *event* that you record and then import as training data\. For both Domain dataset groups and Custom dataset groups, you must at minimum create an Interactions dataset\.

For all use cases \(Domain dataset groups\) and recipes \(Custom dataset groups\), your interactions data must have the following: 
+ At minimum 1000 interactions records from users interacting with items in your catalog\. These interactions can be from bulk imports, or streamed events, or both\.
+ At minimum 25 unique user IDs with at least 2 interactions for each\.

Domain dataset groups and Custom dataset groups can have only one of each type of dataset\. Before you create a dataset, you define a schema for that dataset\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\. A schema has a name key whose value must match the dataset type\. After you create a schema, you can't make changes to the schema\. 

 For Domain dataset groups, each dataset type has a default schema with required fields and reserved keywords\. Each time you create a dataset, you can either use the existing domain schema or create a new one by modifying the existing default schema\. Use the default schema as a guide for what data to import for your domain\. Once you define the schema and create the dataset, you can't make changes to the schema\. 

If you import data in bulk, your data must be stored in comma\-separated values \(CSV\) format\. The first row of your CSV file must contain column headers, which must match your schema\. 

**Topics**
+ [Schema formatting requirements](#general-schema-requirements)
+ [Domain datasets and schemas](domain-datasets-and-schemas.md)
+ [Custom datasets and schemas](custom-datasets-and-schemas.md)

## Schema formatting requirements<a name="general-schema-requirements"></a>

When you create a schema for either dataset in a Domain dataset group or Custom dataset group, you must follow these guidelines:
+  You must define the schema in [Avro format](https://docs.oracle.com/database/nosql-12.1.3.0/GettingStartedGuide/avroschemas.html)\. For information on the Avro data types we support, see [Schema data types](#personalize-datatypes)\.
+ The schema fields can appear in any order, but they must match the order of the corresponding column headers in your CSV file\.
+ You must define required fields as their required data types\. Reserved categorical string fields must have `categorical` set to `true`, while reserved string fields can't be categorical\. The keywords can't be in your data\. Domain dataset group datasets have additional requirements based on both domain and dataset type\. Custom dataset group datasets have additional requirements depending on type\.
+  Schemas must be flat JSON files without nested structures\. For example, a field cannot be the parent of multiple sub\-fields\. 
+  Schema fields must have unique alphanumeric names\. For example, you can't add both a `GENRES_FIELD_1` field and a `GENRESFIELD1` field\. 
+ Amazon Personalize schemas don't support complex types such as arrays and maps\.
+ For fields with multiple values, including categorical metadata and impressions data, use the data type *string* and separate each value using the vertical bar, '\|', character\. For categorical fields, add `"categorical": true`\.

### Schema data types<a name="personalize-datatypes"></a>

Amazon Personalize schemas support the following Avro types for fields:
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