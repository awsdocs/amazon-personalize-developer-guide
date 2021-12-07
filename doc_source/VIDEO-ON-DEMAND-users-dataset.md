# Users dataset requirements \(VIDEO\_ON\_DEMAND domain\)<a name="VIDEO-ON-DEMAND-users-dataset"></a>

 A *Users dataset* stores metadata about your users\. This might include information such as age, gender, and loyalty membership for each item\. A Users dataset is optional but we recommend creating one to get the most relevant recommendations for the VIDEO\_ON\_DEMAND domain\. If you create a Users dataset, your schema must include the following fields\. 
+ USER\_ID
+ 1 metadata field \(categorical `string` or numerical\)

For an example of the default schema for Users datasets for VIDEO\_ON\_DEMAND domains, see [Default Users schema \(VIDEO\_ON\_DEMAND domain\)](#VIDEO-ON-DEMAND-users-dataset-schema)\.

A `SUBSCRIPTION_MODEL` field is included in the default schema\. This field is an optional reserved keyword and must have a type of `string` with categorical set to `true`\. To get the best recommendations, we recommend that you keep this field in your schema if you have subscription model information about each of your users in your data\. The data you import must match your schema\. 

 To use categorical data, add a field of type `string` and set the field's categorical attribute to `true` in your schema\. Then include the categorical data in your bulk CSV file and incremental item imports\. For users with multiple categories, separate each value using the vertical bar, '\|'\. For example, for a SUBSCRIPTION\_MODEL field, your data for a user might be ads\|4k\|DVR\|live\. If you have a multiple levels of categorical data, add a field for each level and append a level indicator after each field name\. For example, CATEGORY\_L1, CATEGORY\_L2, CATEGORY\_L3\. 

Categorical values can have at most 1,000 characters\. If you have a user with a categorical value with more than 1,000 characters, your dataset import job will fail\. 

 For more information on minimum requirements and maximum data limits for a Users dataset, see [Service quotas](limits.md#limits-table)\. 

## Default Users schema \(VIDEO\_ON\_DEMAND domain\)<a name="VIDEO-ON-DEMAND-users-dataset-schema"></a>

 The following is the default VIDEO\_ON\_DEMAND domain schema for Users datasets\. 

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
          "name": "SUBSCRIPTION_MODEL",
          "type": "string",
          "categorical": true
      }
  ],
  "version": "1.0"
}
```