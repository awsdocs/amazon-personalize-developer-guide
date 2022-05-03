# Stopping and starting a recommender<a name="stopping-starting-recommender"></a>

After your recommender is active, you can stop a recommender and start it later\. This way, you can pause recommender billing and only pay for it when you use it\. For example, you might need to get recommendations only on certain days of the week\. You can stop the recommender on the days you don't need it, and then start the recommender on the days you do\. 

After you stop a recommender, you can't use it to get recommendations\. Stopping a recommender halts recommender billing and retraining\. However, stopping a recommender doesn't delete the recommender\. You can restart it at any time and resume getting recommendations\. Starting a recommender doesn't create a new recommender with your data\. Rather, it resumes recommender billing and retraining every 7 days\. 

You can stop and start a recommender with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), AWS SDKs, or the [StartRecommender](API_StartRecommender.md) and [StopRecommender](API_StopRecommender.md) API operations\. 

**Recommender states**

When you stop a recommender, the recommender state changes from ACTIVE to INACTIVE in the following sequence: 

ACTIVE > STOP PENDING > STOP IN PROGRESS > INACTIVE

When you start a recommender, the recommender state changes from INACTIVE to ACTIVE in the following sequence: 

INACTIVE > START PENDING > START IN PROGRESS > ACTIVE

**Topics**
+ [Stopping and starting a recommender \(console\)](#stop-start-recommender-console)
+ [Stopping and restarting a recommender \(AWS CLI\)](#stop-start-recommender-cli)
+ [Stopping and restarting a recommender \(AWS SDKs\)](#stop-start-recommender-sdks)

## Stopping and starting a recommender \(console\)<a name="stop-start-recommender-console"></a>

You can use the Amazon Personalize to stop and restart a recommender\.

**Topics**
+ [Stopping a recommender \(console\)](#stopping-recommender-console)
+ [Starting a recommender \(console\)](#start-recommender-console)

### Stopping a recommender \(console\)<a name="stopping-recommender-console"></a>

You can use the Amazon Personalize console to stop an active recommender as follows\.

**To stop a recommender**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1.  On the **Dataset groups** page, choose your Domain dataset group\. 

1. From the navigation pane, choose **Recommenders**\.

1. On the **Recommenders** page, choose the recommender that you want to stop\.

1. Choose **Stop recommender** at the top right and confirm on the window that displays\.

   When the recommender status is inactive, your recommender has stopped\. This halts any recommender billing and retraining\. You can't use the recommender until you start it\.

### Starting a recommender \(console\)<a name="start-recommender-console"></a>

You can use the Amazon Personalize console to start an inactive recommender as follows\.

**To start a recommender**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. On the **Dataset groups** page, choose your Domain dataset group\. 

1. From the navigation pane, choose **Recommenders**\.

1. On the **Recommenders** page, choose the recommender that you want to start\.

1. Choose **Start recommender** at the top right and confirm that you want to start it on the window that displays\.

   When the recommender status is active, you can resume getting recommendations from it\. Recommender billing and automatic retraining also resumes\.

## Stopping and restarting a recommender \(AWS CLI\)<a name="stop-start-recommender-cli"></a>

To stop an active recommender with the AWS CLI, use the `stop-recommender` command and provide the Amazon Resource Name \(ARN\) for the recommender as follows:

```
aws personalize stop-recommender --recommender-arn "recommender arn"
```

To start an inactive recommender with the AWS CLI, use the `start-recommender` command and provide the ARN for the stopped recommender as follows:

```
aws personalize start-recommender --recommender-arn "recommender arn"
```

For more information about the API operations, see [StartRecommender](API_StartRecommender.md) and [StopRecommender](API_StopRecommender.md)\.

## Stopping and restarting a recommender \(AWS SDKs\)<a name="stop-start-recommender-sdks"></a>

You can use the AWS SDKs to start an active recommender or stop an inactive recommender\. For more information about the API operations, see [StartRecommender](API_StartRecommender.md) and [StopRecommender](API_StopRecommender.md)\.

**Topics**
+ [Stopping a recommender \(AWS SDKs\)](#stop-recommender-sdk)
+ [Starting a recommender \(AWS SDKs\)](#start-recommender-sdk)

### Stopping a recommender \(AWS SDKs\)<a name="stop-recommender-sdk"></a>

The following code shows how to stop an active recommender with the AWS SDKs\. Stopping halts any recommender billing and automatic retraining\. You can't use the recommender until you restart it\.

------
#### [ SDK for Python \(Boto3\) ]

To stop an active recommender with the SDK for Python \(Boto3\), use the `stop_recommender` method and provide the Amazon Resource Name \(ARN\) for the recommender as follows\.

```
import boto3
personalize = boto3.client('personalize')

stop_recommender_response = personalize stop_recommender(
    recommenderArn = "recommenderARN"
)
print(stop_recommender_response)
```

------
#### [ SDK for Java 2\.x ]

To stop an active recommender with the SDK for Java 2\.x, use the `stopRecommender` method and provide the ARN for the recommender as follows\.

```
public static void stopRecommender(PersonalizeClient personalizeClient, 
                                              String datasetGroupArn) {
    
    try {
    
        StopRecommenderRequest stopRecommenderRequest = StopRecommenderRequest.builder()
                .recommenderArn(recommenderArn)
                .build();
        personalizeClient.stopRecommender(stopRecommenderRequest);
    } catch (PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
    return "";
}
```

------

### Starting a recommender \(AWS SDKs\)<a name="start-recommender-sdk"></a>

The following code shows how to start an inactive recommender with the AWS SDKs\. When the recommender status is active, you can resume getting recommendations from it\. At the same time, recommender billing and automatic retraining also resumes\.

------
#### [ SDK for Python \(Boto3\) ]

To start an inactive recommender with the SDK for Python \(Boto3\), use the `start_recommender` method and provide the Amazon Resource Name \(ARN\) for the recommender as follows\.

```
import boto3
personalize = boto3.client('personalize')

start_recommender_response = personalize start_recommender(
    recommenderArn = "recommenderARN"
)
print(start_recommender_response)
```

------
#### [ SDK for Java 2\.x ]

To start an inactive recommender with the SDK for Java 2\.x, use the `startRecommender` method and provide the ARN for the recommender as follows\.

```
public static void startRecommender(PersonalizeClient personalizeClient, 
                                              String datasetGroupArn) {
    
    try {
    
        StartRecommenderRequest startRecommenderRequest = StartRecommenderRequest.builder()
                .recommenderArn(recommenderArn)
                .build();
        personalizeClient.startRecommender(startRecommenderRequest);
    } catch (PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
    return "";
}
```

------