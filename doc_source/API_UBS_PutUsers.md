# PutUsers<a name="API_UBS_PutUsers"></a>

Adds one or more users to a Users dataset\. For more information see [Importing Users Incrementally](https://docs.aws.amazon.com/personalize/latest/dg/importing-users.html)\.

## Request Syntax<a name="API_UBS_PutUsers_RequestSyntax"></a>

```
POST /users HTTP/1.1
Content-type: application/json

{
   "datasetArn": "string",
   "users": [ 
      { 
         "properties": "string",
         "userId": "string"
      }
   ]
}
```

## URI Request Parameters<a name="API_UBS_PutUsers_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_UBS_PutUsers_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [datasetArn](#API_UBS_PutUsers_RequestSyntax) **   <a name="personalize-UBS_PutUsers-request-datasetArn"></a>
The Amazon Resource Name \(ARN\) of the Users dataset you are adding the user or users to\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

 ** [users](#API_UBS_PutUsers_RequestSyntax) **   <a name="personalize-UBS_PutUsers-request-users"></a>
A list of user data\.  
Type: Array of [User](API_UBS_User.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 10 items\.  
Required: Yes

## Response Syntax<a name="API_UBS_PutUsers_ResponseSyntax"></a>

```
HTTP/1.1 200
```

## Response Elements<a name="API_UBS_PutUsers_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_UBS_PutUsers_Errors"></a>

 **InvalidInputException**   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 **ResourceInUseException**   
The specified resource is in use\.  
HTTP Status Code: 409

 **ResourceNotFoundException**   
Could not find the specified resource\.  
HTTP Status Code: 404

## See Also<a name="API_UBS_PutUsers_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [ AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-events-2018-03-22/PutUsers) 
+  [ AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-events-2018-03-22/PutUsers) 
+  [ AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-events-2018-03-22/PutUsers) 
+  [ AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-events-2018-03-22/PutUsers) 
+  [ AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-events-2018-03-22/PutUsers) 
+  [ AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-events-2018-03-22/PutUsers) 
+  [ AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-events-2018-03-22/PutUsers) 
+  [ AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-events-2018-03-22/PutUsers) 
+  [ AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-events-2018-03-22/PutUsers) 