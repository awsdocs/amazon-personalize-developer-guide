# PredictedItem<a name="API_RS_PredictedItem"></a>

An object that identifies an item\.

The [GetRecommendations](API_RS_GetRecommendations.md) and [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) APIs return a list of `PredictedItem`s\.

## Contents<a name="API_RS_PredictedItem_Contents"></a>

 ** itemId **   <a name="personalize-Type-RS_PredictedItem-itemId"></a>
The recommended item ID\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

 ** promotionName **   <a name="personalize-Type-RS_PredictedItem-promotionName"></a>
The name of the promotion that included the predicted item\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: No

 ** score **   <a name="personalize-Type-RS_PredictedItem-score"></a>
A numeric representation of the model's certainty that the item will be the next user selection\. For more information on scoring logic, see [Recommendation scores](getting-recommendations.md#how-scores-work)\.  
Type: Double  
Required: No

## See Also<a name="API_RS_PredictedItem_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-runtime-2018-05-22/PredictedItem) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-runtime-2018-05-22/PredictedItem) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-runtime-2018-05-22/PredictedItem) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-runtime-2018-05-22/PredictedItem) 