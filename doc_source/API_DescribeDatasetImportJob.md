# DescribeDatasetImportJob<a name="API_DescribeDatasetImportJob"></a>

Describes the dataset import job created by [CreateDatasetImportJob](API_CreateDatasetImportJob.md), including the import job status\.

## Request Syntax<a name="API_DescribeDatasetImportJob_RequestSyntax"></a>

```
{
   "[datasetImportJobArn](#personalize-DescribeDatasetImportJob-request-datasetImportJobArn)": "string"
}
```

## Request Parameters<a name="API_DescribeDatasetImportJob_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [datasetImportJobArn](#API_DescribeDatasetImportJob_RequestSyntax) **   <a name="personalize-DescribeDatasetImportJob-request-datasetImportJobArn"></a>
The Amazon Resource Name \(ARN\) of the dataset import job to describe\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

## Response Syntax<a name="API_DescribeDatasetImportJob_ResponseSyntax"></a>

```
{
   "[datasetImportJob](#personalize-DescribeDatasetImportJob-response-datasetImportJob)": { 
      "[creationDateTime](API_DatasetImportJob.md#personalize-Type-DatasetImportJob-creationDateTime)": number,
      "[datasetArn](API_DatasetImportJob.md#personalize-Type-DatasetImportJob-datasetArn)": "string",
      "[datasetImportJobArn](API_DatasetImportJob.md#personalize-Type-DatasetImportJob-datasetImportJobArn)": "string",
      "[dataSource](API_DatasetImportJob.md#personalize-Type-DatasetImportJob-dataSource)": { 
         "[dataLocation](API_DataSource.md#personalize-Type-DataSource-dataLocation)": "string"
      },
      "[failureReason](API_DatasetImportJob.md#personalize-Type-DatasetImportJob-failureReason)": "string",
      "[jobName](API_DatasetImportJob.md#personalize-Type-DatasetImportJob-jobName)": "string",
      "[lastUpdatedDateTime](API_DatasetImportJob.md#personalize-Type-DatasetImportJob-lastUpdatedDateTime)": number,
      "[roleArn](API_DatasetImportJob.md#personalize-Type-DatasetImportJob-roleArn)": "string",
      "[status](API_DatasetImportJob.md#personalize-Type-DatasetImportJob-status)": "string"
   }
}
```

## Response Elements<a name="API_DescribeDatasetImportJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [datasetImportJob](#API_DescribeDatasetImportJob_ResponseSyntax) **   <a name="personalize-DescribeDatasetImportJob-response-datasetImportJob"></a>
Information about the dataset import job, including the status\.  
The status is one of the following values:  
+ CREATE PENDING
+ CREATE IN\_PROGRESS
+ ACTIVE
+ CREATE FAILED
Type: [DatasetImportJob](API_DatasetImportJob.md) object

## Errors<a name="API_DescribeDatasetImportJob_Errors"></a>

 **InvalidInputException**   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeDatasetImportJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/DescribeDatasetImportJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/DescribeDatasetImportJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DescribeDatasetImportJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DescribeDatasetImportJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/personalize-2018-05-22/DescribeDatasetImportJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/DescribeDatasetImportJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/DescribeDatasetImportJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/DescribeDatasetImportJob) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/personalize-2018-05-22/DescribeDatasetImportJob) 