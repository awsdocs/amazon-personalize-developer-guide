# Users dataset requirements \(ECOMMERCE domain\)<a name="ECOMMERCE-users-dataset"></a>

 A *Users dataset* stores metadata about your users\. This might include information such as age, gender, and loyalty membership for each user\. A Users dataset is optional but we recommend creating one to get the most relevant recommendations for the ECOMMERCE domain\. If you create a Users dataset, your schema must include the following fields\. 
+ USER\_ID
+ 1 metadata field \(categorical `string` or numerical\)

For an example of the default schema for Users datasets for ECOMMERCE domains, see [Default Users schema \(ECOMMERCE domain\)](#ECOMMERCE-users-dataset-schema)\.

 To use categorical data, add a field of type `string` and set the field's categorical attribute to `true` in your schema\. Then include the categorical data in your bulk CSV file and incremental item imports\. For users with multiple categories, separate each value using the vertical bar, '\|'\. For example, for a SUBSCRIPTION\_MODEL field, your data for a user might be student\|monthly\|discount\. 

Categorical values can have at most 1,000 characters\. If you have a user with a categorical value with more than 1,000 characters, your dataset import job will fail\. 

 For more information on minimum requirements and maximum data limits for a Users dataset, see [Service quotas](limits.md#limits-table)\. 

## Default Users schema \(ECOMMERCE domain\)<a name="ECOMMERCE-users-dataset-schema"></a>

 The following is the default ECOMMERCE domain schema for Users datasets with a CATEGORY field as the required metadata field\. 

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
          "name": "CATEGORY",
          "type": "string",
          "categorical": true
      }
  ],
  "version": "1.0"
}
```