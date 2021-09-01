# Importing items incrementally<a name="importing-items"></a>

After you create an Items dataset, you can incrementally import one or more new items into the dataset\. Incrementally importing items allows you to keep your Items dataset current as your catalog grows\. If you have a large amount of new items, we recommend that you first import data in bulk and then import item data incrementally as necessary\. See [Importing bulk records](bulk-data-import.md)\.

You can use the Amazon Personalize console, the AWS Command Line Interface \(AWS CLI\), or AWS SDKs to import items\. If you import an item with the same `itemId` as an item that's already in your Items dataset, Amazon Personalize replaces it with the new item\. You can import up to 10 items at a time\.

 For information about how Amazon Personalize updates filters for new records and how new records influence recommendations, see [Importing records incrementally](incremental-data-updates.md)\. 

**Topics**
+ [Importing items incrementally \(console\)](#importing-items-console)
+ [Importing items incrementally \(AWS CLI\)](#importing-items-cli)
+ [Importing items incrementally \(AWS SDKs\)](#importing-items-cli-sdk)

## Importing items incrementally \(console\)<a name="importing-items-console"></a>

You can import up to 10 items to an Items dataset at a time\. This procedure assumes that you have already created an Items dataset\. For information about creating datasets, see [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md)\.

**To import items incrementally \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. On the **Dataset groups** page, choose the dataset group with the Items dataset that you want to import the items to\. 

1. In the navigation pane, choose **Datasets**\. 

1. On the **Datasets** page, choose the Items dataset\. 

1. At the top right of the dataset details page, choose **Modify dataset**, and then choose **Create record**\. 

1. In **Create item record\(s\)** page, for **Record input**, enter the item details in JSON format\. The item's field names and values must match the schema you used when you created the Items dataset\. Amazon Personalize provides a JSON template with field names and data types from this schema\.

1. Choose **Create record\(s\)**\. In **Response**, the result of the import is listed and a success or failure message is displayed\.

## Importing items incrementally \(AWS CLI\)<a name="importing-items-cli"></a>

Add one or more items to your Items dataset using the [ PutItems ](API_UBS_PutItems.md) operation\. You can import up to 10 items with a single `PutItems` call\. This section assumes that you have already created an Items dataset\. For information about creating datasets, see [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md)\.

Use the following `put-items` command to add one or more items with the AWS CLI\. Replace `dataset arn` with the Amazon Resource Name \(ARN\) of your dataset and `item Id` with the ID of the item\. If an item with the same `itemId` is already in your Items dataset, Amazon Personalize replaces it with the new one\.

For `properties`, for each field in your Items dataset, replace the `propertyName` with the field name from your schema in camel case\. For example, GENRES would be `genres` and CREATION\_TIMESTAMP would be creationTimestamp\. Replace `item data` with the data for the item\. `CREATION_TIMESTAMP` data must be in [Unix epoch time format](data-prep-formatting.md#timestamp-data) and in seconds\. For categorical string data, to include multiple categories for a single property, separate each category with a pipe \(`|`\)\. For example `\"Horror|Action\"`\.

```
aws personalize-events put-items \
  --dataset-arn dataset arn \
  --items '[{
      "itemId": "item Id", 
      "properties": "{\"propertyName\": "\item data\"}" 
    }, 
    {
      "itemId": "item Id", 
      "properties": "{\"propertyName\": "\item data\"}" 
    }]'
```

## Importing items incrementally \(AWS SDKs\)<a name="importing-items-cli-sdk"></a>

Add one or more items to your Items dataset using the [ PutItems ](API_UBS_PutItems.md) operation\. You can import up to 10 items with a single `PutItems` call\. This section assumes that you have already created an Items dataset\. For information about creating datasets, see [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md)\.

 The following code shows how to add one or more items to your Items dataset with the AWS SDK for Python \(Boto3\) or the AWS SDK for Java 2\.x\. 

------
#### [ SDK for Python \(Boto3\) ]

Replace `dataset arn` with the Amazon Resource Name \(ARN\) of your dataset and `item Id` with the ID of the item\. If an item with the same `itemId` is already in your Items dataset, Amazon Personalize replaces it with the new one\.

 For `properties`, for each field in your Items dataset, replace the `propertyName` with the field name from your schema in camel case\. For example, GENRES would be `genres` and CREATION\_TIMESTAMP would be `creationTimestamp`\. Replace `item data` with the data for the item\. `CREATION_TIMESTAMP` data must be in [Unix epoch time format](data-prep-formatting.md#timestamp-data) and in seconds\. For categorical string data, to include multiple categories for a single property, separate each category with a pipe \(`|`\)\. For example `\"Horror|Action\"`\.

```
import boto3

personalize_events = boto3.client(service_name='personalize-events')

personalize_events.put_items(
    datasetArn = 'dataset arn',
    items = [{
        'itemId': 'item ID',
        'properties': "{\"propertyName\": \"item data\"}"   
        },
        {
        'itemId': 'item ID',
        'properties': "{\"propertyName\": \"item data\"}"   
        }]
)
```

------
#### [ SDK for Java 2\.x ]

 The following `putItems` method shows how to add two items to an Items dataset with the SDK for Java 2\.x\. If an item with the same `itemId` is already in your Items dataset, Amazon Personalize replaces it with the new one\. In this example, each item has a single property\. If your Items dataset schema has additional fields, modify the code to use additional property and value parameters\. 

For each property name parameter, pass the field name from your schema in camel case\. For example, GENRES would be `genres` and CREATION\_TIMESTAMP would be `creationTimestamp`\. For each property value parameter, pass the data for the item\. `CREATION_TIMESTAMP` data must be in [Unix epoch time format](data-prep-formatting.md#timestamp-data) and in seconds\. For categorical string data, to include multiple categories for a single property, separate each category with a pipe \(`|`\)\. For example `"Horror|Action"`\.

```
public static int putItems(PersonalizeEventsClient personalizeEventsClient,
                           String datasetArn,
                           String item1Id,
                           String item1PropertyName,
                           String item1PropertyValue,
                           String item2Id,
                           String item2PropertyName,
                           String item2PropertyValue) {

    int responseCode = 0;
    ArrayList<Item> items = new ArrayList<>();

    try {
        Item item1 = Item.builder()
                .itemId(item1Id)
                .properties(String.format("{\"%1$s\": \"%2$s\"}",
                        item1PropertyName, item1PropertyValue))
                .build();

        items.add(item1);

        Item item2 = Item.builder()
                .itemId(item2Id)
                .properties(String.format("{\"%1$s\": \"%2$s\"}",
                        item2PropertyName, item2PropertyValue))
                .build();

        items.add(item2);

        PutItemsRequest putItemsRequest = PutItemsRequest.builder()
                .datasetArn(datasetArn)
                .items(items)
                .build();

        responseCode = personalizeEventsClient.putItems(putItemsRequest).sdkHttpResponse().statusCode();
        System.out.println("Response code: " + responseCode);
        return responseCode;

    } catch (PersonalizeEventsException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
    return responseCode;
    }
```

------