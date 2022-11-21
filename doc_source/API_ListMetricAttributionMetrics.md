# ListMetricAttributionMetrics<a name="API_ListMetricAttributionMetrics"></a>

Lists the metrics for the metric attribution\.

## Request Syntax<a name="API_ListMetricAttributionMetrics_RequestSyntax"></a>

```
{
   "maxResults": number,
   "metricAttributionArn": "string",
   "nextToken": "string"
}
```

## Request Parameters<a name="API_ListMetricAttributionMetrics_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [maxResults](#API_ListMetricAttributionMetrics_RequestSyntax) **   <a name="personalize-ListMetricAttributionMetrics-request-maxResults"></a>
The maximum number of metrics to return in one page of results\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [metricAttributionArn](#API_ListMetricAttributionMetrics_RequestSyntax) **   <a name="personalize-ListMetricAttributionMetrics-request-metricAttributionArn"></a>
The Amazon Resource Name \(ARN\) of the metric attribution to retrieve attributes for\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** [nextToken](#API_ListMetricAttributionMetrics_RequestSyntax) **   <a name="personalize-ListMetricAttributionMetrics-request-nextToken"></a>
Specify the pagination token from a previous request to retrieve the next page of results\.  
Type: String  
Length Constraints: Maximum length of 1500\.  
Required: No

## Response Syntax<a name="API_ListMetricAttributionMetrics_ResponseSyntax"></a>

```
{
   "metrics": [ 
      { 
         "eventType": "string",
         "expression": "string",
         "metricName": "string"
      }
   ],
   "nextToken": "string"
}
```

## Response Elements<a name="API_ListMetricAttributionMetrics_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [metrics](#API_ListMetricAttributionMetrics_ResponseSyntax) **   <a name="personalize-ListMetricAttributionMetrics-response-metrics"></a>
The metrics for the specified metric attribution\.  
Type: Array of [MetricAttribute](API_MetricAttribute.md) objects  
Array Members: Maximum number of 10 items\.

 ** [nextToken](#API_ListMetricAttributionMetrics_ResponseSyntax) **   <a name="personalize-ListMetricAttributionMetrics-response-nextToken"></a>
Specify the pagination token from a previous `ListMetricAttributionMetricsResponse` request to retrieve the next page of results\.  
Type: String  
Length Constraints: Maximum length of 1500\.

## Errors<a name="API_ListMetricAttributionMetrics_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** InvalidNextTokenException **   
The token is not valid\.  
HTTP Status Code: 400

## See Also<a name="API_ListMetricAttributionMetrics_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/ListMetricAttributionMetrics) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/ListMetricAttributionMetrics) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/ListMetricAttributionMetrics) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/ListMetricAttributionMetrics) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/ListMetricAttributionMetrics) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/ListMetricAttributionMetrics) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/ListMetricAttributionMetrics) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/ListMetricAttributionMetrics) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/ListMetricAttributionMetrics) 