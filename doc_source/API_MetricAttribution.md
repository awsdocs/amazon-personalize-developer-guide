# MetricAttribution<a name="API_MetricAttribution"></a>

Contains information on a metric attribution\. A metric attribution creates reports on the data that you import into Amazon Personalize\. Depending on how you import the data, you can view reports in Amazon CloudWatch or Amazon S3\. For more information, see [Measuring impact of recommendations](https://docs.aws.amazon.com/personalize/latest/dg/measuring-recommendation-impact.html)\.

## Contents<a name="API_MetricAttribution_Contents"></a>

 ** creationDateTime **   <a name="personalize-Type-MetricAttribution-creationDateTime"></a>
The metric attribution's creation date time\.  
Type: Timestamp  
Required: No

 ** datasetGroupArn **   <a name="personalize-Type-MetricAttribution-datasetGroupArn"></a>
The metric attribution's dataset group Amazon Resource Name \(ARN\)\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** failureReason **   <a name="personalize-Type-MetricAttribution-failureReason"></a>
The metric attribution's failure reason\.  
Type: String  
Required: No

 ** lastUpdatedDateTime **   <a name="personalize-Type-MetricAttribution-lastUpdatedDateTime"></a>
The metric attribution's last updated date time\.  
Type: Timestamp  
Required: No

 ** metricAttributionArn **   <a name="personalize-Type-MetricAttribution-metricAttributionArn"></a>
The metric attribution's Amazon Resource Name \(ARN\)\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** metricsOutputConfig **   <a name="personalize-Type-MetricAttribution-metricsOutputConfig"></a>
The metric attribution's output configuration\.  
Type: [MetricAttributionOutput](API_MetricAttributionOutput.md) object  
Required: No

 ** name **   <a name="personalize-Type-MetricAttribution-name"></a>
The metric attribution's name\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: No

 ** status **   <a name="personalize-Type-MetricAttribution-status"></a>
The metric attribution's status\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

## See Also<a name="API_MetricAttribution_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/MetricAttribution) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/MetricAttribution) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/MetricAttribution) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/MetricAttribution) 