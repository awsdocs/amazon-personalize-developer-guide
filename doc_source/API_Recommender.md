# Recommender<a name="API_Recommender"></a>

Describes a recommendation generator for a Domain dataset group\. You create a recommender in a Domain dataset group for a specific domain use case \(domain recipe\), and specify the recommender in a [GetRecommendations](https://docs.aws.amazon.com/personalize/latest/dg/API_RS_GetRecommendations.html) request\.

## Contents<a name="API_Recommender_Contents"></a>

 ** creationDateTime **   <a name="personalize-Type-Recommender-creationDateTime"></a>
The date and time \(in Unix format\) that the recommender was created\.  
Type: Timestamp  
Required: No

 ** datasetGroupArn **   <a name="personalize-Type-Recommender-datasetGroupArn"></a>
The Amazon Resource Name \(ARN\) of the Domain dataset group that contains the recommender\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** failureReason **   <a name="personalize-Type-Recommender-failureReason"></a>
If a recommender fails, the reason behind the failure\.  
Type: String  
Required: No

 ** lastUpdatedDateTime **   <a name="personalize-Type-Recommender-lastUpdatedDateTime"></a>
The date and time \(in Unix format\) that the recommender was last updated\.  
Type: Timestamp  
Required: No

 ** latestRecommenderUpdate **   <a name="personalize-Type-Recommender-latestRecommenderUpdate"></a>
Provides a summary of the latest updates to the recommender\.   
Type: [RecommenderUpdateSummary](API_RecommenderUpdateSummary.md) object  
Required: No

 ** modelMetrics **   <a name="personalize-Type-Recommender-modelMetrics"></a>
Provides evaluation metrics that help you determine the performance of a recommender\. For more information, see [ Evaluating a recommender](https://docs.aws.amazon.com/personalize/latest/dg/evaluating-recommenders.html)\.  
Type: String to double map  
Map Entries: Maximum number of 100 items\.  
Key Length Constraints: Maximum length of 256\.  
Required: No

 ** name **   <a name="personalize-Type-Recommender-name"></a>
The name of the recommender\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: No

 ** recipeArn **   <a name="personalize-Type-Recommender-recipeArn"></a>
The Amazon Resource Name \(ARN\) of the recipe \(Domain dataset group use case\) that the recommender was created for\.   
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** recommenderArn **   <a name="personalize-Type-Recommender-recommenderArn"></a>
The Amazon Resource Name \(ARN\) of the recommender\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** recommenderConfig **   <a name="personalize-Type-Recommender-recommenderConfig"></a>
The configuration details of the recommender\.  
Type: [RecommenderConfig](API_RecommenderConfig.md) object  
Required: No

 ** status **   <a name="personalize-Type-Recommender-status"></a>
The status of the recommender\.  
A recommender can be in one of the following states:  
+ CREATE PENDING > CREATE IN\_PROGRESS > ACTIVE \-or\- CREATE FAILED
+ STOP PENDING > STOP IN\_PROGRESS > INACTIVE > START PENDING > START IN\_PROGRESS > ACTIVE
+ DELETE PENDING > DELETE IN\_PROGRESS
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

## See Also<a name="API_Recommender_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/Recommender) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/Recommender) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/Recommender) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/Recommender) 