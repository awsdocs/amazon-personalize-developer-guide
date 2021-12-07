# RecommenderSummary<a name="API_RecommenderSummary"></a>

Provides a summary of the properties of the recommender\.

## Contents<a name="API_RecommenderSummary_Contents"></a>

 ** creationDateTime **   <a name="personalize-Type-RecommenderSummary-creationDateTime"></a>
The date and time \(in Unix format\) that the recommender was created\.  
Type: Timestamp  
Required: No

 ** datasetGroupArn **   <a name="personalize-Type-RecommenderSummary-datasetGroupArn"></a>
The Amazon Resource Name \(ARN\) of the Domain dataset group that contains the recommender\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** lastUpdatedDateTime **   <a name="personalize-Type-RecommenderSummary-lastUpdatedDateTime"></a>
The date and time \(in Unix format\) that the recommender was last updated\.  
Type: Timestamp  
Required: No

 ** name **   <a name="personalize-Type-RecommenderSummary-name"></a>
The name of the recommender\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: No

 ** recipeArn **   <a name="personalize-Type-RecommenderSummary-recipeArn"></a>
The Amazon Resource Name \(ARN\) of the recipe \(Domain dataset group use case\) that the recommender was created for\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** recommenderArn **   <a name="personalize-Type-RecommenderSummary-recommenderArn"></a>
The Amazon Resource Name \(ARN\) of the recommender\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** recommenderConfig **   <a name="personalize-Type-RecommenderSummary-recommenderConfig"></a>
The configuration details of the recommender\.  
Type: [ RecommenderConfig ](API_RecommenderConfig.md) object  
Required: No

 ** status **   <a name="personalize-Type-RecommenderSummary-status"></a>
The status of the recommender\. A recommender can be in one of the following states:  
+ CREATE PENDING > CREATE IN\_PROGRESS > ACTIVE \-or\- CREATE FAILED
+ DELETE PENDING > DELETE IN\_PROGRESS
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

## See Also<a name="API_RecommenderSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [ AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/RecommenderSummary) 
+  [ AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/RecommenderSummary) 
+  [ AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/RecommenderSummary) 
+  [ AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/RecommenderSummary) 