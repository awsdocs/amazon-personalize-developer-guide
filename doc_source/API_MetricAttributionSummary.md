# MetricAttributionSummary<a name="API_MetricAttributionSummary"></a>

Provides a summary of the properties of a metric attribution\. For a complete listing, call the [DescribeMetricAttribution](https://docs.aws.amazon.com/personalize/latest/dg/API_DescribeMetricAttribution.html)\.

## Contents<a name="API_MetricAttributionSummary_Contents"></a>

 ** creationDateTime **   <a name="personalize-Type-MetricAttributionSummary-creationDateTime"></a>
The metric attribution's creation date time\.  
Type: Timestamp  
Required: No

 ** failureReason **   <a name="personalize-Type-MetricAttributionSummary-failureReason"></a>
The metric attribution's failure reason\.  
Type: String  
Required: No

 ** lastUpdatedDateTime **   <a name="personalize-Type-MetricAttributionSummary-lastUpdatedDateTime"></a>
The metric attribution's last updated date time\.  
Type: Timestamp  
Required: No

 ** metricAttributionArn **   <a name="personalize-Type-MetricAttributionSummary-metricAttributionArn"></a>
The metric attribution's Amazon Resource Name \(ARN\)\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** name **   <a name="personalize-Type-MetricAttributionSummary-name"></a>
The name of the metric attribution\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: No

 ** status **   <a name="personalize-Type-MetricAttributionSummary-status"></a>
The metric attribution's status\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

## See Also<a name="API_MetricAttributionSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/MetricAttributionSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/MetricAttributionSummary) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/MetricAttributionSummary) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/MetricAttributionSummary) 