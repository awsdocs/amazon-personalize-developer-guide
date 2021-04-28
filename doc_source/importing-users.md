# Importing users incrementally<a name="importing-users"></a>

 After you create a Users dataset, you can incrementally import one or more new users into the dataset\. Incrementally importing users allows you to keep your Users dataset current as your business grows\. If you have a large amount of new users, we recommend that you first import data in bulk and then import user data incrementally as necessary\. See [Importing bulk records](bulk-data-import.md)\. 

You can use the Amazon Personalize console, the AWS Command Line Interface \(AWS CLI\), or AWS SDKs to import users\. If you import a user with the same `userId` as a user that's already in your Users dataset, Amazon Personalize replaces the user with the new one\. You can import up to 10 users at a time\.

For information about how Amazon Personalize updates filters for new records and how new records influence recommendations, see [Importing records incrementally](incremental-data-updates.md)\. 

**Topics**
+ [Importing users incrementally \(console\)](#importing-users-console)
+ [Importing users incrementally \(AWS CLI or AWS SDKs\)](#importing-users-cli-sdk)

## Importing users incrementally \(console\)<a name="importing-users-console"></a>

You can import up to 10 users at a time\. This procedure assumes you have already created a Users dataset\. For information about creating datasets, see [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md)\.

**To import users incrementally \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. On the **Dataset groups** page, choose the dataset group with the Users dataset that you want to import the user to\. 

1. In the navigation pane, choose **Datasets**\. 

1. On the **Datasets** page, choose the Users dataset\. 

1. On the dataset details page, at the top right, choose **Modify dataset** and choose **Create record**\. 

1. On the **Create user record\(s\)** page, for record input, enter the user details in JSON format\. The user's field names and values must match the schema you used when you created the Users dataset\. Amazon Personalize provides a JSON template with field names and data types from this schema\. 

1. Choose **Create record\(s\)**\. In **Response**, the result of the import is listed and a success or failure message is displayed\.

## Importing users incrementally \(AWS CLI or AWS SDKs\)<a name="importing-users-cli-sdk"></a>

Add one or more users to your Users dataset with the [PutUsers](API_UBS_PutUsers.md) operation\. You can import up to 10 users with a single `PutUsers` call\. 

This section assumes that you have already created a Users dataset\. For information about creating datasets, see [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md)\.

Replace `dataset arn` with the Amazon Resource Name \(ARN\) of your dataset and `user Id` with the ID of the user\. If a user with the same `userId` is already in your users dataset, Amazon Personalize replaces it with the new one\. 

For `properties`, for each field in your Users dataset, replace the `propertyName` with the field name from your schema in camel case\. For example, CREATION\_TIMESTAMP would be creationTimestamp\. Replace `user data` with the data for the user\. `CREATION_TIMESTAMP` data must be in [Unix epoch time format](data-prep-formatting.md#timestamp-data) and in seconds\. For categorical string data, to include multiple categories for a single property, separate each category with a pipe \(`|`\)\. For example `\"Horror|Action\"`\. 

------
#### [ Python ]

```
import boto3

personalize_events = boto3.client(service_name='personalize-events')

personalize_events.put_users(
    datasetArn = 'dataset arn',
    users = [{
        'userId': 'user ID',
        'properties': "{\"propertyName\": \"user data\"}"   
        },
        {
        'userId': 'user ID',
        'properties': "{\"propertyName\": \"user data\"}"   
        }]
)
```

------
#### [ CLI ]

```
aws personalize-events put-users \
  --dataset-arn dataset arn \
  --users '[{
      "userId": "user Id", 
      "properties": "{\"propertyName\": "\user data\"}" 
    }, 
    {
      "userId": "user Id", 
      "properties": "{\"propertyName\": "\user data\"}" 
    }]'
```

------