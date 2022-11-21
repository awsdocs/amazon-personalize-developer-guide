# Updating existing bulk data<a name="updating-existing-bulk-data"></a>

If you previously created a dataset import job for a dataset, you can use another import job to add to or replace the existing bulk data\. By default, a dataset import job replaces any existing data in the dataset that you imported in bulk\. You can instead append the new records to existing data by changing the job's [import mode](#bulk-import-modes)\. To append data to an Interactions dataset with a dataset import job, you must have at minimum 1000 new interaction records\.

**Filter updates for bulk records**

Within 15 minutes of completing a bulk import, Amazon Personalize updates any filters you created in the dataset group with your new item and user data\. This update allows Amazon Personalize to use the most recent data when filtering recommendations for your users\. 

**How new bulk records influence recommendations**

If you already created a solution version \(trained a model\), new bulk records influence recommendations as follows:
+  For *new interactions*, you must create a new solution version for bulk interactions data to influence recommendations\. 
+ For *new items*, if you trained the solution version with User\-Personalization, Amazon Personalize automatically updates the model every two hours\. After each update, the new items might be included in recommendations with exploration\. For information about automatic updates see [Automatic updates](native-recipe-new-item-USER_PERSONALIZATION.md#automatic-updates)\. 

   For any other recipe, you must create a new solution version for the new items to be included in recommendations\. 
+ For *new users* without interactions data, recommendations are initially for only popular items\. To get relevant recommendations for a new user, you can import bulk interactions for the user and create a new solution version\. Or you can record events for the user in real time as they interact with your catalog\. Their recommendations will be more relevant as you record more events\. For more information, see [Recording events](recording-events.md)\. 

**Topics**
+ [Import modes](#bulk-import-modes)
+ [Updating bulk records \(console\)](#updating-bulk-data-console)
+ [Updating bulk records \(AWS CLI\)](#updating-bulk-data-cli)
+ [Updating bulk records \(AWS SDKs\)](#updating-bulk-data-sdk)

## Import modes<a name="bulk-import-modes"></a>

To configure how Amazon Personalize adds your new records to your dataset, you specify an import mode for your dataset import job:
+ To overwrite all existing bulk data in your dataset, choose **Replace existing data** in the Amazon Personalize console or specify FULL in the [CreateDatasetImportJob](API_CreateDatasetImportJob.md) API operation\. This doesn't replace data you imported individually, including events recorded in real time\.
+ To append the records to the existing data in your dataset, choose **Add to existing data** or specify `INCREMENTAL` in the CreateDatasetImportJob API operation\. Amazon Personalize replaces any record with the same ID with the new one\.

  To append data to an Interactions dataset with a dataset import job, you must have at minimum 1000 new interaction records\.

 If you haven't imported bulk records, the option is not available in the console and you can only specify `FULL` in the CreateDatasetImportJob API operation\. The default is a full replacement\. 

## Updating bulk records \(console\)<a name="updating-bulk-data-console"></a>

**Important**  
By default, a dataset import job replaces any existing data in the dataset that you imported in bulk\. You can change this by specifying the job's [import mode](#bulk-import-modes)\.

To update bulk data with the Amazon Personalize console, create a dataset import job for the dataset and specify an import mode\.

**To update bulk records \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1.  On the **Dataset groups** page, choose your dataset group\.

1. From the navigation pane, choose **Datasets**\.

1. On the **Datasets** page, choose the dataset you want to update\.

1. In **Dataset import jobs**, choose **Create dataset import job**\.

1. In **Import job details**, for **Dataset import job name**, specify a name for your import job\.

1. For **Import mode**, choose how to update the dataset\. Choose either **Replace existing data** or **Add to existing data**\. data\. For more information see [Import modes](#bulk-import-modes)\.

1. In **Input source**, for **S3 Location**, specify where your data file is stored in Amazon S3\. Use the following syntax:

   **s3://<name of your S3 bucket>/<folder path>/<CSV file name>**

   If your CSV files are in a folder in your S3 bucket and you want to upload multiple CSV files to a dataset with one dataset import job, use this syntax without the CSV file name\. 

1. In **IAM role**, choose to either create a new role or use an existing one\. If you completed the prerequisites, choose **Use an existing service role** and specify the role that you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\. 

1. For **Tags**, optionally add any tags\. For more information about tagging Amazon Personalize resources, see [Tagging Amazon Personalize resources](tagging-resources.md)\.

1. Choose **Finish**\. The data import job starts and the **Dataset overview** page displayed\. The dataset import is complete when the status is ACTIVE\.

## Updating bulk records \(AWS CLI\)<a name="updating-bulk-data-cli"></a>

**Important**  
By default, a dataset import job replaces any existing data in the dataset that you imported in bulk\. You can change this by specifying the job's [import mode](#bulk-import-modes)\.

To update bulk data, use the `create-dataset-import-job` command\. For the `import-mode`, specify `FULL` to replace existing data or `INCREMENTAL` to add to it\. For more information see [Import modes](#bulk-import-modes)\.

The following code shows how to create a dataset import job that incrementally imports data into a dataset\.

```
aws personalize create-dataset-import-job \
--job-name dataset import job name \
--dataset-arn dataset arn \
--data-source dataLocation=s3://bucketname/filename \
--role-arn roleArn \
--import-mode INCREMENTAL
```

## Updating bulk records \(AWS SDKs\)<a name="updating-bulk-data-sdk"></a>

**Important**  
By default, a dataset import job replaces any existing data in the dataset that you imported in bulk\. You can change this by specifying the job's [import mode](#bulk-import-modes)\.

To update bulk data, create a dataset import job and specify an import mode\. The following code show's how to update bulk data in Amazon Personalize with the SDK for Python \(Boto3\) or SDK for Java 2\.x\.

------
#### [ SDK for Python \(Boto3\) ]

To update bulk data, use the `create_dataset_import_job` method\. For the `import-mode`, specify `FULL` to replace existing data or `INCREMENTAL` to add to it\. For more information see [Import modes](#bulk-import-modes)\.

```
import boto3

personalize = boto3.client('personalize')

response = personalize.create_dataset_import_job(
    jobName = 'YourImportJob',
    datasetArn = 'dataset_arn',
    dataSource = {'dataLocation':'s3://bucket/file.csv'},
    roleArn = 'role_arn',
    importMode = 'INCREMENTAL'
)
```

------
#### [ SDK for Java 2\.x ]

To update bulk data, use the following `createPersonalizeDatasetImportJob` method\. For the `importImport`, specify `ImportMode.FULL` to replace existing data or `ImportMode.INCREMENTAL` to add to it\. For more information see [Import modes](#bulk-import-modes)\.

```
public static String createPersonalizeDatasetImportJob(PersonalizeClient personalizeClient,
                                                      String jobName,
                                                      String datasetArn,
                                                      String s3BucketPath,
                                                      String roleArn,
                                                      ImportMode importMode) {

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