# Publishing and viewing results<a name="metric-attribution-results"></a>

 Amazon Personalize sends the reports for each metric to CloudWatch or Amazon S3: 
+ For PutEvents data and incremental bulk data, Amazon Personalize automatically sends metrics to CloudWatch\. For information on viewing and identifying reports in CloudWatch, see [Viewing metrics in CloudWatch](#metric-attribution-results-cloudwatch)\. 
+ For all bulk data, if you provide an Amazon S3 bucket when you create your metric attribution, you can choose to publish metric reports to your Amazon S3 bucket each time you create a dataset import job for interactions data\.

   For information publishing metric reports to Amazon S3, see [Publishing metrics to Amazon S3](#metric-attribution-results-s3)\.

**Topics**
+ [Viewing metrics in CloudWatch](#metric-attribution-results-cloudwatch)
+ [Publishing metrics to Amazon S3](#metric-attribution-results-s3)

## Viewing metrics in CloudWatch<a name="metric-attribution-results-cloudwatch"></a>

**Important**  
 After you create a metric attribution and record events or import incremental bulk data, you will incur some monthly CloudWatch cost per metric\. For information about CloudWatch pricing, see the [Amazon CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/) page\. To stop sending metrics to CloudWatch, [delete the metric attribution](deleting-metric-attribution.md)\.

 To view metrics in CloudWatch, complete the procedure found in [Graphing a metric](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/graph_a_metric.html)\. The minimum **Period** you can graph is 15 minutes\. For the search term, specify the name you gave the metric when you created the metric attribution\. 

The following is an example of how a metric might appear in CloudWatch\. The metric shows the click\-through rate for every 15 minutes for two different recommenders\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/metric-attribution-cw-example.png)

## Publishing metrics to Amazon S3<a name="metric-attribution-results-s3"></a>

To publish metrics to Amazon S3, you provide a path to your Amazon S3 bucket in your metric attribution\. Then you publish reports to Amazon S3 when you create a dataset import job\. 

When the job completes, you can find the metrics in your Amazon S3 bucket\. Each time you publish metrics, Amazon Personalize creates a new file in your Amazon S3 bucket\. The file name includes the import method and date as follows:

`AggregatedAttributionMetrics - ImportMethod - Timestamp.csv`

The following is an example of how the first few rows of a metric report CSV file might appear\. The metric in this example reports on the total clicks from two different recommenders over 15 minute intervals\. Each recommender is identified by its Amazon Resource Name \(ARN\) in the EVENT\_ATTRIBUTION\_SOURCE column\. 

```
METRIC_NAME,EVENT_TYPE,VALUE,MATH_FUNCTION,EVENT_ATTRIBUTION_SOURCE,TIMESTAMP
COUNTWATCHES,WATCH,12.0,samplecount,arn:aws:personalize:us-west-2:acctNum:recommender/recommender1Name,1666925124
COUNTWATCHES,WATCH,112.0,samplecount,arn:aws:personalize:us-west-2:acctNum:recommender/recommender2Name,1666924224
COUNTWATCHES,WATCH,10.0,samplecount,arn:aws:personalize:us-west-2:acctNum:recommender/recommender1Name,1666924224
COUNTWATCHES,WATCH,254.0,samplecount,arn:aws:personalize:us-west-2:acctNum:recommender/recommender2Name,1666922424
COUNTWATCHES,WATCH,112.0,samplecount,arn:aws:personalize:us-west-2:acctNum:recommender/recommender1Name,1666922424
COUNTWATCHES,WATCH,100.0,samplecount,arn:aws:personalize:us-west-2:acctNum:recommender/recommender2Name,1666922424
......
.....
```

### Publishing metrics for bulk data to Amazon S3 \(console\)<a name="metric-attribution-results-s3-console"></a>

To publish metrics to an Amazon S3 bucket with the Amazon Personalize console, create a dataset import job and choose **Publish metrics for this import job** in **Publish event metrics to S3**\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/publish-to-s3.png)

 For step\-by\-step instructions, see [Importing bulk records \(console\)](bulk-data-import-step.md#bulk-data-import-console)\. 

### Publishing metrics for bulk data to Amazon S3 \(AWS CLI\)<a name="metric-attributinon-resuslts-s3-cli"></a>

To publish metrics to an Amazon S3 bucket with the AWS Command Line Interface \(AWS CLI\), use the following code to create a dataset import job and provide the `publishAttributionMetricsToS3` flag\. If you don't want to publish metrics for a particular job, omit the flag\. For information on each parameter, see [CreateDatasetImportJob](API_CreateDatasetImportJob.md)\. 

```
aws personalize create-dataset-import-job \
--job-name dataset import job name \
--dataset-arn dataset arn \
--data-source dataLocation=s3://bucketname/filename \
--role-arn roleArn \
--import-mode INCREMENTAL \
--publish-attribution-metrics-to-s3
```

### Publishing metrics for bulk data to Amazon S3 \(AWS SDKs\)<a name="metric-attributinon-resuslts-s3-sdk"></a>

To publish metrics to an Amazon S3 bucket with the AWS SDKs, create a dataset import job and set `publishAttributionMetricsToS3` to true\. For information on each parameter, see [CreateDatasetImportJob](API_CreateDatasetImportJob.md)\. 

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3

personalize = boto3.client('personalize')

response = personalize.create_dataset_import_job(
    jobName = 'YourImportJob',
    datasetArn = 'dataset_arn',
    dataSource = {'dataLocation':'s3://bucket/file.csv'},
    roleArn = 'role_arn',
    importMode = 'INCREMENTAL',
    publishAttributionMetricsToS3 = True
)

dsij_arn = response['datasetImportJobArn']

print ('Dataset Import Job arn: ' + dsij_arn)

description = personalize.describe_dataset_import_job(
    datasetImportJobArn = dsij_arn)['datasetImportJob']

print('Name: ' + description['jobName'])
print('ARN: ' + description['datasetImportJobArn'])
print('Status: ' + description['status'])
```

------
#### [ SDK for Java 2\.x ]

```
public static String createPersonalizeDatasetImportJob(PersonalizeClient personalizeClient,
                                                      String jobName,
                                                      String datasetArn,
                                                      String s3BucketPath,
                                                      String roleArn,
                                                      ImportMode importMode,
                                                      boolean publishToS3) {

  long waitInMilliseconds = 60 * 1000;
  String status;
  String datasetImportJobArn;
  
  try {
      DataSource importDataSource = DataSource.builder()
              .dataLocation(s3BucketPath)
              .build();
      
      CreateDatasetImportJobRequest createDatasetImportJobRequest = CreateDatasetImportJobRequest.builder()
              .datasetArn(datasetArn)
              .dataSource(importDataSource)
              .jobName(jobName)
              .roleArn(roleArn)
              .importMode(importMode)
              .publishAttributionMetricsToS3(publishToS3)
              .build();
  
      datasetImportJobArn = personalizeClient.createDatasetImportJob(createDatasetImportJobRequest)
              .datasetImportJobArn();
      
      DescribeDatasetImportJobRequest describeDatasetImportJobRequest = DescribeDatasetImportJobRequest.builder()
              .datasetImportJobArn(datasetImportJobArn)
              .build();
  
      long maxTime = Instant.now().getEpochSecond() + 3 * 60 * 60;
  
      while (Instant.now().getEpochSecond() < maxTime) {
  
          DatasetImportJob datasetImportJob = personalizeClient
                  .describeDatasetImportJob(describeDatasetImportJobRequest)
                  .datasetImportJob();
  
          status = datasetImportJob.status();
          System.out.println("Dataset import job status: " + status);
  
          if (status.equals("ACTIVE") || status.equals("CREATE FAILED")) {
              break;
          }
          try {
              Thread.sleep(waitInMilliseconds);
          } catch (InterruptedException e) {
              System.out.println(e.getMessage());
          }
      }
      return datasetImportJobArn;
  
  } catch (PersonalizeException e) {
      System.out.println(e.awsErrorDetails().errorMessage());
  }
  return "";
}
```

------