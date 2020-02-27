# DescribeBatchInferenceJob<a name="API_DescribeBatchInferenceJob"></a>

Gets the properties of a batch inference job including name, Amazon Resource Name \(ARN\), status, input and output configurations, and the ARN of the solution version used to generate the recommendations\.

## Request Syntax<a name="API_DescribeBatchInferenceJob_RequestSyntax"></a>

```
{
   "[batchInferenceJobArn](#personalize-DescribeBatchInferenceJob-request-batchInferenceJobArn)": "string"
}
```

## Request Parameters<a name="API_DescribeBatchInferenceJob_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [batchInferenceJobArn](#API_DescribeBatchInferenceJob_RequestSyntax) **   <a name="personalize-DescribeBatchInferenceJob-request-batchInferenceJobArn"></a>
The ARN of the batch inference job to describe\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

## Response Syntax<a name="API_DescribeBatchInferenceJob_ResponseSyntax"></a>

```
{
   "[batchInferenceJob](#personalize-DescribeBatchInferenceJob-response-batchInferenceJob)": { 
      "[batchInferenceJobArn](API_BatchInferenceJob.md#personalize-Type-BatchInferenceJob-batchInferenceJobArn)": "string",
      "[creationDateTime](API_BatchInferenceJob.md#personalize-Type-BatchInferenceJob-creationDateTime)": number,
      "[failureReason](API_BatchInferenceJob.md#personalize-Type-BatchInferenceJob-failureReason)": "string",
      "[jobInput](API_BatchInferenceJob.md#personalize-Type-BatchInferenceJob-jobInput)": { 
         "[s3DataSource](API_BatchInferenceJobInput.md#personalize-Type-BatchInferenceJobInput-s3DataSource)": { 
            "[kmsKeyArn](API_S3DataConfig.md#personalize-Type-S3DataConfig-kmsKeyArn)": "string",
            "[path](API_S3DataConfig.md#personalize-Type-S3DataConfig-path)": "string"
         }
      },
      "[jobName](API_BatchInferenceJob.md#personalize-Type-BatchInferenceJob-jobName)": "string",
      "[jobOutput](API_BatchInferenceJob.md#personalize-Type-BatchInferenceJob-jobOutput)": { 
         "[s3DataDestination](API_BatchInferenceJobOutput.md#personalize-Type-BatchInferenceJobOutput-s3DataDestination)": { 
            "[kmsKeyArn](API_S3DataConfig.md#personalize-Type-S3DataConfig-kmsKeyArn)": "string",
            "[path](API_S3DataConfig.md#personalize-Type-S3DataConfig-path)": "string"
         }
      },
      "[lastUpdatedDateTime](API_BatchInferenceJob.md#personalize-Type-BatchInferenceJob-lastUpdatedDateTime)": number,
      "[numResults](API_BatchInferenceJob.md#personalize-Type-BatchInferenceJob-numResults)": number,
      "[roleArn](API_BatchInferenceJob.md#personalize-Type-BatchInferenceJob-roleArn)": "string",
      "[solutionVersionArn](API_BatchInferenceJob.md#personalize-Type-BatchInferenceJob-solutionVersionArn)": "string",
      "[status](API_BatchInferenceJob.md#personalize-Type-BatchInferenceJob-status)": "string"
   }
}
```

## Response Elements<a name="API_DescribeBatchInferenceJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [batchInferenceJob](#API_DescribeBatchInferenceJob_ResponseSyntax) **   <a name="personalize-DescribeBatchInferenceJob-response-batchInferenceJob"></a>
Information on the specified batch inference job\.  
Type: [BatchInferenceJob](API_BatchInferenceJob.md) object

## Errors<a name="API_DescribeBatchInferenceJob_Errors"></a>

 **InvalidInputException**   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeBatchInferenceJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/DescribeBatchInferenceJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/DescribeBatchInferenceJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DescribeBatchInferenceJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DescribeBatchInferenceJob) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/personalize-2018-05-22/DescribeBatchInferenceJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/DescribeBatchInferenceJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/DescribeBatchInferenceJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/DescribeBatchInferenceJob) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/DescribeBatchInferenceJob) 