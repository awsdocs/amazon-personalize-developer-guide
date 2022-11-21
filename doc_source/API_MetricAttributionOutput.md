# MetricAttributionOutput<a name="API_MetricAttributionOutput"></a>

The output configuration details for a metric attribution\.

## Contents<a name="API_MetricAttributionOutput_Contents"></a>

 ** roleArn **   <a name="personalize-Type-MetricAttributionOutput-roleArn"></a>
The Amazon Resource Name \(ARN\) of the IAM service role that has permissions to add data to your output Amazon S3 bucket and add metrics to Amazon CloudWatch\. For more information, see [Measuring impact of recommendations](https://docs.aws.amazon.com/personalize/latest/dg/measuring-recommendation-impact.html)\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+`   
Required: Yes

 ** s3DataDestination **   <a name="personalize-Type-MetricAttributionOutput-s3DataDestination"></a>
The configuration details of an Amazon S3 input or output bucket\.  
Type: [S3DataConfig](API_S3DataConfig.md) object  
Required: No

## See Also<a name="API_MetricAttributionOutput_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/MetricAttributionOutput) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/MetricAttributionOutput) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/MetricAttributionOutput) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/MetricAttributionOutput) 