# User<a name="API_UBS_User"></a>

Represents user metadata added to a Users dataset using the `PutUsers` API\.

## Contents<a name="API_UBS_User_Contents"></a>

 **properties**   <a name="personalize-Type-UBS_User-properties"></a>
A string map of user\-specific metadata\. Each element in the map consists of a key\-value pair\. For example,   
 `{"numberOfVideosWatched": "45"}`   
The keys use camel case names that match the fields in the Users schema\. In the above example, the `numberOfVideosWatched` would match the 'NUMBER\_OF\_VIDEOS\_WATCHED' field defined in the Users schema\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Required: No

 **userId**   <a name="personalize-Type-UBS_User-userId"></a>
The ID associated with the user\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Required: Yes

## See Also<a name="API_UBS_User_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-events-2018-03-22/User) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-events-2018-03-22/User) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/personalize-events-2018-03-22/User) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-events-2018-03-22/User) 