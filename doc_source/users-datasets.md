# Users dataset<a name="users-datasets"></a>

 A *Users dataset* stores metadata about your users\. This might include information such as age, gender, or loyalty membership\. A Users dataset is optional\. You must at minimum create an [Interactions dataset](interactions-datasets.md)\. 

 When you create a Users dataset, you must also create a schema for the dataset\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\.  For an example of a Users schema, see [Users schema example](#schema-examples-users)\. For information on schema requirements see [Dataset and schema requirements](how-it-works-dataset-schema.md#dataset-requirements)\. 

 This section provides information about required user data and the kinds of user data you can upload for training\. It also includes a [Users schema example](#schema-examples-users)\. For information about importing user data into a Users dataset, see [Preparing and importing data](data-prep.md)\. 

 Once you create a Users dataset and add user data, you can then filter recommendations to include or exclude items based on specific user conditions\. For more information see [Filtering recommendations](filter.md)\. 

**Note**  
RELATED\_ITEMS recipes, such as item\-to\-item similarities \(SIMS\), do not use Users datasets\.

**Topics**
+ [Required user data](#user-dataset-requirements)
+ [Categorical metadata](#user-categorical-data)
+ [Users schema example](#schema-examples-users)

## Required user data<a name="user-dataset-requirements"></a>

 The training data you provide for each user must match your schema\. At minimum, you must provide a User ID for each user \(max length 256 characters\)\. Depending on your schema, user metadata can include empty/null values\. 

For more information on minimum requirements and maximum data limits for a Users dataset, see [Service quotas](limits.md#limits-table)\.

## Categorical metadata<a name="user-categorical-data"></a>

With the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) or [Personalized\-Ranking](native-recipe-search.md) recipes, Amazon Personalize uses categorical metadata, such as a user's gender or membership status, when identifying underlying patterns that reveal the most relevant items for your users\. 

With all recipes, you can import categorical metadata and use it to filter recommendations based on a user's attributes\. For information about filtering recommendations see [Filtering recommendations](filter.md)\. 

 To use categorical data, add a field of type `string` and set the field's categorical attribute to `true` in your schema\. Then include the categorical data in your bulk CSV file and incremental item imports\. For users with multiple categories, separate each value using the vertical bar, '\|'\. For an example of a schema with a categorical field see [Users schema example](#schema-examples-users)\. 

Categorical values can have at most 1,000 characters\. Any user with a categorical value with more than 1,000 characters is dropped during a dataset import job and is not used in training\. 

## Users schema example<a name="schema-examples-users"></a>

The following example shows how to structure a Users schema\. The `USER_ID` field is required and the `AGE` and `GENDER` fields are metadata\. At least one metadata field is required and you can add at most 5 metadata fields\. For information about schema requirements see [Dataset and schema requirements](how-it-works-dataset-schema.md#dataset-requirements)\.

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

For this schema, the first few lines of historical data in a CSV file might look like the following\.

```
USER_ID,AGE,GENDER
5,34,Male
6,56,Female
8,65,Male
...
...
```