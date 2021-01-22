# Importing Users Incrementally<a name="importing-users"></a>

 You can incrementally add one or more new users to your Users dataset using the AWS Command Line Interface \(AWS CLI\) or AWS SDK\. If a user with the same `userId` is already in your Users dataset, Amazon Personalize replaces the existing user with the new one\.

**Topics**
+ [Importing Users Incrementally \(AWS CLI\)](#importing-users-cli)
+ [Importing Users Incrementally \(AWS Python SDK\)](#importing-users-sdk)

## Importing Users Incrementally \(AWS CLI\)<a name="importing-users-cli"></a>

Add one or more users to your Users dataset using the [PutUsers](API_UBS_PutUsers.md) API\. You can add up to 10 users with a single `PutUsers` call\.

Replace the `dataset arn` with the Amazon Resource Name \(ARN\) of your dataset and replace `user Id` with the ID of the user\. If a user with the same `userId` is already in your Users dataset, Amazon Personalize replaces the existing user with the new one\.

 For `properties`, for each metadata field in your Users dataset, replace the `propertyName` with the field name from your schema in camel case\. For example, NUMBER\_OF\_VIDEOS\_WATCHED would be numberOfVideosWatched\. Replace `user data` with the data for the user\. For categorical string data, to include multiple categories for a single property, separate each category with a pipe separator \(`|`\)\. For example `\"Member|Frequent shopper\"`\.

```
aws personalize-events put-users \
  --dataset-arn dataset arn \
  --users '[{
      "userId": "user Id",
      "properties": "{\"propertyName\": \"user data\"}"
    }]'
```

## Importing Users Incrementally \(AWS Python SDK\)<a name="importing-users-sdk"></a>

Add one or more users to your Users dataset using the [PutUsers](API_UBS_PutUsers.md) operation\. You can add up to 10 users with a single `PutUsers` call\.

Replace the `dataset arn` with the ARN of your dataset and replace `user Id` with the ID of the user\. If a user with the same `userId` is already in your Users dataset, Amazon Personalize replaces the existing user with the new one\.

For `properties`, for each metadata field in your Users dataset, replace the `propertyName` with the field name from your schema in camel case\. For example, NUMBER\_OF\_VIDEOS\_WATCHED would be numberOfVideosWatched\. Replace `user data` with the data for the user\. For categorical string data, to include multiple categories for a single property, separate each category with a pipe separator \(`|`\)\. For example `\"Member|Frequent shopper\"`\.

```
import boto3

personalize_events = boto3.client(service_name='personalize-events')

personalize_events.put_users(
    datasetArn = 'dataset arn',
    users = [{
        'userId': 'user ID',
        'properties': "{\"propertyName\": \"user data\"}"   
        }]
)
```