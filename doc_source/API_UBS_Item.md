# Item<a name="API_UBS_Item"></a>

Represents item metadata added to an Items dataset using the `PutItems` API\. For more information see [Importing Items Incrementally](https://docs.aws.amazon.com/personalize/latest/dg/importing-items.html)\. 

## Contents<a name="API_UBS_Item_Contents"></a>

 **itemId**   <a name="personalize-Type-UBS_Item-itemId"></a>
The ID associated with the item\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Required: Yes

 **properties**   <a name="personalize-Type-UBS_Item-properties"></a>
A string map of item\-specific metadata\. Each element in the map consists of a key\-value pair\. For example, `{"numberOfRatings": "12"}`\.  
The keys use camel case names that match the fields in the schema for the Items dataset\. In the previous example, the `numberOfRatings` matches the 'NUMBER\_OF\_RATINGS' field defined in the Items schema\. For categorical string data, to include multiple categories for a single item, separate each category with a pipe separator \(`|`\)\. For example, `\"Horror|Action\"`\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 24262\.  
Required: No

## See Also<a name="API_UBS_Item_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-events-2018-03-22/Item) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-events-2018-03-22/Item) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-events-2018-03-22/Item) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-events-2018-03-22/Item) 