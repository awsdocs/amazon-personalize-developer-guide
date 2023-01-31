# Importing bulk records with a dataset import job<a name="bulk-data-import-step"></a>

After you have formatted your input data \(see [Formatting your input data](data-prep-formatting.md)\) and uploaded it to an Amazon Simple Storage Service \(Amazon S3\) bucket \(see [Uploading to an Amazon S3 bucket](data-prep-upload-s3.md)\), import the bulk records by creating a dataset import job\. 

A *dataset import job* is a bulk import tool that populates your dataset with data from your Amazon S3 bucket\. You can create a dataset import job using the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\.

If you've previously created a dataset import job for a dataset, you can use a new dataset import job to add to or replace the existing bulk data\. For more information see [Updating existing bulk data](updating-existing-bulk-data.md)\. 

**Topics**
+ [Importing bulk records \(console\)](#bulk-data-import-console)
+ [Importing bulk records \(AWS CLI\)](#bulk-data-import-cli)
+ [Importing bulk records \(AWS SDKs\)](#python-import-ex)

## Importing bulk records \(console\)<a name="bulk-data-import-console"></a>

**Important**  
By default, a dataset import job replaces any existing data in the dataset that you imported in bulk\. For information about updating existing data, see [Updating existing bulk data](updating-existing-bulk-data.md)\.

 To import bulk records into a dataset with the Amazon Personalize console, create a dataset import job with a name, the IAM service role, and the location of your data\.

If you just created your dataset in [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md), skip to step 5\. If you've already completed an initial import job and want to refresh your data, see [Updating bulk records \(console\)](updating-existing-bulk-data.md#updating-bulk-data-console)\.

**To import bulk records \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1.  On the **Dataset groups** page, choose your dataset group\. The dataset group **Overview** displays\.

1. In the **Create datasets** section, choose the **Import** button for the type of data you want to import\. The **Configure < dataset type > schema** page displays\.

1. In **Dataset import job details**, for **Data import source** choose **Import data from S3**\. 

1. For **Dataset import job name**, specify a name for your import job\.

1. In **Input source**, for **S3 Location**, specify where your data file is stored in Amazon S3\. Use the following syntax:

   **s3://<name of your S3 bucket>/<folder path>/<CSV filename>**
**Note**  
If your CSV files are in a folder in your S3 bucket and you want to upload multiple CSV files to a dataset with one dataset import job, use this syntax without the CSV file name\. 

1. In **IAM role**, choose to either create a new role or use an existing one\. If you completed the perquisites, choose **Use an existing service role** and specify the role that you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\. 

1. If you created a metric attribution and want to publish metrics related to this job to Amazon S3, in **Publish event metrics to S3** choose **Publish metrics for this import job**\. 

   If you haven't created one and want to publish metrics for this job, choose **Create metric attribution** to create a new one on a different tab\. After you create the metric attribution, you can return to this screen and finish creating the import job\. 

   For more information on metric attributions, see [Measuring impact of recommendations](measuring-recommendation-impact.md)\.

1. For **Tags**, optionally add any tags\. For more information about tagging Amazon Personalize resources, see [Tagging Amazon Personalize resources](tagging-resources.md)\.

1. Choose **Finish**\. The data import job starts and the **Dashboard Overview** page is displayed\. The dataset import is complete when the status shows as ACTIVE\. After you import data into an Amazon Personalize dataset, you can analyze it, export it to an Amazon S3 bucket, update it, or delete it by deleting the dataset\. For more information, see [Managing data](managing-data.md)\. 

   After you import your data you are ready to create a solution\. For more information, see [Creating a solution](training-deploying-solutions.md)\. 

## Importing bulk records \(AWS CLI\)<a name="bulk-data-import-cli"></a>

**Important**  
By default, a dataset import job replaces any existing data in the dataset that you imported in bulk\. For information about updating existing data, see [Updating existing bulk data](updating-existing-bulk-data.md)\.

 To import bulk records using the AWS CLI, create a dataset import job using the [CreateDatasetImportJob](API_CreateDatasetImportJob.md) command\. If you've previously created a dataset import job for a dataset, you can use the import mode parameter to specify how to add the new data\. For a code sample, see [Updating bulk records \(AWS CLI\)](updating-existing-bulk-data.md#updating-bulk-data-cli)\.

**Import bulk records \(AWS CLI\)**

1. Create a dataset import job by running the following command\. Provide the Amazon Resource Name \(ARN\) for your dataset and specify the path to your Amazon S3 bucket where you stored the training data\. Use the following syntax for the path:

   **s3://<name of your S3 bucket>/<folder path>/<CSV filename>**

   Provide the AWS Identity and Access Management \(IAM\) role Amazon Resource Name \(ARN\) that you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\. The default `import-mode` is `FULL`\. For more information see [Updating existing bulk data](updating-existing-bulk-data.md)\. For more information about the operation, see [CreateDatasetImportJob](API_CreateDatasetImportJob.md)\.

   ```
   aws personalize create-dataset-import-job \
   --job-name dataset import job name \
   --dataset-arn dataset arn \
   --data-source dataLocation=s3://bucketname/filename \
   --role-arn roleArn \
   --import-mode FULL
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
         "importMode": "FULL",
         "roleArn": "role-arn",
         "status": "CREATE PENDING",
         "creationDateTime": 1542392161.837,
         "lastUpdatedDateTime": 1542393013.377
     }
   }
   ```

   The dataset import is complete when the status shows as ACTIVE\. After you import data into an Amazon Personalize dataset, you can analyze it, export it to an Amazon S3 bucket, update it, or delete it by deleting the dataset\. For more information, see [Managing data](managing-data.md)\. 

   After you import your data into the relevant datasets in the dataset group, you can create a solution version \(trained model\)\. For more information, see [Creating a solution](training-deploying-solutions.md)\.

## Importing bulk records \(AWS SDKs\)<a name="python-import-ex"></a>

**Important**  
By default, a dataset import job replaces any existing data in the dataset that you imported in bulk\. For information about updating existing data, see [Updating existing bulk data](updating-existing-bulk-data.md)\.

To import data, create a dataset import job with the [CreateDatasetImportJob](API_CreateDatasetImportJob.md) operation\. The following code shows how to create a dataset import job\.

------
#### [ SDK for Python \(Boto3\) ]

Give the job name, set the `datasetArn` the Amazon Resource Name \(ARN\) of your dataset, and set the `dataLocation` to the path to your Amazon S3 bucket where you stored the training data\. Use the following syntax for the path:

**s3://<name of your S3 bucket>/<folder path>/<CSV filename>**

For the `roleArn`, specify the AWS Identity and Access Management \(IAM\) role that gives Amazon Personalize permissions to access your S3 bucket\. See [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\. The default `importMode` is `FULL`\. For more information see [Updating bulk records \(AWS SDKs\)](updating-existing-bulk-data.md#updating-bulk-data-sdk)\. 

```
import boto3

personalize = boto3.client('personalize')

response = personalize.create_dataset_import_job(
    jobName = 'YourImportJob',
    datasetArn = 'dataset_arn',
    dataSource = {'dataLocation':'s3://bucket/file.csv'},
    roleArn = 'role_arn',
    importMode = 'FULL'
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

Use the following `createPersonalizeDatasetImportJob` method to create a dataset import job\. Pass the following as parameters: an Amazon Personalize service client, a name for the job, the dataset's ARN, the Amazon S3 bucket path \(`s3://<bucketname>/<file name.csv>`\) where you stored your training data, and your Amazon Personalize IAM service role's ARN \(see [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\)\. 

If your CSV files are in a folder in an Amazon S3 bucket, you can upload multiple CSV files to a dataset in one dataset import job\. For the bucket path, specify the `bucket-name/folder-name/` instead of the file name\.

 The `importMode` can be either `ImportMode.BULK` or, if you've imported bulk data previously, `ImportMode.INCREMENTAL`\. For more information see [Updating bulk records \(AWS SDKs\)](updating-existing-bulk-data.md#updating-bulk-data-sdk)\. 

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
#### [ SDK for JavaScript v3 ]

```
// Get service clients module and commands using ES6 syntax.

import { CreateDatasetGroupCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the dataset group parameters.
export const createDatasetGroupParam = { 
  name: 'NAME' /* required */
}

export const run = async (createDatasetGroupParam) => {
  try {
    const response = await personalizeClient.send(new CreateDatasetGroupCommand(createDatasetGroupParam));
    console.log("Success", response);
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run(createDatasetGroupParam);
```

------

The response from the [DescribeDatasetImportJob](API_DescribeDatasetImportJob.md) operation includes the status of the operation\.

You must wait until the status changes to ACTIVE before you can use the data to train a model\.

The dataset import is complete when the status shows as ACTIVE\. After you import data into an Amazon Personalize dataset, you can analyze it, export it to an Amazon S3 bucket, update it, or delete it by deleting the dataset\. For more information, see [Managing data](managing-data.md)\. 

After you import your data into the relevant datasets in the dataset group, you can create a solution version \(trained model\)\. For more information, see [Creating a solution](training-deploying-solutions.md)\.