# CreateDatasetExportJob<a name="API_CreateDatasetExportJob"></a>

 Creates a job that exports data from your dataset to an Amazon S3 bucket\. To allow Amazon Personalize to export the training data, you must specify an service\-linked IAM role that gives Amazon Personalize `PutObject` permissions for your Amazon S3 bucket\. For information, see [Exporting a dataset](https://docs.aws.amazon.com/personalize/latest/dg/export-data.html) in the Amazon Personalize developer guide\. 

 **Status** 

A dataset export job can be in one of the following states:
+ CREATE PENDING > CREATE IN\_PROGRESS > ACTIVE \-or\- CREATE FAILED

 To get the status of the export job, call [DescribeDatasetExportJob](https://docs.aws.amazon.com/personalize/latest/dg/API_DescribeDatasetExportJob.html), and specify the Amazon Resource Name \(ARN\) of the dataset export job\. The dataset export is complete when the status shows as ACTIVE\. If the status shows as CREATE FAILED, the response includes a `failureReason` key, which describes why the job failed\. 

## Request Syntax<a name="API_CreateDatasetExportJob_RequestSyntax"></a>

```
{
   "datasetArn": "string",
   "ingestionMode": "string",
   "jobName": "string",
   "jobOutput": { 
      "s3DataDestination": { 
         "kmsKeyArn": "string",
         "path": "string"
      }
   },
   "roleArn": "string"
}
```

## Request Parameters<a name="API_CreateDatasetExportJob_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [datasetArn](#API_CreateDatasetExportJob_RequestSyntax) **   <a name="personalize-CreateDatasetExportJob-request-datasetArn"></a>
The Amazon Resource Name \(ARN\) of the dataset that contains the data to export\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

 ** [ingestionMode](#API_CreateDatasetExportJob_RequestSyntax) **   <a name="personalize-CreateDatasetExportJob-request-ingestionMode"></a>
The data to export, based on how you imported the data\. You can choose to export only `BULK` data that you imported using a dataset import job, only `PUT` data that you imported incrementally \(using the console, PutEvents, PutUsers and PutItems operations\), or `ALL` for both types\. The default value is `PUT`\.   
Type: String  
Valid Values:` BULK | PUT | ALL`   
Required: No

 ** [jobName](#API_CreateDatasetExportJob_RequestSyntax) **   <a name="personalize-CreateDatasetExportJob-request-jobName"></a>
The name for the dataset export job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: Yes

 ** [jobOutput](#API_CreateDatasetExportJob_RequestSyntax) **   <a name="personalize-CreateDatasetExportJob-request-jobOutput"></a>
The path to the Amazon S3 bucket where the job's output is stored\.  
Type: [DatasetExportJobOutput](API_DatasetExportJobOutput.md) object  
Required: Yes

 ** [roleArn](#API_CreateDatasetExportJob_RequestSyntax) **   <a name="personalize-CreateDatasetExportJob-request-roleArn"></a>
The Amazon Resource Name \(ARN\) of the IAM service role that has permissions to add data to your output Amazon S3 bucket\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+`   
Required: Yes

## Response Syntax<a name="API_CreateDatasetExportJob_ResponseSyntax"></a>

```
{
   "datasetExportJobArn": "string"
}
```

## Response Elements<a name="API_CreateDatasetExportJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [datasetExportJobArn](#API_CreateDatasetExportJob_ResponseSyntax) **   <a name="personalize-CreateDatasetExportJob-response-datasetExportJobArn"></a>
The Amazon Resource Name \(ARN\) of the dataset export job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+` 

## Errors<a name="API_CreateDatasetExportJob_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** LimitExceededException **   
The limit on the number of requests per second has been exceeded\.  
HTTP Status Code: 400

 ** ResourceAlreadyExistsException **   
The specified resource already exists\.  
HTTP Status Code: 400

 ** ResourceInUseException **   
The specified resource is in use\.  
HTTP Status Code: 400

 ** ResourceNotFoundException **   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_CreateDatasetExportJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/CreateDatasetExportJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/CreateDatasetExportJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/CreateDatasetExportJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/CreateDatasetExportJob) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/CreateDatasetExportJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/CreateDatasetExportJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/CreateDatasetExportJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/CreateDatasetExportJob) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/CreateDatasetExportJob) 