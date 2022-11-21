# Promotion<a name="API_RS_Promotion"></a>

Contains information on a promotion\. A promotion defines additional business rules that apply to a configurable subset of recommended items\.

## Contents<a name="API_RS_Promotion_Contents"></a>

 ** filterArn **   <a name="personalize-Type-RS_Promotion-filterArn"></a>
The Amazon Resource Name \(ARN\) of the filter used by the promotion\. This filter defines the criteria for promoted items\. For more information, see [Promotion filters](https://docs.aws.amazon.com/personalize/latest/dg/promoting-items.html#promotion-filters)\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** filterValues **   <a name="personalize-Type-RS_Promotion-filterValues"></a>
The values to use when promoting items\. For each placeholder parameter in your promotion's filter expression, provide the parameter name \(in matching case\) as a key and the filter value\(s\) as the corresponding value\. Separate multiple values for one parameter with a comma\.   
For filter expressions that use an `INCLUDE` element to include items, you must provide values for all parameters that are defined in the expression\. For filters with expressions that use an `EXCLUDE` element to exclude items, you can omit the `filter-values`\. In this case, Amazon Personalize doesn't use that portion of the expression to filter recommendations\.  
For more information on creating filters, see [Filtering recommendations and user segments](https://docs.aws.amazon.com/personalize/latest/dg/filter.html)\.  
Type: String to string map  
Map Entries: Maximum number of 25 items\.  
Key Length Constraints: Maximum length of 50\.  
Key Pattern: `[A-Za-z0-9_]+`   
Value Length Constraints: Maximum length of 1000\.  
Required: No

 ** name **   <a name="personalize-Type-RS_Promotion-name"></a>
The name of the promotion\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: No

 ** percentPromotedItems **   <a name="personalize-Type-RS_Promotion-percentPromotedItems"></a>
The percentage of recommended items to apply the promotion to\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

## See Also<a name="API_RS_Promotion_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-runtime-2018-05-22/Promotion) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-runtime-2018-05-22/Promotion) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-runtime-2018-05-22/Promotion) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-runtime-2018-05-22/Promotion) 