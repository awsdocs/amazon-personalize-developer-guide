# RecommenderConfig<a name="API_RecommenderConfig"></a>

The configuration details of the recommender\.

## Contents<a name="API_RecommenderConfig_Contents"></a>

 ** itemExplorationConfig **   <a name="personalize-Type-RecommenderConfig-itemExplorationConfig"></a>
Specifies the exploration configuration hyperparameters, including `explorationWeight` and `explorationItemAgeCutOff`, you want to use to configure the amount of item exploration Amazon Personalize uses when recommending items\. Provide `itemExplorationConfig` data only if your recommenders generate personalized recommendations for a user \(not popular items or similar items\)\.  
Type: String to string map  
Map Entries: Maximum number of 100 items\.  
Key Length Constraints: Maximum length of 256\.  
Value Length Constraints: Maximum length of 1000\.  
Required: No

 ** minRecommendationRequestsPerSecond **   <a name="personalize-Type-RecommenderConfig-minRecommendationRequestsPerSecond"></a>
Specifies the requested minimum provisioned recommendation requests per second that Amazon Personalize will support\.  
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

## See Also<a name="API_RecommenderConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/RecommenderConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/RecommenderConfig) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/RecommenderConfig) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/RecommenderConfig) 