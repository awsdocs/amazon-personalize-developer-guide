# Importing users individually<a name="importing-users"></a>

 After you create a Users dataset, you can individually import one or more new users into the dataset\. Individually importing users allows you to keep your Users dataset current with small batch imports as your catalog grows\. You can import up to 10 users at a time\. If you have a large amount of new users, we recommend that you first import data in bulk and then import user data individually as necessary\. See [Importing bulk records](bulk-data-import.md)\. 

You can use the Amazon Personalize console, the AWS Command Line Interface \(AWS CLI\), or AWS SDKs to import users\. If you import a user with the same `userId` as a user that's already in your Users dataset, Amazon Personalize replaces the user with the new one\. You can import up to 10 users at a time\.

For information about how Amazon Personalize updates filters for new records and how new records influence recommendations, see [Importing individual records](incremental-data-updates.md)\. 

**Topics**
+ [Importing users individually \(console\)](#importing-users-console)
+ [Importing users individually \(AWS CLI\)](#importing-users-cli)
+ [Importing users individually \(AWS SDKs\)](#importing-users-sdk)

## Importing users individually \(console\)<a name="importing-users-console"></a>

You can import up to 10 users at a time\. This procedure assumes you have already created a Users dataset\. For information about creating datasets, see [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md)\.

**To import users individually \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. On the **Dataset groups** page, choose the dataset group with the Users dataset that you want to import the user to\. 

1. In the navigation pane, choose **Datasets**\. 

1. On the **Datasets** page, choose the Users dataset\. 

1. On the dataset details page, at the top right, choose **Modify dataset** and choose **Create record**\. 

1. On the **Create user record\(s\)** page, for record input, enter the user details in JSON format\. The user's field names and values must match the schema you used when you created the Users dataset\. Amazon Personalize provides a JSON template with field names and data types from this schema\. 

1. Choose **Create record\(s\)**\. In **Response**, the result of the import is listed and a success or failure message is displayed\.

## Importing users individually \(AWS CLI\)<a name="importing-users-cli"></a>

Add one or more users to your Users dataset with the [PutUsers](API_UBS_PutUsers.md) operation\. You can import up to 10 users with a single `PutUsers` call\. This section assumes that you have already created an Users dataset\. For information about creating datasets, see [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md)\.

Use the following `put-users` command to add one or more users with the AWS CLI\. Replace `dataset arn` with the Amazon Resource Name \(ARN\) of your dataset and `user Id` with the ID of the user\. If an user with the same `userId` is already in your Users dataset, Amazon Personalize replaces it with the new one\.

For `properties`, for each field in your Users dataset, replace the `propertyName` with the field name from your schema in camel case\. For example, GENDER would be `gender` and MEMBERSHIP\_TYPE would be `membershipType`\. Replace `user data` with the data for the user\. For categorical string data, to include multiple categories for a single property, separate each category with a pipe \(`|`\)\. For example `\"Premium Class|Legacy Member\"`\.

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

## Importing users individually \(AWS SDKs\)<a name="importing-users-sdk"></a>

Add one or more users to your Users dataset with the [PutUsers](API_UBS_PutUsers.md) operation\. If a user with the same `userId` is already in your Users dataset, Amazon Personalize replaces it with the new one\. You can import up to 10 users with a single `PutUsers` call\. This section assumes that you have already created a Users dataset\. For information about creating datasets, see [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md)\.

 The following code shows how to add one or more users to your Users dataset\. For each property name parameter, pass the field name from your schema in camel case\. For example, GENDER would be `gender` and MEMBERSHIP\_TYPE would be `membershipType`\. For each property value parameter, pass the data for the user\. 

For categorical string data, to include multiple categories for a single property separate each category with a pipe \(`|`\)\. For example `"Premium class|Legacy Member"`\.

------
#### [ SDK for Python \(Boto3\) ]

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
#### [ SDK for Java 2\.x ]

```
public static int putUsers(PersonalizeEventsClient personalizeEventsClient,
                         String datasetArn,
                         String user1Id,
                         String user1PropertyName,
                         String user1PropertyValue,
                         String user2Id,
                         String user2PropertyName,
                         String user2PropertyValue) {

    int responseCode = 0;
    ArrayList<User> users = new ArrayList<>();

    try {
        User user1 = User.builder()
          .userId(user1Id)
          .properties(String.format("{\"%1$s\": \"%2$s\"}", user1PropertyName, user1PropertyValue))
          .build();

        users.add(user1);

        User user2 = User.builder()
          .userId(user2Id)
          .properties(String.format("{\"%1$s\": \"%2$s\"}", user2PropertyName, user2PropertyValue))
          .build();

        users.add(user2);

        PutUsersRequest putUsersRequest = PutUsersRequest.builder()
          .datasetArn(datasetArn)
          .build();

        responseCode = personalizeEventsClient.putUsers(putUsersRequest).sdkHttpResponse().statusCode();
        System.out.println("Response code: " + responseCode);
        return responseCode;

    } catch (PersonalizeEventsException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
    return responseCode;
}
```

------
#### [ SDK for JavaScript v3 ]

```
// Get service clients module and commands using ES6 syntax.
import { PutUsersCommand } from
  "@aws-sdk/client-personalize-events";
import { personalizeEventsClient } from "./libs/personalizeClients.js";
// Or, create the client here.
// const personalizeEventsClient = new PersonalizeEventsClient({ region: "REGION"});

// Set the put users parameters. For string properties and values, use the \ character to escape quotes.
var putUsersParam = {
    datasetArn: "DATASET_ARN",
    users: [ 
      {
        'userId': 'USER_ID',
        'properties': "{\"PROPERTY1_NAME\": \"PROPERTY1_VALUE\"}"   
      }
    ]
};
export const run = async () => {
  try {
    const response = await personalizeEventsClient.send(new PutUsersCommand(putUsersParam));
    console.log("Success!", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

------