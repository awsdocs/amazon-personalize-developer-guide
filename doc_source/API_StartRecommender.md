# StartRecommender<a name="API_StartRecommender"></a>

Starts a recommender that is INACTIVE\. Starting a recommender does not create any new models, but resumes billing and automatic retraining for the recommender\.

## Request Syntax<a name="API_StartRecommender_RequestSyntax"></a>

```
{
   "recommenderArn": "string"
}
```

## Request Parameters<a name="API_StartRecommender_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [recommenderArn](#API_StartRecommender_RequestSyntax) **   <a name="personalize-StartRecommender-request-recommenderArn"></a>
The Amazon Resource Name \(ARN\) of the recommender to start\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

## Response Syntax<a name="API_StartRecommender_ResponseSyntax"></a>

```
{
   "recommenderArn": "string"
}
```

## Response Elements<a name="API_StartRecommender_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [recommenderArn](#API_StartRecommender_ResponseSyntax) **   <a name="personalize-StartRecommender-response-recommenderArn"></a>
The Amazon Resource Name \(ARN\) of the recommender you started\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+` 

## Errors<a name="API_StartRecommender_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** ResourceInUseException **   
The specified resource is in use\.  
HTTP Status Code: 400

 ** ResourceNotFoundException **   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_StartRecommender_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/StartRecommender) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/StartRecommender) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/StartRecommender) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/StartRecommender) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/StartRecommender) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/StartRecommender) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/StartRecommender) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/StartRecommender) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/StartRecommender) 