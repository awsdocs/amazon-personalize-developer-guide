# Users dataset requirements \(custom\)<a name="user-dataset-requirements"></a>

 The training data you provide for each user must match your schema\. At minimum, you must provide a User ID for each user \(max length 256 characters\)\. Depending on your schema, user metadata can include empty/null values\. Your Users schema must have minimum one metadata field, but if you add a `null` type, this value can be null for the user\. 

 To use categorical data, add a field of type `string` and set the field's categorical attribute to `true` in your schema\. Then include the categorical data in your bulk CSV file and incremental item imports\. For users with multiple categories, separate each value using the vertical bar, '\|'\. For example, for a SUBSCRIPTION\_MODEL field, your data for a user might be student\|monthly\|discount\. 

Categorical values can have at most 1,000 characters\. If you have a user with a categorical value with more than 1,000 characters, your dataset import job will fail\. 

For more information on minimum requirements and maximum data limits for a Users dataset, see [Service quotas](limits.md#limits-table)\.

## Users schema example \(custom\)<a name="schema-examples-users"></a>

The following example shows how to structure a Users schema\. The `USER_ID` field is required and the `AGE` and `GENDER` fields are metadata\. At least one metadata field is required and you can add at most 5 metadata fields\. For information about schema requirements see [Custom dataset and schema requirements](custom-datasets-and-schemas.md#dataset-requirements)\.

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