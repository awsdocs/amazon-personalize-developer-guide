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

After you create a metric attribution, you can add or remove metrics and modify its output configuration\. The following code shows how to remove metrics with the SDK for Python \(Boto3\):

```
import boto3
            
personalize = boto3.client('personalize')

metricsToRemove = ["metricName1", "metricName2"]
            
response = personalize.update_metric_attribution(
  metricAttributionArn = "metric attribution ARN",
  removeMetrics = metricsToRemove
)
```

 The following code shows how to add an additional metric and specify a new output configuration with the SDK for Python \(Boto3\):

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

If successful, Amazon Personalize returns the ARN of the metric attribution you updated\. For a complete listing of all parameters, see [UpdateMetricAttribution](API_UpdateMetricAttribution.md)\.