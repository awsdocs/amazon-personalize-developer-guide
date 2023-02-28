# Creating a metric attribution<a name="creating-metric-attribution"></a>

**Important**  
 After you create a metric attribution and record events or import incremental bulk data, you will incur some monthly CloudWatch cost per metric\. For information about CloudWatch pricing, see the [Amazon CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/) page\. To stop sending metrics to CloudWatch, [delete the metric attribution](deleting-metric-attribution.md)\. 

To start generating metric reports, you create a metric attribution and import interactions data\. When you create a metric attribution, you specify a list of event types to report on\. For each event type, you specify a function that Amazon Personalize applies as it collects the data\. Available functions include `SUM(DatasetType.COLUMN_NAME)` and `SAMPLECOUNT()`\. 

 For example, you might have an online video streaming app and want to track two metrics: the click\-through rate for recommendations, and the total length of movies watched, where each video in the Items dataset includes a `LENGTH` attribute\. You would create a metric attribution and add two metrics, each with an event type and function\. The first might be for the `Click` event type with a `SAMPLECOUNT()` function\. The second might be for the `Watch` event type with a `SUM(Items.LENGTH)` function\. 

You can apply `SUM()` functions to only numeric columns of Items and Interactions datasets\. To apply a `SUM()` function to a column in an Items dataset, you must first import item metadata\.

 You can create a metric attribution with the Amazon Personalize console, AWS Command Line Interface, or AWS SDKS\.

**Topics**
+ [Creating a metric attribution \(console\)](#create-metric-attribution-console)
+ [Creating a metric attribution \(AWS CLI\)](#create-metric-attribution-cli)
+ [Creating a metric attribution \(AWS SDKs\)](#create-metric-attribution-sdk)

## Creating a metric attribution \(console\)<a name="create-metric-attribution-console"></a>

 To create a metric attribution with the Amazon Personalize console, you navigate to the **Metric attribution** page and choose **Create metric attribution**\. When you create a metric attribution, you specify an optional Amazon S3 bucket path, your Amazon Personalize IAM service role, and a list of metrics to report on\. 

 When you create an Interactions dataset import job with the Amazon Personalize console, you have the option to create a metric attribution in a new tab\. Then you can return to the import job to complete it\. If you're already on the **Configure metric attribution** page, you can skip to step 4\. 

**To create a metric attribution**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\. 

1. Choose your dataset group\.

1. In the navigation pane, choose **Metric attribution**\.

1. In **Metric attribution details**, choose **Create metric attribution**\.

1. On the **Configure metric attribution** page, give the metric attribution a name\.

1. If you want to publish metrics to Amazon S3 for **Amazon S3 data output path**, enter the destination Amazon S3 bucket\. This enables the option to publish metrics each time you create a dataset import job\. Use the following syntax:

   **s3://<name of your S3 bucket>/<folder> path>**

1. If you are using AWS KMS for encryption, for **KMS key ARN**, enter the Amazon Resource Name \(ARN\) for the AWS KMS key\. You must grant Amazon Personalize and your Amazon Personalize IAM service role permission to use your key\. For more information, see [Giving Amazon Personalize permission to use your AWS KMS key](granting-personalize-key-access.md)\.

1. In **IAM role**, choose to create a new service role or use an existing one\. The role you choose must have `PutMetricData` permissions for CloudWatch\. If you want to publish to Amazon S3, the role must have `PutObject` permissions for your Amazon S3 bucket\. 

   To use the role that you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions), you might have to add policies for CloudWatch and Amazon S3\.

   For policy examples, see [Giving Amazon Personalize access to CloudWatch](metric-attribution-requirements.md#metric-attribution-cw-permissions) and [Giving Amazon Personalize access to your Amazon S3 bucket](metric-attribution-requirements.md#metric-attribution-s3-permissions)\.

1. Choose **Next**\.

1. On the **Define metric attributes** page, choose how to define metrics\. Choose **Build metric attributes** to use the builder tool\. Choose **Input metric attributes** to enter metrics in JSON format\.
   + If you choose **Build metric attributes**, for each metric provide a name, event type, and choose a function\. For `SUM()` functions, choose the column name\. Choose **Add metric attribute** to add additional metrics\. 
   + If you choose **Input metric attributes**, enter each metric in JSON format\. The following shows how to format a metric\.

     ```
     {
         "EventType": "watch",
         "MetricName": "MinutesWatchedTracker", 
         "MetricMathExpression": "SUM(Items.LENGTH)"
     }
     ```

1. Choose **Next**\.

1. On the **Review and create page**, review the details for the new metric attribution\. To make changes, choose **Previous**\. To create the metric attribution, choose **Create**\. When the metric attribution is active, you can start importing data and view the results\. For information on viewing results, see [Publishing and viewing results](metric-attribution-results.md)\. 

## Creating a metric attribution \(AWS CLI\)<a name="create-metric-attribution-cli"></a>

 The following code shows how to create a metric attribution with the AWS Command Line Interface\. The role you specify must have `PutMetricData` permissions for CloudWatch and, if publishing to Amazon S3, `PutObject` permissions for your Amazon S3 bucket\. To use the role that you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions), you might have to add policies for CloudWatch and Amazon S3\. For policy examples, see [Giving Amazon Personalize access to CloudWatch](metric-attribution-requirements.md#metric-attribution-cw-permissions) and [Giving Amazon Personalize access to your Amazon S3 bucket](metric-attribution-requirements.md#metric-attribution-s3-permissions)\. 

 For each metric specify a name, event type, and expression \(a function\)\. Available functions include `SUM(DatasetType.COLUMN_NAME)` and `SAMPLECOUNT()`\. For SUM\(\) functions, specify the dataset type and column name\. For example, `SUM(Items.LENGTH)`\. For information on each parameter, see [CreateMetricAttribution](API_CreateMetricAttribution.md)\. 

```
aws personalize create-metric-attribution \
--name metric attribution name \
--dataset-group-arn dataset group arn \
--metrics-output-config "{\"roleArn\": \"Amazon Personalize service role ARN\", \"s3DataDestination\":{\"kmsKeyArn\":\"kms key ARN\",\"path\":\"s3://bucket-name/folder-name/\"}}" \
--metrics "[{
  \"eventType\": \"event type\",
  \"expression\": \"SUM(DatasetType.COLUMN_NAME)\",
  \"metricName\": \"metric name\"
}]"
```

## Creating a metric attribution \(AWS SDKs\)<a name="create-metric-attribution-sdk"></a>

 The following code shows how to create a metric attribution with the SDK for Python \(Boto3\)\. The role you specify must have `PutMetricData` permissions for CloudWatch and, if publishing to Amazon S3, `PutObject` permissions for your Amazon S3 bucket\. To use the role that you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions), you might have to add policies for CloudWatch and Amazon S3\. For policy examples, see [Giving Amazon Personalize access to CloudWatch](metric-attribution-requirements.md#metric-attribution-cw-permissions) and [Giving Amazon Personalize access to your Amazon S3 bucket](metric-attribution-requirements.md#metric-attribution-s3-permissions)\. 

 For each metric specify a name, event type, and expression \(a function\)\. Available functions include `SUM(DatasetType.COLUMN_NAME)` and `SAMPLECOUNT()`\. For SUM\(\) functions, specify the dataset type and column name\. For example, `SUM(Items.LENGTH)`\. For information on each parameter, see [CreateMetricAttribution](API_CreateMetricAttribution.md)\. 

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3

personalize = boto3.client('personalize')

metricsList = [{ 
      "eventType": "event type",
      "expression": "SUM(DatasetType.COLUMN_NAME)",
      "metricName": "metric name"
}]

outputConfig = {
  "roleArn": "Amazon Personalize service role ARN", 
  "s3DataDestination": {
    "kmsKeyArn": "key ARN", 
    "path": "s3://<name of your S3 bucket>/<folder>"
  }
}
response = personalize.create_metric_attribution(
  name = 'metric attribution name',
  datasetGroupArn = 'dataset group arn',
  metricsOutputConfig = outputConfig,
  metrics = metricsList
)

metric_attribution_arn = response['metricAttributionArn']

print ('Metric attribution ARN: ' + metric_attribution_arn)

description = personalize.describe_metric_attribution(
    metricAttributionArn = metric_attribution_arn)['metricAttribution']

print('Name: ' + description['name'])
print('ARN: ' + description['metricAttributionArn'])
print('Status: ' + description['status'])
```

------
#### [ SDK for Java 2\.x ]

```
public static String createMetricAttribution(PersonalizeClient personalizeClient,
                                             String eventType,
                                             String expression,
                                             String metricName,
                                             String metricAttributionName,
                                             String roleArn,
                                             String s3Path,
                                             String kmsKeyArn,
                                             String datasetGroupArn) {
    String metricAttributionArn = "";

    try {

        MetricAttribute attribute = MetricAttribute.builder()
                .eventType(eventType)
                .expression(expression)
                .metricName(metricName)
                .build();

        ArrayList<MetricAttribute> metricAttributes = new ArrayList<>();
        metricAttributes.add(attribute);

        S3DataConfig s3DataDestination = S3DataConfig.builder()
                .kmsKeyArn(kmsKeyArn)
                .path(s3Path)
                .build();

        MetricAttributionOutput outputConfig = MetricAttributionOutput.builder()
                .roleArn(roleArn)
                .s3DataDestination(s3DataDestination)
                .build();

        CreateMetricAttributionRequest createMetricAttributionRequest = CreateMetricAttributionRequest.builder()
                .name(metricAttributionName)
                .datasetGroupArn(datasetGroupArn)
                .metrics(metricAttributes)
                .metricsOutputConfig(outputConfig)
                .build();
        CreateMetricAttributionResponse createMetricAttributionResponse = personalizeClient.createMetricAttribution(createMetricAttributionRequest);

        metricAttributionArn = createMetricAttributionResponse.metricAttributionArn();
        System.out.println("Metric attribution ARN: " + metricAttributionArn);
        return metricAttributionArn;
    } catch (PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
    return "";
}
```

------