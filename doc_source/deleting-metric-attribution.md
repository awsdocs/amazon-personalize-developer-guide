# Deleting a metric attribution<a name="deleting-metric-attribution"></a>

If you no longer want to generate reports, you can delete a metric attribution\. Deleting a metric attribution deletes all of its metrics and output configuration\. 

 If you delete a metric attribution, Amazon Personalize stops automatically sending reports related to PutEvents and incremental bulk data to CloudWatch\. Data already sent to CloudWatch or published to Amazon S3 is not affected\. You can delete a metric attribution with the Amazon Personalize console, AWS Command Line Interface, or AWS SDKS\. 

**Topics**
+ [Deleting a metric attribution \(console\)](#deleting-metric-attribution-console)
+ [Deleting a metric attribution \(AWS CLI\)](#deleting-metric-attribution-cli)
+ [Deleting a metric attribution \(AWS SDKs\)](#deleting-metric-attribution-sdk)

## Deleting a metric attribution \(console\)<a name="deleting-metric-attribution-console"></a>

You delete a metric attribution on the overview page for your metric attribution\.

**To delete a metric attribution**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\. 

1. Choose your dataset group\.

1. In the navigation pane, choose **Metric attribution**\.

1. Choose **Delete** and then confirm the deletion\.

## Deleting a metric attribution \(AWS CLI\)<a name="deleting-metric-attribution-cli"></a>

To delete a metric attribution with the AWS CLI, use the `delete-metric-attribution` command as follows\.

```
aws personalize delete-metric-attribution --metric-attribution-arn metric attribution ARN
```

## Deleting a metric attribution \(AWS SDKs\)<a name="deleting-metric-attribution-sdk"></a>

 The following code shows how to delete a metric attribution with the SDK for Python \(Boto3\):

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3
            
personalize = boto3.client('personalize')

response = personalize.delete_metric_attribution(
  metricAttributionArn = 'metric attribution ARN'
)
```

------
#### [ SDK for Java 2\.x ]

```
public static void deleteMetricAttribution(PersonalizeClient client, String metricAttributionArn) {

    try {
    
        DeleteMetricAttributionRequest request = DeleteMetricAttributionRequest.builder()
                .metricAttributionArn(metricAttributionArn)
                .build();
                
        DeleteMetricAttributionResponse response = client.deleteMetricAttribution(request);
        if (response.sdkHttpResponse().statusCode() == 200) {
            System.out.println("Metric attribution deleted!");
        }
        
    } catch (PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
}
```

------