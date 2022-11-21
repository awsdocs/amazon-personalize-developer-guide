# DescribeMetricAttribution<a name="API_DescribeMetricAttribution"></a>

Describes a metric attribution\.

## Request Syntax<a name="API_DescribeMetricAttribution_RequestSyntax"></a>

```
{
   "metricAttributionArn": "string"
}
```

## Request Parameters<a name="API_DescribeMetricAttribution_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [metricAttributionArn](#API_DescribeMetricAttribution_RequestSyntax) **   <a name="personalize-DescribeMetricAttribution-request-metricAttributionArn"></a>
The metric attribution's Amazon Resource Name \(ARN\)\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

## Response Syntax<a name="API_DescribeMetricAttribution_ResponseSyntax"></a>

```
{
   "metricAttribution": { 
      "creationDateTime": number,
      "datasetGroupArn": "string",
      "failureReason": "string",
      "lastUpdatedDateTime": number,
      "metricAttributionArn": "string",
      "metricsOutputConfig": { 
         "roleArn": "string",
         "s3DataDestination": { 
            "kmsKeyArn": "string",
            "path": "string"
         }
      },
      "name": "string",
      "status": "string"
   }
}
```

## Response Elements<a name="API_DescribeMetricAttribution_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [metricAttribution](#API_DescribeMetricAttribution_ResponseSyntax) **   <a name="personalize-DescribeMetricAttribution-response-metricAttribution"></a>
The details of the metric attribution\.  
Type: [MetricAttribution](API_MetricAttribution.md) object

## Errors<a name="API_DescribeMetricAttribution_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** ResourceNotFoundException **   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeMetricAttribution_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/DescribeMetricAttribution) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/DescribeMetricAttribution) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DescribeMetricAttribution) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DescribeMetricAttribution) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/DescribeMetricAttribution) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/DescribeMetricAttribution) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/DescribeMetricAttribution) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/DescribeMetricAttribution) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/DescribeMetricAttribution) 