# GetRecommendations<a name="API_RS_GetRecommendations"></a>

Returns a list of recommended items\. For campaigns, the campaign's Amazon Resource Name \(ARN\) is required and the required user and item input depends on the recipe type used to create the solution backing the campaign as follows:
+ USER\_PERSONALIZATION \- `userId` required, `itemId` not used
+ RELATED\_ITEMS \- `itemId` required, `userId` not used

**Note**  
Campaigns that are backed by a solution created using a recipe of type PERSONALIZED\_RANKING use the [ GetPersonalizedRanking ](API_RS_GetPersonalizedRanking.md) API\.

 For recommenders, the recommender's ARN is required and the required item and user input depends on the use case \(domain\-based recipe\) backing the recommender\. For information on use case requirements see [Choosing recommender use cases](https://docs.aws.amazon.com/personalize/latest/dg/domain-use-cases.html)\. 

## Request Syntax<a name="API_RS_GetRecommendations_RequestSyntax"></a>

```
POST /recommendations HTTP/1.1
Content-type: application/json

{
   "campaignArn": "string",
   "context": { 
      "string" : "string" 
   },
   "filterArn": "string",
   "filterValues": { 
      "string" : "string" 
   },
   "itemId": "string",
   "numResults": number,
   "recommenderArn": "string",
   "userId": "string"
}
```

## URI Request Parameters<a name="API_RS_GetRecommendations_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_RS_GetRecommendations_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [ campaignArn ](#API_RS_GetRecommendations_RequestSyntax) **   <a name="personalize-RS_GetRecommendations-request-campaignArn"></a>
The Amazon Resource Name \(ARN\) of the campaign to use for getting recommendations\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** [ context ](#API_RS_GetRecommendations_RequestSyntax) **   <a name="personalize-RS_GetRecommendations-request-context"></a>
The contextual metadata to use when getting recommendations\. Contextual metadata includes any interaction information that might be relevant when getting a user's recommendations, such as the user's current location or device type\.  
Type: String to string map  
Map Entries: Maximum number of 150 items\.  
Key Length Constraints: Maximum length of 150\.  
Key Pattern: `[A-Za-z\d_]+`   
Value Length Constraints: Maximum length of 1000\.  
Required: No

 ** [ filterArn ](#API_RS_GetRecommendations_RequestSyntax) **   <a name="personalize-RS_GetRecommendations-request-filterArn"></a>
The ARN of the filter to apply to the returned recommendations\. For more information, see [Filtering Recommendations](https://docs.aws.amazon.com/personalize/latest/dg/filter.html)\.  
When using this parameter, be sure the filter resource is `ACTIVE`\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** [ filterValues ](#API_RS_GetRecommendations_RequestSyntax) **   <a name="personalize-RS_GetRecommendations-request-filterValues"></a>
The values to use when filtering recommendations\. For each placeholder parameter in your filter expression, provide the parameter name \(in matching case\) as a key and the filter value\(s\) as the corresponding value\. Separate multiple values for one parameter with a comma\.   
For filter expressions that use an `INCLUDE` element to include items, you must provide values for all parameters that are defined in the expression\. For filters with expressions that use an `EXCLUDE` element to exclude items, you can omit the `filter-values`\.In this case, Amazon Personalize doesn't use that portion of the expression to filter recommendations\.  
For more information, see [Filtering Recommendations](https://docs.aws.amazon.com/personalize/latest/dg/filter.html)\.  
Type: String to string map  
Map Entries: Maximum number of 25 items\.  
Key Length Constraints: Maximum length of 50\.  
Key Pattern: `[A-Za-z0-9_]+`   
Value Length Constraints: Maximum length of 1000\.  
Required: No

 ** [ itemId ](#API_RS_GetRecommendations_RequestSyntax) **   <a name="personalize-RS_GetRecommendations-request-itemId"></a>
The item ID to provide recommendations for\.  
Required for `RELATED_ITEMS` recipe type\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

 ** [ numResults ](#API_RS_GetRecommendations_RequestSyntax) **   <a name="personalize-RS_GetRecommendations-request-numResults"></a>
The number of results to return\. The default is 25\. The maximum is 500\.  
Type: Integer  
Valid Range: Minimum value of 0\.  
Required: No

 ** [ recommenderArn ](#API_RS_GetRecommendations_RequestSyntax) **   <a name="personalize-RS_GetRecommendations-request-recommenderArn"></a>
The Amazon Resource Name \(ARN\) of the recommender to use to get recommendations\. Provide a recommender ARN if you created a Domain dataset group with a recommender for a domain use case\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** [ userId ](#API_RS_GetRecommendations_RequestSyntax) **   <a name="personalize-RS_GetRecommendations-request-userId"></a>
The user ID to provide recommendations for\.  
Required for `USER_PERSONALIZATION` recipe type\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

## Response Syntax<a name="API_RS_GetRecommendations_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "itemList": [ 
      { 
         "itemId": "string",
         "score": number
      }
   ],
   "recommendationId": "string"
}
```

## Response Elements<a name="API_RS_GetRecommendations_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ itemList ](#API_RS_GetRecommendations_ResponseSyntax) **   <a name="personalize-RS_GetRecommendations-response-itemList"></a>
A list of recommendations sorted in descending order by prediction score\. There can be a maximum of 500 items in the list\.  
Type: Array of [ PredictedItem ](API_RS_PredictedItem.md) objects

 ** [ recommendationId ](#API_RS_GetRecommendations_ResponseSyntax) **   <a name="personalize-RS_GetRecommendations-response-recommendationId"></a>
The ID of the recommendation\.  
Type: String

## Errors<a name="API_RS_GetRecommendations_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** ResourceNotFoundException **   
The specified resource does not exist\.  
HTTP Status Code: 404

## See Also<a name="API_RS_GetRecommendations_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [ AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-runtime-2018-05-22/GetRecommendations) 
+  [ AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-runtime-2018-05-22/GetRecommendations) 
+  [ AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-runtime-2018-05-22/GetRecommendations) 
+  [ AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-runtime-2018-05-22/GetRecommendations) 
+  [ AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-runtime-2018-05-22/GetRecommendations) 
+  [ AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-runtime-2018-05-22/GetRecommendations) 
+  [ AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-runtime-2018-05-22/GetRecommendations) 
+  [ AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-runtime-2018-05-22/GetRecommendations) 
+  [ AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-runtime-2018-05-22/GetRecommendations) 