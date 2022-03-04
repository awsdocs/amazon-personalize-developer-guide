# DescribeBatchSegmentJob<a name="API_DescribeBatchSegmentJob"></a>

Gets the properties of a batch segment job including name, Amazon Resource Name \(ARN\), status, input and output configurations, and the ARN of the solution version used to generate segments\.

## Request Syntax<a name="API_DescribeBatchSegmentJob_RequestSyntax"></a>

```
{
   "batchSegmentJobArn": "string"
}
```

## Request Parameters<a name="API_DescribeBatchSegmentJob_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [batchSegmentJobArn](#API_DescribeBatchSegmentJob_RequestSyntax) **   <a name="personalize-DescribeBatchSegmentJob-request-batchSegmentJobArn"></a>
The ARN of the batch segment job to describe\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

## Response Syntax<a name="API_DescribeBatchSegmentJob_ResponseSyntax"></a>

```
{
   "batchSegmentJob": { 
      "batchSegmentJobArn": "string",
      "creationDateTime": number,
      "failureReason": "string",
      "filterArn": "string",
      "jobInput": { 
         "s3DataSource": { 
            "kmsKeyArn": "string",
            "path": "string"
         }
      },
      "jobName": "string",
      "jobOutput": { 
         "s3DataDestination": { 
            "kmsKeyArn": "string",
            "path": "string"
         }
      },
      "lastUpdatedDateTime": number,
      "numResults": number,
      "roleArn": "string",
      "solutionVersionArn": "string",
      "status": "string"
   }
}
```

## Response Elements<a name="API_DescribeBatchSegmentJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [batchSegmentJob](#API_DescribeBatchSegmentJob_ResponseSyntax) **   <a name="personalize-DescribeBatchSegmentJob-response-batchSegmentJob"></a>
Information on the specified batch segment job\.  
Type: [BatchSegmentJob](API_BatchSegmentJob.md) object

## Errors<a name="API_DescribeBatchSegmentJob_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** ResourceNotFoundException **   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeBatchSegmentJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/DescribeBatchSegmentJob) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/DescribeBatchSegmentJob) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DescribeBatchSegmentJob) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DescribeBatchSegmentJob) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/DescribeBatchSegmentJob) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/DescribeBatchSegmentJob) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/DescribeBatchSegmentJob) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/DescribeBatchSegmentJob) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/DescribeBatchSegmentJob) 