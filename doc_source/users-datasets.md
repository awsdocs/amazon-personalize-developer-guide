# Users dataset<a name="users-datasets"></a>

 A *Users dataset* stores metadata about your users\. This might include information such as age, gender, or loyalty membership\. 

 When you create a Users dataset, you must also create a schema for the dataset\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\.  For an example of a Users schema, see [Users schema example](#schema-examples-users)\. For information on schema requirements see [Dataset and schema requirements](how-it-works-dataset-schema.md#dataset-requirements)\. 

 This section provides information about required user data and the kinds of user data you can upload for training\. It also includes a [Users schema example](#schema-examples-users)\. For information about importing user data into a Users dataset, see [Preparing and importing data](data-prep.md)\. 

 Once you create a Users dataset and add user data, you can then filter recommendations to include or exclude items based on specific user conditions\. For more information see [Filtering recommendations](filter.md)\. 

**Note**  
RELATED\_ITEMS recipes, such as item\-to\-item similarities \(SIMS\), do not use Users datasets\.

**Topics**
+ [Required user data](#user-dataset-requirements)
+ [Users schema example](#schema-examples-users)

## Required user data<a name="user-dataset-requirements"></a>

 The training data you provide for each user must match your schema\. At minimum, you must provide a User ID for each user\. Depending on your schema, user metadata can include empty/null values\. Categorical values can have at most 1000 characters\. Any user with a categorical value with more than 1000 is dropped during a dataset import job and is not used in training\. 

For more information on minimum requirements and maximum data limits for a Users dataset, see [Service quotas](limits.md#limits-table)\.

## Users schema example<a name="schema-examples-users"></a>

The following example shows a Users schema in Avro format\. The `USER_ID` field is required and the `AGE` and `GENDER` fields are metadata\. At least one metadata field is required and you can add at most 5 metadata fields\. For information on schema requirements see [Dataset and schema requirements](how-it-works-dataset-schema.md#dataset-requirements)\.

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