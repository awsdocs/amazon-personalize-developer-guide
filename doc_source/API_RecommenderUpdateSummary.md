# RecommenderUpdateSummary<a name="API_RecommenderUpdateSummary"></a>

Provides a summary of the properties of a recommender update\. For a complete listing, call the [DescribeRecommender](https://docs.aws.amazon.com/personalize/latest/dg/API_DescribeRecommender.html) API\.

## Contents<a name="API_RecommenderUpdateSummary_Contents"></a>

 ** creationDateTime **   <a name="personalize-Type-RecommenderUpdateSummary-creationDateTime"></a>
The date and time \(in Unix format\) that the recommender update was created\.  
Type: Timestamp  
Required: No

 ** failureReason **   <a name="personalize-Type-RecommenderUpdateSummary-failureReason"></a>
If a recommender update fails, the reason behind the failure\.  
Type: String  
Required: No

 ** lastUpdatedDateTime **   <a name="personalize-Type-RecommenderUpdateSummary-lastUpdatedDateTime"></a>
The date and time \(in Unix time\) that the recommender update was last updated\.  
Type: Timestamp  
Required: No

 ** recommenderConfig **   <a name="personalize-Type-RecommenderUpdateSummary-recommenderConfig"></a>
The configuration details of the recommender update\.  
Type: [RecommenderConfig](API_RecommenderConfig.md) object  
Required: No

 ** status **   <a name="personalize-Type-RecommenderUpdateSummary-status"></a>
The status of the recommender update\.  
A recommender can be in one of the following states:  
+ CREATE PENDING > CREATE IN\_PROGRESS > ACTIVE \-or\- CREATE FAILED
+ DELETE PENDING > DELETE IN\_PROGRESS
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

## See Also<a name="API_RecommenderUpdateSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/RecommenderUpdateSummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/RecommenderUpdateSummary) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/RecommenderUpdateSummary) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/RecommenderUpdateSummary) 