# Importing bulk records with a dataset import job<a name="bulk-data-import-step"></a>

**Important**  
Bulk imports in Amazon Personalize are a full refresh of bulk data\. Existing bulk data in the dataset is replaced\. This does not include records imported incrementally\.

After you have formatted your input data \(see [Formatting your input data](data-prep-formatting.md)\) and uploaded it to an Amazon Simple Storage Service \(Amazon S3\) bucket \(see [Uploading to an Amazon S3 bucket](data-prep-upload-s3.md)\), import the bulk records by creating a dataset import job\. 

A *dataset import job* is a bulk import tool that populates your dataset with data from your S3 bucket\. You create a dataset import job and import bulk records using the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\.

**Topics**
+ [Importing bulk records \(console\)](#bulk-data-import-console)
+ [Importing bulk records \(AWS CLI\)](#bulk-data-import-cli)
+ [Importing bulk records \(AWS SDKs\)](#python-import-ex)

## Importing bulk records \(console\)<a name="bulk-data-import-console"></a>

 To import bulk records into a dataset in Amazon Personalize using the console, create a dataset import job with a name, the IAM service role, and the location of your data\. 

**To import bulk records \(console\)**
**Note**  
If you just created your dataset in [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md), skip to step 5\.

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1.  On the **Dataset groups** page, choose your dataset group\. The dataset group **Dashboard** is displayed\. 

1. In the **Upload datasets** section, for the type of dataset you want to import, choose **Import**\.\. The **Configure < dataset type >** page is displayed\.

1. If you already created a dataset of this type, all **Dataset details** and **Schema details** fields are disabled\. Choose **Next**\. 

   If haven't created a dataset of this type, complete the **Dataset details** and **Schema details** fields to create a dataset\. 

1. In **Dataset import job details**, for **Dataset import job name**, specify a name for your import job\.

1. For **IAM service role**, keep the default selection of **Enter a custom IAM role ARN**\.

1. For **Custom IAM role ARN**, specify the role that you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\.

1. For **Data location**, specify where your data file is stored in Amazon S3\. Use the following syntax:

   **s3://<name of your S3 bucket>/<folder path>/<CSV filename>**
**Note**  
If your CSV files are in a folder in your S3 bucket and you want to upload multiple CSV files to a dataset with one dataset import job, use this syntax without the CSV file name\. 

1. Choose **Finish**\. The data import job starts and the **Dashboard Overview** page is displayed\.

   The dataset import is complete when the status shows as ACTIVE\. You can now train the model using the specified dataset\.

   After you import your data into the relevant datasets in the dataset group, create a solution version by training a model\. For more information, see [Creating a solution](training-deploying-solutions.md)\. 

## Importing bulk records \(AWS CLI\)<a name="bulk-data-import-cli"></a>

 To import bulk records using the AWS CLI, create a dataset import job using the [CreateDatasetImportJob](API_CreateDatasetImportJob.md) command\. 

**Import bulk records \(AWS CLI\)**

1. Create a dataset import job by running the following command\. Provide the dataset Amazon Resource Name \(ARN\) from [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md) and your S3 bucket name\. Supply the AWS Identity and Access Management \(IAM\) role Amazon Resource Name \(ARN\) that you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\. For more information about the operation, see [CreateDatasetImportJob](API_CreateDatasetImportJob.md)\.

   ```
   aws personalize create-dataset-import-job \
     --job-name dataset import job name \
     --dataset-arn dataset arn \
     --data-source dataLocation=s3://bucketname/filename \
     --role-arn roleArn
   ```

   The dataset import job ARN is displayed, as shown in the following example\.

   ```
   {
     "datasetImportJobArn": "arn:aws:personalize:us-west-2:acct-id:dataset-import-job/DatasetImportJobName"
   }
   ```

1. Check the status by using the `describe-dataset-import-job` command\. Provide the dataset import job ARN that was returned in the previous step\. For more information about the operation, see [DescribeDatasetImportJob](API_DescribeDatasetImportJob.md)\.

   ```
   aws personalize describe-dataset-import-job \
     --dataset-import-job-arn dataset import job arn
   ```

   The properties of the dataset import job, including its status, are displayed\. Initially, the `status` shows as CREATE PENDING\.

   ```
   {
     "datasetImportJob": {
         "jobName": "Dataset Import job name",
         "datasetImportJobArn": "arn:aws:personalize:us-west-2:acct-id:dataset-import-job/DatasetImportJobArn",
         "datasetArn": "arn:aws:personalize:us-west-2:acct-id:dataset/DatasetGroupName/INTERACTIONS",
         "dataSource": {
             "dataLocation": "s3://<bucketname>/ratings.csv"
         },
         "roleArn": "role-arn",
         "status": "CREATE PENDING",
         "creationDateTime": 1542392161.837,
         "lastUpdatedDateTime": 1542393013.377
     }
   }
   ```

   The dataset import is complete when the status shows as ACTIVE\. You can now train the model using the specified dataset\.

   After you import your data into the relevant datasets in the dataset group, create a solution version by training a model\. For more information, see [Creating a solution](training-deploying-solutions.md)\.

## Importing bulk records \(AWS SDKs\)<a name="python-import-ex"></a>

To add data to your dataset, create and run a dataset import job using the [CreateDatasetImportJob](API_CreateDatasetImportJob.md) operation\. The following code shows how to create a dataset import job using the SDK for Python \(Boto3\) or SDK for Java 2\.x\.

------
#### [ SDK for Python \(Boto3\) ]

Specify the `datasetGroupArn` and set the `dataLocation` to the path to your Amazon S3 bucket where you stored the training data\. 

For the `roleArn`, specify the AWS Identity and Access Management \(IAM\) role that gives Amazon Personalize permissions to access your S3 bucket\. See [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\.

```
import boto3

personalize = boto3.client('personalize')

response = personalize.create_dataset_import_job(
    jobName = 'YourImportJob',
    datasetArn = 'dataset_arn',
    dataSource = {'dataLocation':'s3://bucket/file.csv'},
    roleArn = 'role_arn'
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

Use the following `createPersonalizeDatasetImportJob` method to create a dataset import job\. Pass the following as parameters: an Amazon Personalize service client, the dataset group's ARN \(Amazon Resource Name\), a name for the job, the dataset ARN, the `bucket-name/file.csv` where you stored the training data, and your service\-linked role's ARN \(see [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\)\. 

If your CSV files are in a folder in an Amazon S3 bucket, you can upload multiple CSV files to a dataset in one dataset import job\. For the bucket path, specify the `bucket-name/folder-name/` instead of the file name\.

```
public static String createPersonalizeDatasetImportJob(PersonalizeClient personalizeClient,
                                                      String jobName,
                                                      String datasetArn,
                                                      String s3BucketPath,
                                                      String roleArn) {

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

The response from the [DescribeDatasetImportJob](API_DescribeDatasetImportJob.md) operation includes the status of the operation\.

You must wait until the status changes to ACTIVE before you can use the data to train a model\.

Amazon Personalize provides operations for managing datasets, dataset groups, and dataset import jobs\. For example, you can use [ListDatasets](API_ListDatasets.md) to list the datasets in a dataset group and [DeleteDataset](API_DeleteDataset.md) to delete a dataset\.

After you import your data into the relevant datasets in the dataset group, create a solution version by training a model\. For more information, see [Creating a solution](training-deploying-solutions.md)\.