# MetricAttribute<a name="API_MetricAttribute"></a>

Contains information on a metric that a metric attribution reports on\. For more information, see [Measuring impact of recommendations](https://docs.aws.amazon.com/personalize/latest/dg/measuring-recommendation-impact.html)\.

## Contents<a name="API_MetricAttribute_Contents"></a>

 ** eventType **   <a name="personalize-Type-MetricAttribute-eventType"></a>
The metric's event type\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

 ** expression **   <a name="personalize-Type-MetricAttribute-expression"></a>
The attribute's expression\. Available functions are `SUM()` or `SAMPLECOUNT()`\. For SUM\(\) functions, provide the dataset type \(either Interactions or Items\) and column to sum as a parameter\. For example SUM\(Items\.PRICE\)\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

 ** metricName **   <a name="personalize-Type-MetricAttribute-metricName"></a>
The metric's name\. The name helps you identify the metric in Amazon CloudWatch or Amazon S3\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: Yes

## See Also<a name="API_MetricAttribute_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/MetricAttribute) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/MetricAttribute) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/MetricAttribute) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/MetricAttribute) 