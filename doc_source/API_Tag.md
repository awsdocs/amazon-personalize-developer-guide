# Tag<a name="API_Tag"></a>

The optional metadata that you apply to resources to help you categorize and organize them\. Each tag consists of a key and an optional value, both of which you define\. For more information see [Tagging Personalize resources](https://docs.aws.amazon.com/personalize/latest/dev/tagging-resources.html)\. 

## Contents<a name="API_Tag_Contents"></a>

 ** tagKey **   <a name="personalize-Type-Tag-tagKey"></a>
One part of a key\-value pair that makes up a tag\. A key is a general label that acts like a category for more specific tag values\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^([\p{L}\p{Z}\p{N}_.:/=+\-@]*)$`   
Required: Yes

 ** tagValue **   <a name="personalize-Type-Tag-tagValue"></a>
The optional part of a key\-value pair that makes up a tag\. A value acts as a descriptor within a tag category \(key\)\.  
Type: String  
Length Constraints: Minimum length of 0\. Maximum length of 256\.  
Pattern: `^([\p{L}\p{Z}\p{N}_.:/=+\-@]*)$`   
Required: Yes

## See Also<a name="API_Tag_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/Tag) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/Tag) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/Tag) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/Tag) 