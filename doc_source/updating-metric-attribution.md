# Updating a metric attribution<a name="updating-metric-attribution"></a>

 When you update a metric attribution, you can add and remove metrics and modify its output configuration\. You can update a metric attribution with the Amazon Personalize console, AWS Command Line Interface, or AWS SDKS\. 

**Topics**
+ [Updating a metric attribution \(console\)](#updating-metric-attribution-console)
+ [Updating a metric attribution \(AWS CLI\)](#updating-metric-attribution-cli)
+ [Updating a metric attribution \(AWS SDK\)](#updating-metric-attribution-sdk)

## Updating a metric attribution \(console\)<a name="updating-metric-attribution-console"></a>

To update a metric attribution with the Amazon Personalize console, you make your changes on the **Metric attribution** page\.

**To update a metric attribution**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\. 

1. Choose your dataset group\.

1. In the navigation pane, choose **Metric attribution**\.

1. In the bottom section, choose the **Metric attributes** tab or **Metric attribution configuration** tab to start making changes\.
   + To add or remove metrics, choose the **Metric attributes** tab and choose **Edit attributes**\. Make your changes on the **Edit metric attributes** page and choose **Update** to save your changes\.
   + To make changes to the Amazon S3 output bucket or IAM service role, choose the **Edit metric attribution configuration** tab and make changes on the **Edit attribution configuration** page\. Choose **Update** to save your changes\.

## Updating a metric attribution \(AWS CLI\)<a name="updating-metric-attribution-cli"></a>

After you create a metric attribution, you can use the AWS Command Line Interface \(AWS CLI\) to add and remove metrics and modify its output configuration\. The following code shows how to remove metrics with the `update-metric-attribution` command:

```
aws personalize update-metric-attribution \
--metric-attribution-arn metric attribution arn \
--remove-metrics metricName1 metricName2
```

 The following code shows how to add an additional metric and specify a new output configuration:

```
aws personalize update-metric-attribution \
--metric-attribution-arn metric attribution arn \
--metrics-output-config "{\"roleArn\": \"new role ARN\", \"s3DataDestination\":{\"kmsKeyArn\":\"kms key ARN\",\"path\":\"s3://new-bucket-name/new-folder-name/\"}}" \
--add-metrics "[{
  \"eventType\": \"event type\",
  \"expression\": \"SUM(DatasetType.COLUMN_NAME)\",
  \"metricName\": \"metric name\"
}]"
```

 If successful, Amazon Personalize returns the ARN of the metric attribution you updated\. For a complete listing of all parameters, see [UpdateMetricAttribution](API_UpdateMetricAttribution.md)\.

## Updating a metric attribution \(AWS SDK\)<a name="updating-metric-attribution-sdk"></a>

After you create a metric attribution, you can add or remove metrics and modify its output configuration\. The following code shows how to remove metrics from a metric attribution\.

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3
            
personalize = boto3.client('personalize')

metricsToRemove = ["metricName1", "metricName2"]
            
response = personalize.update_metric_attribution(
  metricAttributionArn = "metric attribution ARN",
  removeMetrics = metricsToRemove
)
```

------
#### [ SDK for Java 2\.x ]

```
public static void removeMetrics(PersonalizeClient client,
                                 String metricAttributionArn,
                                 String metric1Name,
                                 String metric2Name) {

    ArrayList<String> metricsToRemove = new ArrayList<>(Arrays.asList(metric1Name, metric2Name));
    
    try {
    
        UpdateMetricAttributionRequest request = UpdateMetricAttributionRequest.builder()
                .metricAttributionArn(metricAttributionArn)
                .removeMetrics(metricsToRemove)
                .build();
                
        UpdateMetricAttributionResponse response = client.updateMetricAttribution(request);
        System.out.println(response);
        
    } catch (PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
}
```

------

 The following code shows how to add an additional metric and specify a new output configuration:

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3

personalize = boto3.client('personalize')

newMetrics = [{ 
      "eventType": "event type",
      "expression": "SUM(DatasetType.COLUMN_NAME)",
      "metricName": "metric name"
}]

newOutputConfig = {
  "roleArn": "Amazon Personalize service role ARN", 
  "s3DataDestination": {
    "kmsKeyArn": "key ARN", 
    "path": "s3://<name of your S3 bucket>/<folder>"
  }
}

response = personalize.update_metric_attribution(
  metricAttributionArn = "metric attribution arn",
  metricsOutputConfig = newOutputConfig,
  addMetrics = newMetrics
)
```

------
#### [ SDK for Java 2\.x ]

```
public static void addMetricsAndUpdateOutputConfig(PersonalizeClient personalizeClient,
                                                String metricAttributionArn,
                                                String newMetric1EventType,
                                                String newMetric1Expression,
                                                String newMetric1Name,
                                                String newMetric2EventType,
                                                String newMetric2Expression,
                                                String newMetric2Name,
                                                String roleArn,
                                                String s3Path,
                                                String kmsKeyArn) {
    try {
    
        MetricAttribute newAttribute = MetricAttribute.builder()
                .eventType(newMetric1EventType)
                .expression(newMetric1Expression)
                .metricName(newMetric1Name)
                .build();
                
        MetricAttribute newAttribute2 = MetricAttribute.builder()
                .eventType(newMetric2EventType)
                .expression(newMetric2Expression)
                .metricName(newMetric2Name)
                .build();

        ArrayList<MetricAttribute> newAttributes = new ArrayList<>(Arrays.asList(newAttribute, newAttribute2));

        S3DataConfig newDataDestination = S3DataConfig.builder()
                .kmsKeyArn(kmsKeyArn)
                .path(s3Path)
                .build();

        MetricAttributionOutput newOutputConfig = MetricAttributionOutput.builder()
                .roleArn(roleArn)
                .s3DataDestination(newDataDestination)
                .build();
                
        UpdateMetricAttributionRequest request = UpdateMetricAttributionRequest.builder()
                .metricAttributionArn(metricAttributionArn)
                .metricsOutputConfig(newOutputConfig)
                .addMetrics(newAttributes)
                .build();
                
        UpdateMetricAttributionResponse response = personalizeClient.updateMetricAttribution(request);
        System.out.println("New metrics added!");
        System.out.println(response);
        
    } catch (PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
}
```

------

If successful, Amazon Personalize returns the ARN of the metric attribution you updated\. For a complete listing of all parameters, see [UpdateMetricAttribution](API_UpdateMetricAttribution.md)\.