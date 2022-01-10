# ListRecommenders<a name="API_ListRecommenders"></a>

Returns a list of recommenders in a given Domain dataset group\. When a Domain dataset group is not specified, all the recommenders associated with the account are listed\. The response provides the properties for each recommender, including the Amazon Resource Name \(ARN\)\. For more information on recommenders, see [CreateRecommender](https://docs.aws.amazon.com/personalize/latest/dg/API_CreateRecommender.html)\.

## Request Syntax<a name="API_ListRecommenders_RequestSyntax"></a>

```
{
   "datasetGroupArn": "string",
   "maxResults": number,
   "nextToken": "string"
}
```

## Request Parameters<a name="API_ListRecommenders_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [ datasetGroupArn ](#API_ListRecommenders_RequestSyntax) **   <a name="personalize-ListRecommenders-request-datasetGroupArn"></a>
The Amazon Resource Name \(ARN\) of the Domain dataset group to list the recommenders for\. When a Domain dataset group is not specified, all the recommenders associated with the account are listed\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** [ maxResults ](#API_ListRecommenders_RequestSyntax) **   <a name="personalize-ListRecommenders-request-maxResults"></a>
The maximum number of recommenders to return\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [ nextToken ](#API_ListRecommenders_RequestSyntax) **   <a name="personalize-ListRecommenders-request-nextToken"></a>
A token returned from the previous call to `ListRecommenders` for getting the next set of recommenders \(if they exist\)\.  
Type: String  
Length Constraints: Maximum length of 1500\.  
Required: No

## Response Syntax<a name="API_ListRecommenders_ResponseSyntax"></a>

```
{
   "nextToken": "string",
   "recommenders": [ 
      { 
         "creationDateTime": number,
         "datasetGroupArn": "string",
         "lastUpdatedDateTime": number,
         "name": "string",
         "recipeArn": "string",
         "recommenderArn": "string",
         "recommenderConfig": { 
            "itemExplorationConfig": { 
               "string" : "string" 
            }
         },
         "status": "string"
      }
   ]
}
```

## Response Elements<a name="API_ListRecommenders_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ nextToken ](#API_ListRecommenders_ResponseSyntax) **   <a name="personalize-ListRecommenders-response-nextToken"></a>
A token for getting the next set of recommenders \(if they exist\)\.  
Type: String  
Length Constraints: Maximum length of 1500\.

 ** [ recommenders ](#API_ListRecommenders_ResponseSyntax) **   <a name="personalize-ListRecommenders-response-recommenders"></a>
A list of the recommenders\.  
Type: Array of [RecommenderSummary](API_RecommenderSummary.md) objects  
Array Members: Maximum number of 100 items\.

## Errors<a name="API_ListRecommenders_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** InvalidNextTokenException **   
The token is not valid\.  
HTTP Status Code: 400

## See Also<a name="API_ListRecommenders_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [ AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/ListRecommenders) 
+  [ AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/ListRecommenders) 
+  [ AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/ListRecommenders) 
+  [ AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/ListRecommenders) 
+  [ AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/ListRecommenders) 
+  [ AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/ListRecommenders) 
+  [ AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/ListRecommenders) 
+  [ AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/ListRecommenders) 
+  [ AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/ListRecommenders) 