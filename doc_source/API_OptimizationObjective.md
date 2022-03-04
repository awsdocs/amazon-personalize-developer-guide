# OptimizationObjective<a name="API_OptimizationObjective"></a>

Describes the additional objective for the solution, such as maximizing streaming minutes or increasing revenue\. For more information see [Optimizing a solution](https://docs.aws.amazon.com/personalize/latest/dg/optimizing-solution-for-objective.html)\.

## Contents<a name="API_OptimizationObjective_Contents"></a>

 ** itemAttribute **   <a name="personalize-Type-OptimizationObjective-itemAttribute"></a>
The numerical metadata column in an Items dataset related to the optimization objective\. For example, VIDEO\_LENGTH \(to maximize streaming minutes\), or PRICE \(to maximize revenue\)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 150\.  
Required: No

 ** objectiveSensitivity **   <a name="personalize-Type-OptimizationObjective-objectiveSensitivity"></a>
Specifies how Amazon Personalize balances the importance of your optimization objective versus relevance\.  
Type: String  
Valid Values:` LOW | MEDIUM | HIGH | OFF`   
Required: No

## See Also<a name="API_OptimizationObjective_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/OptimizationObjective) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/OptimizationObjective) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/OptimizationObjective) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/OptimizationObjective) 