# ListMetricAttributions<a name="API_ListMetricAttributions"></a>

Lists metric attributions\.

## Request Syntax<a name="API_ListMetricAttributions_RequestSyntax"></a>

```
{
   "datasetGroupArn": "string",
   "maxResults": number,
   "nextToken": "string"
}
```

## Request Parameters<a name="API_ListMetricAttributions_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [datasetGroupArn](#API_ListMetricAttributions_RequestSyntax) **   <a name="personalize-ListMetricAttributions-request-datasetGroupArn"></a>
The metric attributions' dataset group Amazon Resource Name \(ARN\)\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** [maxResults](#API_ListMetricAttributions_RequestSyntax) **   <a name="personalize-ListMetricAttributions-request-maxResults"></a>
The maximum number of metric attributions to return in one page of results\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [nextToken](#API_ListMetricAttributions_RequestSyntax) **   <a name="personalize-ListMetricAttributions-request-nextToken"></a>
Specify the pagination token from a previous request to retrieve the next page of results\.  
Type: String  
Length Constraints: Maximum length of 1500\.  
Required: No

## Response Syntax<a name="API_ListMetricAttributions_ResponseSyntax"></a>

```
{
   "metricAttributions": [ 
      { 
         "creationDateTime": number,
         "failureReason": "string",
         "lastUpdatedDateTime": number,
         "metricAttributionArn": "string",
         "name": "string",
         "status": "string"
      }
   ],
   "nextToken": "string"
}
```

## Response Elements<a name="API_ListMetricAttributions_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [metricAttributions](#API_ListMetricAttributions_ResponseSyntax) **   <a name="personalize-ListMetricAttributions-response-metricAttributions"></a>
The list of metric attributions\.  
Type: Array of [MetricAttributionSummary](API_MetricAttributionSummary.md) objects  
Array Members: Maximum number of 100 items\.

 ** [nextToken](#API_ListMetricAttributions_ResponseSyntax) **   <a name="personalize-ListMetricAttributions-response-nextToken"></a>
Specify the pagination token from a previous request to retrieve the next page of results\.  
Type: String  
Length Constraints: Maximum length of 1500\.

## Errors<a name="API_ListMetricAttributions_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** InvalidNextTokenException **   
The token is not valid\.  
HTTP Status Code: 400

## See Also<a name="API_ListMetricAttributions_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/ListMetricAttributions) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/ListMetricAttributions) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/ListMetricAttributions) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/ListMetricAttributions) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/ListMetricAttributions) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/ListMetricAttributions) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/ListMetricAttributions) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/ListMetricAttributions) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/ListMetricAttributions) 