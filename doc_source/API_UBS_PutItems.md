# PutItems<a name="API_UBS_PutItems"></a>

Adds one or more items to an Items dataset\. For more information see [Importing Items Incrementally](https://docs.aws.amazon.com/personalize/latest/dg/importing-items.html)\. 

## Request Syntax<a name="API_UBS_PutItems_RequestSyntax"></a>

```
POST /items HTTP/1.1
Content-type: application/json

{
   "datasetArn": "string",
   "items": [ 
      { 
         "itemId": "string",
         "properties": "string"
      }
   ]
}
```

## URI Request Parameters<a name="API_UBS_PutItems_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_UBS_PutItems_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [datasetArn](#API_UBS_PutItems_RequestSyntax) **   <a name="personalize-UBS_PutItems-request-datasetArn"></a>
The Amazon Resource Name \(ARN\) of the Items dataset you are adding the item or items to\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

 ** [items](#API_UBS_PutItems_RequestSyntax) **   <a name="personalize-UBS_PutItems-request-items"></a>
A list of item data\.  
Type: Array of [Item](API_UBS_Item.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 10 items\.  
Required: Yes

## Response Syntax<a name="API_UBS_PutItems_ResponseSyntax"></a>

```
HTTP/1.1 200
```

## Response Elements<a name="API_UBS_PutItems_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_UBS_PutItems_Errors"></a>

 **InvalidInputException**   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 **ResourceInUseException**   
The specified resource is in use\.  
HTTP Status Code: 409

 **ResourceNotFoundException**   
Could not find the specified resource\.  
HTTP Status Code: 404

## See Also<a name="API_UBS_PutItems_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [ AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-events-2018-03-22/PutItems) 
+  [ AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-events-2018-03-22/PutItems) 
+  [ AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-events-2018-03-22/PutItems) 
+  [ AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-events-2018-03-22/PutItems) 
+  [ AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-events-2018-03-22/PutItems) 
+  [ AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-events-2018-03-22/PutItems) 
+  [ AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-events-2018-03-22/PutItems) 
+  [ AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-events-2018-03-22/PutItems) 
+  [ AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-events-2018-03-22/PutItems) 