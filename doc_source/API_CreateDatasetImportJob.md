# CreateDatasetImportJob<a name="API_CreateDatasetImportJob"></a>

Creates a job that imports training data from your data source \(an Amazon S3 bucket\) to an Amazon Personalize dataset\. To allow Amazon Personalize to import the training data, you must specify an IAM service role that has permission to read from the data source, as Amazon Personalize makes a copy of your data and processes it internally\. For information on granting access to your Amazon S3 bucket, see [Giving Amazon Personalize Access to Amazon S3 Resources](https://docs.aws.amazon.com/personalize/latest/dg/granting-personalize-s3-access.html)\. 

**Important**  
By default, a dataset import job replaces any existing data in the dataset that you imported in bulk\. To add new records without replacing existing data, specify INCREMENTAL for the import mode in the CreateDatasetImportJob operation\.

 **Status** 

A dataset import job can be in one of the following states:
+ CREATE PENDING > CREATE IN\_PROGRESS > ACTIVE \-or\- CREATE FAILED

To get the status of the import job, call [DescribeDatasetImportJob](https://docs.aws.amazon.com/personalize/latest/dg/API_DescribeDatasetImportJob.html), providing the Amazon Resource Name \(ARN\) of the dataset import job\. The dataset import is complete when the status shows as ACTIVE\. If the status shows as CREATE FAILED, the response includes a `failureReason` key, which describes why the job failed\.

**Note**  
Importing takes time\. You must wait until the status shows as ACTIVE before training a model using the dataset\.

**Related APIs**
+  [ListDatasetImportJobs](https://docs.aws.amazon.com/personalize/latest/dg/API_ListDatasetImportJobs.html) 
+  [DescribeDatasetImportJob](https://docs.aws.amazon.com/personalize/latest/dg/API_DescribeDatasetImportJob.html) 

## Request Syntax<a name="API_CreateDatasetImportJob_RequestSyntax"></a>

```
{
   "datasetArn": "string",
   "dataSource": { 
      "dataLocation": "string"
   },
   "importMode": "string",
   "jobName": "string",
   "publishAttributionMetricsToS3": boolean,
   "roleArn": "string",
   "tags": [ 
      { 
         "tagKey": "string",
         "tagValue": "string"
      }
   ]
}
```

## Request Parameters<a name="API_CreateDatasetImportJob_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [datasetArn](#API_CreateDatasetImportJob_RequestSyntax) **   <a name="personalize-CreateDatasetImportJob-request-datasetArn"></a>
The ARN of the dataset that receives the imported data\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

 ** [dataSource](#API_CreateDatasetImportJob_RequestSyntax) **   <a name="personalize-CreateDatasetImportJob-request-dataSource"></a>
The Amazon S3 bucket that contains the training data to import\.  
Type: [DataSource](API_DataSource.md) object  
Required: Yes

 ** [importMode](#API_CreateDatasetImportJob_RequestSyntax) **   <a name="personalize-CreateDatasetImportJob-request-importMode"></a>
Specify how to add the new records to an existing dataset\. The default import mode is `FULL`\. If you haven't imported bulk records into the dataset previously, you can only specify `FULL`\.  
+ Specify `FULL` to overwrite all existing bulk data in your dataset\. Data you imported individually is not replaced\.
+ Specify `INCREMENTAL` to append the new records to the existing data in your dataset\. Amazon Personalize replaces any record with the same ID with the new one\.
Type: String  
Valid Values:` FULL | INCREMENTAL`   
Required: No

 ** [jobName](#API_CreateDatasetImportJob_RequestSyntax) **   <a name="personalize-CreateDatasetImportJob-request-jobName"></a>
The name for the dataset import job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: Yes

 ** [publishAttributionMetricsToS3](#API_CreateDatasetImportJob_RequestSyntax) **   <a name="personalize-CreateDatasetImportJob-request-publishAttributionMetricsToS3"></a>
If you created a metric attribution, specify whether to publish metrics for this import job to Amazon S3  
Type: Boolean  
Required: No

 ** [roleArn](#API_CreateDatasetImportJob_RequestSyntax) **   <a name="personalize-CreateDatasetImportJob-request-roleArn"></a>
The ARN of the IAM role that has permissions to read from the Amazon S3 data source\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+`   
Required: Yes

 ** [tags](#API_CreateDatasetImportJob_RequestSyntax) **   <a name="personalize-CreateDatasetImportJob-request-tags"></a>
A list of [tags](https://docs.aws.amazon.com/personalize/latest/dev/tagging-resources.html) to apply to the dataset import job\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 200 items\.  
Required: No

## Response Syntax<a name="API_CreateDatasetImportJob_ResponseSyntax"></a>

```
{
   "datasetImportJobArn": "string"
}
```

## Response Elements<a name="API_CreateDatasetImportJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [datasetImportJobArn](#API_CreateDatasetImportJob_ResponseSyntax) **   <a name="personalize-CreateDatasetImportJob-response-datasetImportJobArn"></a>
The ARN of the dataset import job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+` 

## Errors<a name="API_CreateDatasetImportJob_Errors"></a>

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

 ** TooManyTagsException **   
You have exceeded the maximum number of tags you can apply to this resource\.   
HTTP Status Code: 400

## See Also<a name="API_CreateDatasetImportJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/CreateDatasetImportJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/CreateDatasetImportJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/CreateDatasetImportJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/CreateDatasetImportJob) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/CreateDatasetImportJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/CreateDatasetImportJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/CreateDatasetImportJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/CreateDatasetImportJob) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/CreateDatasetImportJob) 