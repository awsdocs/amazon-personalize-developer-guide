# DeleteMetricAttribution<a name="API_DeleteMetricAttribution"></a>

Deletes a metric attribution\.

## Request Syntax<a name="API_DeleteMetricAttribution_RequestSyntax"></a>

```
{
   "metricAttributionArn": "string"
}
```

## Request Parameters<a name="API_DeleteMetricAttribution_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [metricAttributionArn](#API_DeleteMetricAttribution_RequestSyntax) **   <a name="personalize-DeleteMetricAttribution-request-metricAttributionArn"></a>
The metric attribution's Amazon Resource Name \(ARN\)\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

## Response Elements<a name="API_DeleteMetricAttribution_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_DeleteMetricAttribution_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** ResourceInUseException **   
The specified resource is in use\.  
HTTP Status Code: 400

 ** ResourceNotFoundException **   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_DeleteMetricAttribution_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/DeleteMetricAttribution) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/DeleteMetricAttribution) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DeleteMetricAttribution) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DeleteMetricAttribution) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/DeleteMetricAttribution) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/DeleteMetricAttribution) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/DeleteMetricAttribution) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/DeleteMetricAttribution) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/DeleteMetricAttribution) 