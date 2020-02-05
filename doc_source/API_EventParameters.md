# EventParameters<a name="API_EventParameters"></a>

Contains the properties associated with an event\.

## Contents<a name="API_EventParameters_Contents"></a>

 **eventType**   <a name="personalize-Type-EventParameters-eventType"></a>
The type of event\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

 **eventValueThreshold**   <a name="personalize-Type-EventParameters-eventValueThreshold"></a>
The minimum value required for the event to take on a custom weight\. Events with values below the threshold will be given the the default weight\.  
Type: Double  
Valid Range: Minimum value of \-1000000\. Maximum value of 1000000\.  
Required: No

 **weight**   <a name="personalize-Type-EventParameters-weight"></a>
The relative value with which to weigh the event when including it in solution training\.  
Type: Double  
Valid Range: Minimum value of 0\. Maximum value of 1\.  
Required: No

## See Also<a name="API_EventParameters_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/EventParameters) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/EventParameters) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/personalize-2018-05-22/EventParameters) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/personalize-2018-05-22/EventParameters) 