# DatasetExportJob<a name="API_DatasetExportJob"></a>

Describes a job that exports a dataset to an Amazon S3 bucket\. For more information, see [CreateDatasetExportJob](API_CreateDatasetExportJob.md)\.

A dataset export job can be in one of the following states:
+ CREATE PENDING > CREATE IN\_PROGRESS > ACTIVE \-or\- CREATE FAILED

## Contents<a name="API_DatasetExportJob_Contents"></a>

 **creationDateTime**   <a name="personalize-Type-DatasetExportJob-creationDateTime"></a>
The creation date and time \(in Unix time\) of the dataset export job\.  
Type: Timestamp  
Required: No

 **datasetArn**   <a name="personalize-Type-DatasetExportJob-datasetArn"></a>
The Amazon Resource Name \(ARN\) of the dataset to export\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 **datasetExportJobArn**   <a name="personalize-Type-DatasetExportJob-datasetExportJobArn"></a>
The Amazon Resource Name \(ARN\) of the dataset export job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 **failureReason**   <a name="personalize-Type-DatasetExportJob-failureReason"></a>
If a dataset export job fails, provides the reason why\.  
Type: String  
Required: No

 **ingestionMode**   <a name="personalize-Type-DatasetExportJob-ingestionMode"></a>
The data to export, based on how you imported the data\. You can choose to export `BULK` data that you imported using a dataset import job, `PUT` data that you imported incrementally \(using the console, PutEvents, PutUsers and PutItems operations\), or `ALL` for both types\. The default value is `PUT`\.   
Type: String  
Valid Values:` BULK | PUT | ALL`   
Required: No

 **jobName**   <a name="personalize-Type-DatasetExportJob-jobName"></a>
The name of the export job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: No

 **jobOutput**   <a name="personalize-Type-DatasetExportJob-jobOutput"></a>
The path to the Amazon S3 bucket where the job's output is stored\. For example:  
 `s3://bucket-name/folder-name/`   
Type: [DatasetExportJobOutput](API_DatasetExportJobOutput.md) object  
Required: No

 **lastUpdatedDateTime**   <a name="personalize-Type-DatasetExportJob-lastUpdatedDateTime"></a>
The date and time \(in Unix time\) the status of the dataset export job was last updated\.  
Type: Timestamp  
Required: No

 **roleArn**   <a name="personalize-Type-DatasetExportJob-roleArn"></a>
The Amazon Resource Name \(ARN\) of the IAM service role that has permissions to add data to your output Amazon S3 bucket\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 **status**   <a name="personalize-Type-DatasetExportJob-status"></a>
The status of the dataset export job\.  
A dataset export job can be in one of the following states:  
+ CREATE PENDING > CREATE IN\_PROGRESS > ACTIVE \-or\- CREATE FAILED
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

## See Also<a name="API_DatasetExportJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DatasetExportJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DatasetExportJob) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/DatasetExportJob) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/DatasetExportJob) 