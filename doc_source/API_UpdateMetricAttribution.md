# UpdateMetricAttribution<a name="API_UpdateMetricAttribution"></a>

Updates a metric attribution\.

## Request Syntax<a name="API_UpdateMetricAttribution_RequestSyntax"></a>

```
{
   "addMetrics": [ 
      { 
         "eventType": "string",
         "expression": "string",
         "metricName": "string"
      }
   ],
   "metricAttributionArn": "string",
   "metricsOutputConfig": { 
      "roleArn": "string",
      "s3DataDestination": { 
         "kmsKeyArn": "string",
         "path": "string"
      }
   },
   "removeMetrics": [ "string" ]
}
```

## Request Parameters<a name="API_UpdateMetricAttribution_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [addMetrics](#API_UpdateMetricAttribution_RequestSyntax) **   <a name="personalize-UpdateMetricAttribution-request-addMetrics"></a>
Add new metric attributes to the metric attribution\.  
Type: Array of [MetricAttribute](API_MetricAttribute.md) objects  
Array Members: Maximum number of 10 items\.  
Required: No

 ** [metricAttributionArn](#API_UpdateMetricAttribution_RequestSyntax) **   <a name="personalize-UpdateMetricAttribution-request-metricAttributionArn"></a>
The Amazon Resource Name \(ARN\) for the metric attribution to update\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** [metricsOutputConfig](#API_UpdateMetricAttribution_RequestSyntax) **   <a name="personalize-UpdateMetricAttribution-request-metricsOutputConfig"></a>
An output config for the metric attribution\.  
Type: [MetricAttributionOutput](API_MetricAttributionOutput.md) object  
Required: No

 ** [removeMetrics](#API_UpdateMetricAttribution_RequestSyntax) **   <a name="personalize-UpdateMetricAttribution-request-removeMetrics"></a>
Remove metric attributes from the metric attribution\.  
Type: Array of strings  
Array Members: Maximum number of 10 items\.  
Length Constraints: Maximum length of 256\.  
Required: No

## Response Syntax<a name="API_UpdateMetricAttribution_ResponseSyntax"></a>

```
{
   "metricAttributionArn": "string"
}
```

## Response Elements<a name="API_UpdateMetricAttribution_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [metricAttributionArn](#API_UpdateMetricAttribution_ResponseSyntax) **   <a name="personalize-UpdateMetricAttribution-response-metricAttributionArn"></a>
The Amazon Resource Name \(ARN\) for the metric attribution that you updated\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+` 

## Errors<a name="API_UpdateMetricAttribution_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
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

## See Also<a name="API_UpdateMetricAttribution_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/UpdateMetricAttribution) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/UpdateMetricAttribution) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/UpdateMetricAttribution) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/UpdateMetricAttribution) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/UpdateMetricAttribution) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/UpdateMetricAttribution) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/UpdateMetricAttribution) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/UpdateMetricAttribution) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/UpdateMetricAttribution) 