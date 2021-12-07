# ListBatchSegmentJobs<a name="API_ListBatchSegmentJobs"></a>

Gets a list of the batch segment jobs that have been performed off of a solution version that you specify\.

## Request Syntax<a name="API_ListBatchSegmentJobs_RequestSyntax"></a>

```
{
   "maxResults": number,
   "nextToken": "string",
   "solutionVersionArn": "string"
}
```

## Request Parameters<a name="API_ListBatchSegmentJobs_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [ maxResults ](#API_ListBatchSegmentJobs_RequestSyntax) **   <a name="personalize-ListBatchSegmentJobs-request-maxResults"></a>
The maximum number of batch segment job results to return in each page\. The default value is 100\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [ nextToken ](#API_ListBatchSegmentJobs_RequestSyntax) **   <a name="personalize-ListBatchSegmentJobs-request-nextToken"></a>
The token to request the next page of results\.  
Type: String  
Length Constraints: Maximum length of 1500\.  
Required: No

 ** [ solutionVersionArn ](#API_ListBatchSegmentJobs_RequestSyntax) **   <a name="personalize-ListBatchSegmentJobs-request-solutionVersionArn"></a>
The Amazon Resource Name \(ARN\) of the solution version that the batch segment jobs used to generate batch segments\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

## Response Syntax<a name="API_ListBatchSegmentJobs_ResponseSyntax"></a>

```
{
   "batchSegmentJobs": [ 
      { 
         "batchSegmentJobArn": "string",
         "creationDateTime": number,
         "failureReason": "string",
         "jobName": "string",
         "lastUpdatedDateTime": number,
         "solutionVersionArn": "string",
         "status": "string"
      }
   ],
   "nextToken": "string"
}
```

## Response Elements<a name="API_ListBatchSegmentJobs_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ batchSegmentJobs ](#API_ListBatchSegmentJobs_ResponseSyntax) **   <a name="personalize-ListBatchSegmentJobs-response-batchSegmentJobs"></a>
A list containing information on each job that is returned\.  
Type: Array of [ BatchSegmentJobSummary ](API_BatchSegmentJobSummary.md) objects  
Array Members: Maximum number of 100 items\.

 ** [ nextToken ](#API_ListBatchSegmentJobs_ResponseSyntax) **   <a name="personalize-ListBatchSegmentJobs-response-nextToken"></a>
The token to use to retrieve the next page of results\. The value is `null` when there are no more results to return\.  
Type: String  
Length Constraints: Maximum length of 1500\.

## Errors<a name="API_ListBatchSegmentJobs_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** InvalidNextTokenException **   
The token is not valid\.  
HTTP Status Code: 400

## See Also<a name="API_ListBatchSegmentJobs_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [ AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/ListBatchSegmentJobs) 
+  [ AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/ListBatchSegmentJobs) 
+  [ AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/ListBatchSegmentJobs) 
+  [ AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/ListBatchSegmentJobs) 
+  [ AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/ListBatchSegmentJobs) 
+  [ AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/ListBatchSegmentJobs) 
+  [ AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/ListBatchSegmentJobs) 
+  [ AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/ListBatchSegmentJobs) 
+  [ AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/ListBatchSegmentJobs) 