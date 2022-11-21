# CreateMetricAttribution<a name="API_CreateMetricAttribution"></a>

Creates a metric attribution\. A metric attribution creates reports on the data that you import into Amazon Personalize\. Depending on how you imported the data, you can view reports in Amazon CloudWatch or Amazon S3\. For more information, see [Measuring impact of recommendations](https://docs.aws.amazon.com/personalize/latest/dg/measuring-recommendation-impact.html)\.

## Request Syntax<a name="API_CreateMetricAttribution_RequestSyntax"></a>

```
{
   "datasetGroupArn": "string",
   "metrics": [ 
      { 
         "eventType": "string",
         "expression": "string",
         "metricName": "string"
      }
   ],
   "metricsOutputConfig": { 
      "roleArn": "string",
      "s3DataDestination": { 
         "kmsKeyArn": "string",
         "path": "string"
      }
   },
   "name": "string"
}
```

## Request Parameters<a name="API_CreateMetricAttribution_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [datasetGroupArn](#API_CreateMetricAttribution_RequestSyntax) **   <a name="personalize-CreateMetricAttribution-request-datasetGroupArn"></a>
The Amazon Resource Name \(ARN\) of the destination dataset group for the metric attribution\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

 ** [metrics](#API_CreateMetricAttribution_RequestSyntax) **   <a name="personalize-CreateMetricAttribution-request-metrics"></a>
A list of metric attributes for the metric attribution\. Each metric attribute specifies an event type to track and a function\. Available functions are `SUM()` or `SAMPLECOUNT()`\. For SUM\(\) functions, provide the dataset type \(either Interactions or Items\) and column to sum as a parameter\. For example SUM\(Items\.PRICE\)\.  
Type: Array of [MetricAttribute](API_MetricAttribute.md) objects  
Array Members: Maximum number of 10 items\.  
Required: Yes

 ** [metricsOutputConfig](#API_CreateMetricAttribution_RequestSyntax) **   <a name="personalize-CreateMetricAttribution-request-metricsOutputConfig"></a>
The output configuration details for the metric attribution\.  
Type: [MetricAttributionOutput](API_MetricAttributionOutput.md) object  
Required: Yes

 ** [name](#API_CreateMetricAttribution_RequestSyntax) **   <a name="personalize-CreateMetricAttribution-request-name"></a>
A name for the metric attribution\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: Yes

## Response Syntax<a name="API_CreateMetricAttribution_ResponseSyntax"></a>

```
{
   "metricAttributionArn": "string"
}
```

## Response Elements<a name="API_CreateMetricAttribution_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [metricAttributionArn](#API_CreateMetricAttribution_ResponseSyntax) **   <a name="personalize-CreateMetricAttribution-response-metricAttributionArn"></a>
The Amazon Resource Name \(ARN\) for the new metric attribution\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+` 

## Errors<a name="API_CreateMetricAttribution_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** LimitExceededException **   
The limit on the number of requests per second has been exceeded\.  
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

## See Also<a name="API_CreateMetricAttribution_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/CreateMetricAttribution) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/CreateMetricAttribution) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/CreateMetricAttribution) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/CreateMetricAttribution) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/CreateMetricAttribution) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/CreateMetricAttribution) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/CreateMetricAttribution) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/CreateMetricAttribution) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/CreateMetricAttribution) 