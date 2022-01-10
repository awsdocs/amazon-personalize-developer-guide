# UpdateRecommender<a name="API_UpdateRecommender"></a>

Updates the recommender to modify the recommender configuration\.

## Request Syntax<a name="API_UpdateRecommender_RequestSyntax"></a>

```
{
   "recommenderArn": "string",
   "recommenderConfig": { 
      "itemExplorationConfig": { 
         "string" : "string" 
      }
   }
}
```

## Request Parameters<a name="API_UpdateRecommender_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [ recommenderArn ](#API_UpdateRecommender_RequestSyntax) **   <a name="personalize-UpdateRecommender-request-recommenderArn"></a>
The Amazon Resource Name \(ARN\) of the recommender to modify\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

 ** [ recommenderConfig ](#API_UpdateRecommender_RequestSyntax) **   <a name="personalize-UpdateRecommender-request-recommenderConfig"></a>
The configuration details of the recommender\.  
Type: [RecommenderConfig](API_RecommenderConfig.md) object  
Required: Yes

## Response Syntax<a name="API_UpdateRecommender_ResponseSyntax"></a>

```
{
   "recommenderArn": "string"
}
```

## Response Elements<a name="API_UpdateRecommender_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ recommenderArn ](#API_UpdateRecommender_ResponseSyntax) **   <a name="personalize-UpdateRecommender-response-recommenderArn"></a>
The same recommender Amazon Resource Name \(ARN\) as given in the request\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+` 

## Errors<a name="API_UpdateRecommender_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** ResourceInUseException **   
The specified resource is in use\.  
HTTP Status Code: 400

 ** ResourceNotFoundException **   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_UpdateRecommender_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [ AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/UpdateRecommender) 
+  [ AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/UpdateRecommender) 
+  [ AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/UpdateRecommender) 
+  [ AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/UpdateRecommender) 
+  [ AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/UpdateRecommender) 
+  [ AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/UpdateRecommender) 
+  [ AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/UpdateRecommender) 
+  [ AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/UpdateRecommender) 
+  [ AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/UpdateRecommender) 