# DescribeDatasetExportJob<a name="API_DescribeDatasetExportJob"></a>

Describes the dataset export job created by [CreateDatasetExportJob](API_CreateDatasetExportJob.md), including the export job status\.

## Request Syntax<a name="API_DescribeDatasetExportJob_RequestSyntax"></a>

```
{
   "datasetExportJobArn": "string"
}
```

## Request Parameters<a name="API_DescribeDatasetExportJob_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [datasetExportJobArn](#API_DescribeDatasetExportJob_RequestSyntax) **   <a name="personalize-DescribeDatasetExportJob-request-datasetExportJobArn"></a>
The Amazon Resource Name \(ARN\) of the dataset export job to describe\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

## Response Syntax<a name="API_DescribeDatasetExportJob_ResponseSyntax"></a>

```
{
   "datasetExportJob": { 
      "creationDateTime": number,
      "datasetArn": "string",
      "datasetExportJobArn": "string",
      "failureReason": "string",
      "ingestionMode": "string",
      "jobName": "string",
      "jobOutput": { 
         "s3DataDestination": { 
            "kmsKeyArn": "string",
            "path": "string"
         }
      },
      "lastUpdatedDateTime": number,
      "roleArn": "string",
      "status": "string"
   }
}
```

## Response Elements<a name="API_DescribeDatasetExportJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [datasetExportJob](#API_DescribeDatasetExportJob_ResponseSyntax) **   <a name="personalize-DescribeDatasetExportJob-response-datasetExportJob"></a>
Information about the dataset export job, including the status\.  
The status is one of the following values:  
+ CREATE PENDING
+ CREATE IN\_PROGRESS
+ ACTIVE
+ CREATE FAILED
Type: [DatasetExportJob](API_DatasetExportJob.md) object

## Errors<a name="API_DescribeDatasetExportJob_Errors"></a>

 **InvalidInputException**   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeDatasetExportJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/DescribeDatasetExportJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/DescribeDatasetExportJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DescribeDatasetExportJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DescribeDatasetExportJob) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/DescribeDatasetExportJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/DescribeDatasetExportJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/DescribeDatasetExportJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/DescribeDatasetExportJob) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/DescribeDatasetExportJob) 