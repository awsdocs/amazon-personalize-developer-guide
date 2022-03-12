# Exporting a dataset<a name="export-data"></a>

After you've imported your data, you can export the data in an Interactions, Items, or Users dataset to an Amazon S3 bucket\. After exporting a dataset, you can verify and inspect the data that Amazon Personalize uses to generate recommendations, view the user interaction events that you previously recorded in real time, and perform offline analysis on your data\. 

You can choose to export only the data that you imported in bulk \(imported using an Amazon Personalize dataset import job\), only the data that you imported incrementally \(historical and real\-time records imported using the console or the `PutEvents`, `PutUsers`, or `PutItems` operations\), or both\. 

Amazon Personalize combines duplicate records that match exactly *for all fields* into one record\. If two records have the same ID, Amazon Personalize includes or removes duplicates depending on data you choose to export: 
+ If you export both bulk and incremental data, Amazon Personalize removes duplicate items or users based on their IDs, keeping only the newest items or users\. For interactions datasets, Amazon Personalize includes all interactions data, regardless of timestamp\.
+ If you export incremental data only, Amazon Personalize includes all item, user, or interaction data that you imported incrementally, including items or users with the same IDs\. Only records that match exactly for all fields are excluded\.
+ If you export bulk data only, Amazon Personalize includes all item, user, or interaction data that you imported in bulk, including items or users with the same IDs\. Only records that match exactly for all fields are excluded\.

To export a dataset, you create a dataset export job\. A *dataset export job* is a record export tool that outputs the records in a dataset to one or more CSV files in an Amazon S3 bucket\. The output CSV file includes a header row with column names that match the fields in the dataset's schema\. 

You create a dataset export job using the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\. 

**Topics**
+ [Dataset export job permissions requirements](#export-permissions)
+ [Creating a dataset export job \(console\)](#export-data-console)
+ [Creating a dataset export job \(AWS CLI\)](#export-data-cli)
+ [Creating a dataset export job \(AWS SDKs\)](#export-data-sdk)

## Dataset export job permissions requirements<a name="export-permissions"></a>

To export a dataset, Amazon Personalize needs permission to add files to your Amazon S3 bucket\. To grant permissions, attach a new AWS Identity and Access Management \(IAM\) policy to your Amazon Personalize service\-linked role that grants the role permission to use the `PutObject` and `ListBucket` Actions on your bucket, and attach a bucket policy to your output Amazon S3 bucket that grants the Amazon Personalize principle permission to use the `PutObject` and `ListBucket` Actions\.

 Amazon S3 buckets and objects must be either encryption free or, if you are using AWS Key Management Service \(AWS KMS\) for encryption, you must give your IAM user and Amazon Personalize service role permission to use your key\. You must also add Amazon Personalize as a Principle in your AWS KMS key policy\. For more information, see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.

If your Domain dataset group or Custom dataset group uses an AWS KMS key for encryption, your IAM user and your AWS KMS key policies must grant `GenerateDataKeyWithoutPlaintext` and `Decrypt` permissions\.

### Service\-linked role policy for exporting a dataset<a name="role-policy-for-dataset-export-job"></a>

The following example policy grants your Amazon Personalize service\-linked role permission to use the `PutObject` and `ListBucket` Actions\. Replace `bucket-name` with the name of your output bucket\. For information about attaching policies to a service\-linked IAM role, see [Attaching an Amazon S3 policy to your Amazon Personalize service role](granting-personalize-s3-access.md#attaching-s3-policy-to-role)\. 

```
{
    "Version": "2012-10-17",
    "Id": "PersonalizeS3BucketAccessPolicy",
    "Statement": [
        {
            "Sid": "PersonalizeS3BucketAccessPolicy",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucket-name",
                "arn:aws:s3:::bucket-name/*"
            ]
        }
    ]
}
```

### Amazon S3 bucket policy for exporting a dataset<a name="bucket-policy-for-dataset-export-job"></a>

The following example policy grants Amazon Personalize permission to use the `PutObject` and `ListBucket` Actions on an Amazon S3 bucket\. Replace `bucket-name` with the name of your bucket\. For information on adding an Amazon S3 bucket policy to a bucket, see [How Do I Add an S3 Bucket Policy?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-bucket-policy.html) in the *Amazon Simple Storage Service User Guide\.* 

```
{
    "Version": "2012-10-17",
    "Id": "PersonalizeS3BucketAccessPolicy",
    "Statement": [
        {
            "Sid": "PersonalizeS3BucketAccessPolicy",
            "Effect": "Allow",
            "Principal": {
                "Service": "personalize.amazonaws.com"
            },
            "Action": [
                "s3:PutObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucket-name",
                "arn:aws:s3:::bucket-name/*"
            ]
        }
    ]
}
```

## Creating a dataset export job \(console\)<a name="export-data-console"></a>

After you import your data into a dataset and create an output Amazon S3 bucket, you can export the data to the bucket for analysis\. **To export a dataset using the Amazon Personalize console, you create a dataset export job\. For information about creating an Amazon S3 bucket, see [Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html) in the *Amazon Simple Storage Service User Guide*\.

Before you export a dataset, make sure that your Amazon Personalize service\-linked role can access and write to your output Amazon S3 bucket\. See [Dataset export job permissions requirements](#export-permissions)\. 

**To create a dataset export job \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home)\.

1. In the navigation pane, choose **Dataset groups**\.

1. On the **Dataset groups** page, choose your dataset group\.

1. In the navigation pane, choose **Datasets**\.

1. Choose the dataset that you want to export to an Amazon S3 bucket\.

1.  In **Dataset export jobs**, choose **Create dataset export job**\. 

1. In **Dataset export job details**, for **Dataset export job name**, enter a name for the export job\.

1. For **IAM service role**, choose the Amazon Personalize service\-linked role that you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\.

1. For **Amazon S3 data output path**, enter the destination Amazon S3 bucket\. Use the following syntax:

   **s3://<name of your S3 bucket>/<folder path>**

1. If you are using AWS KMS for encryption, for **KMS key ARN**, enter the Amazon Resource Name \(ARN\) for the AWS KMS key\. 

1. For **Export data type**, choose the type data to export based on how you originally imported the data\.
   +  Choose **Bulk** to export only data that you imported in bulk using a dataset import job\. 
   + Choose **Incremental** to export only data that you imported incrementally using the console or the `PutEvents`, `PutUsers`, or `PutItems` operations\. 
   + Choose **Both** to export all of the data in the dataset\. 

1. Choose **Create dataset export job**\. 

   On the **Dataset overview** page, in **Dataset export jobs**, the job is listed with an **Export job status**\. The dataset export job is complete when the status is **ACTIVE**\. You can then download the data from the output Amazon S3 bucket\. For information on downloading objects from an Amazon S3 bucket, see [Downloading an object](https://docs.aws.amazon.com/AmazonS3/latest/userguide/download-objects.html) in the *Amazon Simple Storage Service User Guide\.*\.

## Creating a dataset export job \(AWS CLI\)<a name="export-data-cli"></a>

After you import your data into the dataset and create an output Amazon S3 bucket, you can export the dataset to the bucket for analysis\. To export a dataset using the AWS CLI, create a dataset export job using the `create-dataset-export-job` AWS CLI command\. For information about creating an Amazon S3 bucket, see [Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html) in the *Amazon Simple Storage Service User Guide*\. 

Before you export a dataset, make sure that the Amazon Personalize service\-linked role can access and write to your output Amazon S3 bucket\. See [Dataset export job permissions requirements](#export-permissions)\. 

 The following is an example of the `create-dataset-export-job` AWS CLI command\. Give the job a name, replace `dataset arn` with the Amazon Resource Name \(ARN\) of the dataset that you want to export, and replace `role ARN` with the ARN of the Amazon Personalize service\-linked role that you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\. In `s3DataDestination`, for the `kmsKeyArn`, optionally provide the ARN for your AWS KMS key, and for the `path` provide the path to your output Amazon S3 bucket\. 

 For `ingestion-mode`, specify the data to export from the following options: 
+  Specify `BULK` to export only data that you imported in bulk using a dataset import job\. 
+  Specify `PUT` to export only data that you imported incrementally using the console or the `PutEvents`, PutUsers, or `PutItems` operations\. 
+  Specify `ALL` to export all of the data in the dataset\. 

 For more information see [CreateDatasetExportJob](API_CreateDatasetExportJob.md)\. 

```
aws personalize create-dataset-export-job \
  --job-name job name \
  --dataset-arn dataset ARN \
  --job-output "{\"s3DataDestination\":{\"kmsKeyArn\":\"kms key ARN\",\"path\":\"s3://bucket-name/folder-name/\"}}" \
  --role-arn role ARN \
  --ingestion-mode PUT
```

The dataset export job ARN is displayed\.

```
{
  "datasetExportJobArn": "arn:aws:personalize:us-west-2:acct-id:dataset-export-job/DatasetExportJobName"
}
```

Use the `DescribeDatasetExportJob` operation to check the status\.

```
aws personalize describe-dataset-export-job \
  --dataset-export-job-arn dataset export job ARN
```

## Creating a dataset export job \(AWS SDKs\)<a name="export-data-sdk"></a>

 After you import your data into the dataset and create an output Amazon S3 bucket, you can export the dataset to the bucket for analysis\. To export a dataset using the AWS SDKs, create a dataset export job using the [CreateDatasetExportJob](API_CreateDatasetExportJob.md) operation\. For information about creating an Amazon S3 bucket, see [Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html) in the *Amazon Simple Storage Service User Guide*\. 

The following code shows how to create a dataset export job using the SDK for Python \(Boto3\) or the SDK for Java 2\.x SDK\.

Before you export a dataset, make sure that the Amazon Personalize service\-linked role can access and write to your output Amazon S3 bucket\. See [Dataset export job permissions requirements](#export-permissions)\. 

------
#### [ SDK for Python \(Boto3\) ]

Use the following `create_dataset_export_job` to export the data in a dataset to an Amazon S3 bucket\. Give the job a name, replace `dataset arn` with the Amazon Resource Name \(ARN\) of the dataset that you want to export, and replace `role ARN` with the ARN of the Amazon Personalize service\-linked role that you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\. In `s3DataDestination`, for the `kmsKeyArn`, optionally provide the ARN for your AWS KMS key, and for the `path` provide the path to your output Amazon S3 bucket\. 

 For `ingestionMode`, specify the data to export from the following options: 
+ Specify `BULK` to export only data that you imported in bulk using a dataset import job\. 
+ Specify `PUT` to export only data that you imported incrementally using the console or the `PutEvents`, PutUsers, or `PutItems` operations\. 
+ Specify `ALL` to export all of the data in the dataset\.

```
import boto3

personalize = boto3.client('personalize')

response = personalize.create_dataset_export_job(
    jobName = 'job name',
    datasetArn = 'dataset ARN',
    jobOutput = {
      "s3DataDestination": {
        "kmsKeyArn": "kms key ARN",
        "path": "s3://bucket-name/folder-name/"
      }
    },
    roleArn = 'role ARN',
    ingestionMode = 'PUT'
)

dsej_arn = response['datasetExportJobArn']

print ('Dataset Export Job arn: ' + dsej_arn)

description = personalize.describe_dataset_export_job(
    datasetExportJobArn = dsej_arn)['datasetExportJob']

print('Name: ' + description['jobName'])
print('ARN: ' + description['datasetExportJobArn'])
print('Status: ' + description['status'])
```

------
#### [ SDK for Java 2\.x ]

Use the following `createDatasetExportJob` method to create a dataset export job\. Pass the following as parameters: a PersonalizeClient, the name for your export job, the ARN of the dataset you want to export, the ingestion mode, the path for the output Amazon S3 bucket, and the ARN for your AWS KMS key\.

 The `ingestionMode` can be one of the following options: 
+ Use `IngestionMode.BULK` to export only data that you imported in bulk using a dataset import job\. 
+ Use `IngestionMode.PUT` to export only data that you imported incrementally using the console or the `PutEvents`, PutUsers, or `PutItems` operations\. 
+ Use `IngestionMode.ALL` to export all of the data in the dataset\.

```
public static void createDatasetExportJob(PersonalizeClient personalizeClient, 
                                        String jobName,
                                        String datasetArn, 
                                        IngestionMode ingestionMode, 
                                        String roleArn,
                                        String s3BucketPath,
                                        String kmsKeyArn) {

    long waitInMilliseconds = 30 * 1000; // 30 seconds
    String status = null;

    try {
        S3DataConfig exportS3DataConfig = S3DataConfig.builder()
            .path(s3BucketPath)
            .kmsKeyArn(kmsKeyArn)
            .build();
            
        DatasetExportJobOutput jobOutput = DatasetExportJobOutput.builder()
            .s3DataDestination(exportS3DataConfig)
            .build();

        CreateDatasetExportJobRequest createRequest = CreateDatasetExportJobRequest.builder()
            .jobName(jobName)
            .datasetArn(datasetArn)
            .ingestionMode(ingestionMode)
            .jobOutput(jobOutput)
            .roleArn(roleArn)
            .build();

        String datasetExportJobArn = personalizeClient.createDatasetExportJob(createRequest).datasetExportJobArn();

        DescribeDatasetExportJobRequest describeDatasetExportJobRequest = DescribeDatasetExportJobRequest.builder()
            .datasetExportJobArn(datasetExportJobArn)
            .build();

        long maxTime = Instant.now().getEpochSecond() + 3 * 60 * 60;

        while (Instant.now().getEpochSecond() < maxTime) {

            DatasetExportJob datasetExportJob = personalizeClient.describeDatasetExportJob(describeDatasetExportJobRequest)
                .datasetExportJob();

            status = datasetExportJob.status();
            System.out.println("Export job status: " + status);

            if (status.equals("ACTIVE") || status.equals("CREATE FAILED")) {
                break;
            }
            try {
                Thread.sleep(waitInMilliseconds);
            } catch (InterruptedException e) {
                System.out.println(e.getMessage());
            }
        }
    } catch (PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
}
```

------