# CreateFilter<a name="API_CreateFilter"></a>

Creates a recommendation filter\. For more information, see [Filtering recommendations and user segments](filter.md)\.

## Request Syntax<a name="API_CreateFilter_RequestSyntax"></a>

```
{
   "datasetGroupArn": "string",
   "filterExpression": "string",
   "name": "string"
}
```

## Request Parameters<a name="API_CreateFilter_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [ datasetGroupArn ](#API_CreateFilter_RequestSyntax) **   <a name="personalize-CreateFilter-request-datasetGroupArn"></a>
The ARN of the dataset group that the filter will belong to\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

 ** [ filterExpression ](#API_CreateFilter_RequestSyntax) **   <a name="personalize-CreateFilter-request-filterExpression"></a>
The filter expression defines which items are included or excluded from recommendations\. Filter expression must follow specific format rules\. For information about filter expression structure and syntax, see [Filter expressions](filter-expressions.md)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2500\.  
Required: Yes

 ** [ name ](#API_CreateFilter_RequestSyntax) **   <a name="personalize-CreateFilter-request-name"></a>
The name of the filter to create\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: Yes

## Response Syntax<a name="API_CreateFilter_ResponseSyntax"></a>

```
{
   "filterArn": "string"
}
```

## Response Elements<a name="API_CreateFilter_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ filterArn ](#API_CreateFilter_ResponseSyntax) **   <a name="personalize-CreateFilter-response-filterArn"></a>
The ARN of the new filter\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+` 

## Errors<a name="API_CreateFilter_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** LimitExceededException **   
The limit on the number of requests per second has been exceeded\.  
HTTP Status Code: 400

 ** ResourceAlreadyExistsException **   
The specified resource already exists\.  
HTTP Status Code: 400

 ** ResourceNotFoundException **   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_CreateFilter_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [ AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/CreateFilter) 
+  [ AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/CreateFilter) 
+  [ AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/CreateFilter) 
+  [ AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/CreateFilter) 
+  [ AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/CreateFilter) 
+  [ AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/CreateFilter) 
+  [ AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/CreateFilter) 
+  [ AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/CreateFilter) 
+  [ AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/CreateFilter) 