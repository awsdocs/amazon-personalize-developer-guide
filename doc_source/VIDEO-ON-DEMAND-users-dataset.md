# Users dataset requirements \(VIDEO\_ON\_DEMAND domain\)<a name="VIDEO-ON-DEMAND-users-dataset"></a>

 A *Users dataset* stores metadata about your users\. This might include information such as age, gender, and loyalty membership for each item\. For information on the types of user data you can import into Amazon Personalize, see [User data](users-datasets.md)\. For information about general Amazon Personalize schema requirements, such as formatting requirements and available field data types, see [Datasets and schemas](how-it-works-dataset-schema.md)\. These requirements apply to all schemas, regardless of domain\. 

 A Users dataset is optional for all VIDEO\_ON\_DEMAND use cases\. If you have user data, we recommend creating one to get the most relevant recommendations\. If you create a Users dataset, your schema must include the following fields\. 
+ USER\_ID
+ 1 metadata field \(categorical `string` or numerical\)

You are free to add additional fields depending on your use case and your data\. As long as the fields aren't listed as required or reserved, and the data types are listed in [Schema data types](how-it-works-dataset-schema.md#personalize-datatypes), the field names and data types are up to you\. For an example of the default schema for Users datasets for VIDEO\_ON\_DEMAND domains, see [Default Users schema \(VIDEO\_ON\_DEMAND domain\)](#VIDEO-ON-DEMAND-users-dataset-schema)\.

A `SUBSCRIPTION_MODEL` field is included in the default schema\. This field is an optional reserved keyword and must have a type of `string` with categorical set to `true`\. To get the best recommendations, we recommend that you keep this field in your schema if you have subscription model information about each of your users in your data\. The data you import must match your schema\. 

## Using categorical data<a name="vod-categorical-users"></a>

 To use categorical data, add a field of type `string` and set the field's categorical attribute to `true` in your schema\. Then include the categorical data in your bulk CSV file and incremental item imports\. For users with multiple categories, separate each value using the vertical bar, '\|'\. For example, for a SUBSCRIPTION\_MODEL field, your data for a user might be student\|monthly\|discount\. 

Categorical values can have at most 1000 characters\. If you have a user with a categorical value with more than 1000 characters, your dataset import job will fail\. 

## Default Users schema \(VIDEO\_ON\_DEMAND domain\)<a name="VIDEO-ON-DEMAND-users-dataset-schema"></a>

 The following is the default VIDEO\_ON\_DEMAND domain schema for Users datasets\. 

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
          "name": "SUBSCRIPTION_MODEL",
          "type": "string",
          "categorical": true
      }
  ],
  "version": "1.0"
}
```