# Importing Items Incrementally<a name="importing-items"></a>

 You can incrementally add one or more new items, such as a movie or book, to your Items dataset using the AWS Command Line Interface \(AWS CLI\) or AWS SDK\. If an item with the same `itemId` is already in your Items dataset, Amazon Personalize replaces the existing item with the new one\.

**Topics**
+ [Importing Items Incrementally \(AWS CLI\)](#importing-items-cli)
+ [Importing Items Incrementally \(AWS Python SDK\)](#importing-items-sdk)

## Importing Items Incrementally \(AWS CLI\)<a name="importing-items-cli"></a>

Add one or more items to your Items dataset using the [PutItems](API_UBS_PutItems.md) API\. You can add up to 10 items with a single `PutItems` call\.

Replace the `dataset arn` with the Amazon Resource Name \(ARN\) of your dataset and replace `item Id` with the ID of the item\. If an item with the same `itemId` is already in your Items dataset, Amazon Personalize replaces the existing item with the new one\.

 For `properties`, for each field in your Items dataset, replace the `propertyName` with the field name from your schema in camel case\. For example, CREATION\_TIMESTAMP would be creationTimestamp\. Replace `item data` with the data for the item\. 

```
aws personalize-events put-items \
  --dataset-arn dataset arn \
  --items '[{
      "itemId": "item Id",
      "properties": "{\"propertyName\": \"item data\"}"
    }]'
```

## Importing Items Incrementally \(AWS Python SDK\)<a name="importing-items-sdk"></a>

 Add one or more items to your Items dataset using the [PutItems](API_UBS_PutItems.md) operation\. You can add up to 10 items with a single `PutItems` call\.

 Replace the `dataset arn` with the ARN of your dataset and replace `item Id` with the ID of the item\. If an item with the same `itemId` is already in your Items dataset, Amazon Personalize replaces the existing item with the new one\.

 For `properties`, for each metadata field in your Items dataset, replace the `propertyName` with the field name from your schema in camel case\. For example, CREATION\_TIMESTAMP would be creationTimestamp\. Replace `item data` with the data for the item\.

```
import boto3

personalize_events = boto3.client(service_name='personalize-events')

personalize_events.put_items(
    datasetArn = 'dataset arn',
    items = [{
        'itemId': 'item ID',
        'properties': "{\"propertyName\": \"item data\"}"   
        }]
)
```