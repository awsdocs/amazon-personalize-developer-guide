# ListDatasetExportJobs<a name="API_ListDatasetExportJobs"></a>

Returns a list of dataset export jobs that use the given dataset\. When a dataset is not specified, all the dataset export jobs associated with the account are listed\. The response provides the properties for each dataset export job, including the Amazon Resource Name \(ARN\)\. For more information on dataset export jobs, see [CreateDatasetExportJob](https://docs.aws.amazon.com/personalize/latest/dg/API_CreateDatasetExportJob.html)\. For more information on datasets, see [CreateDataset](https://docs.aws.amazon.com/personalize/latest/dg/API_CreateDataset.html)\.

## Request Syntax<a name="API_ListDatasetExportJobs_RequestSyntax"></a>

```
{
   "datasetArn": "string",
   "maxResults": number,
   "nextToken": "string"
}
```

## Request Parameters<a name="API_ListDatasetExportJobs_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [datasetArn](#API_ListDatasetExportJobs_RequestSyntax) **   <a name="personalize-ListDatasetExportJobs-request-datasetArn"></a>
The Amazon Resource Name \(ARN\) of the dataset to list the dataset export jobs for\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** [maxResults](#API_ListDatasetExportJobs_RequestSyntax) **   <a name="personalize-ListDatasetExportJobs-request-maxResults"></a>
The maximum number of dataset export jobs to return\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [nextToken](#API_ListDatasetExportJobs_RequestSyntax) **   <a name="personalize-ListDatasetExportJobs-request-nextToken"></a>
A token returned from the previous call to `ListDatasetExportJobs` for getting the next set of dataset export jobs \(if they exist\)\.  
Type: String  
Length Constraints: Maximum length of 1500\.  
Required: No

## Response Syntax<a name="API_ListDatasetExportJobs_ResponseSyntax"></a>

```
{
   "datasetExportJobs": [ 
      { 
         "creationDateTime": number,
         "datasetExportJobArn": "string",
         "failureReason": "string",
         "jobName": "string",
         "lastUpdatedDateTime": number,
         "status": "string"
      }
   ],
   "nextToken": "string"
}
```

## Response Elements<a name="API_ListDatasetExportJobs_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [datasetExportJobs](#API_ListDatasetExportJobs_ResponseSyntax) **   <a name="personalize-ListDatasetExportJobs-response-datasetExportJobs"></a>
The list of dataset export jobs\.  
Type: Array of [DatasetExportJobSummary](API_DatasetExportJobSummary.md) objects  
Array Members: Maximum number of 100 items\.

 ** [nextToken](#API_ListDatasetExportJobs_ResponseSyntax) **   <a name="personalize-ListDatasetExportJobs-response-nextToken"></a>
A token for getting the next set of dataset export jobs \(if they exist\)\.  
Type: String  
Length Constraints: Maximum length of 1500\.

## Errors<a name="API_ListDatasetExportJobs_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** InvalidNextTokenException **   
The token is not valid\.  
HTTP Status Code: 400

## See Also<a name="API_ListDatasetExportJobs_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/ListDatasetExportJobs) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/ListDatasetExportJobs) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/ListDatasetExportJobs) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/ListDatasetExportJobs) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/ListDatasetExportJobs) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/ListDatasetExportJobs) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/ListDatasetExportJobs) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/ListDatasetExportJobs) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/ListDatasetExportJobs) 