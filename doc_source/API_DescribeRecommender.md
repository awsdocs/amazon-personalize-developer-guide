# DescribeRecommender<a name="API_DescribeRecommender"></a>

Describes the given recommender, including its status\.

A recommender can be in one of the following states:
+ CREATE PENDING > CREATE IN\_PROGRESS > ACTIVE \-or\- CREATE FAILED
+ DELETE PENDING > DELETE IN\_PROGRESS

When the `status` is `CREATE FAILED`, the response includes the `failureReason` key, which describes why\.

For more information on recommenders, see [CreateRecommender](https://docs.aws.amazon.com/personalize/latest/dg/API_CreateRecommender.html)\.

## Request Syntax<a name="API_DescribeRecommender_RequestSyntax"></a>

```
{
   "recommenderArn": "string"
}
```

## Request Parameters<a name="API_DescribeRecommender_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [ recommenderArn ](#API_DescribeRecommender_RequestSyntax) **   <a name="personalize-DescribeRecommender-request-recommenderArn"></a>
The Amazon Resource Name \(ARN\) of the recommender to describe\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

## Response Syntax<a name="API_DescribeRecommender_ResponseSyntax"></a>

```
{
   "recommender": { 
      "creationDateTime": number,
      "datasetGroupArn": "string",
      "failureReason": "string",
      "lastUpdatedDateTime": number,
      "latestRecommenderUpdate": { 
         "creationDateTime": number,
         "failureReason": "string",
         "lastUpdatedDateTime": number,
         "recommenderConfig": { 
            "itemExplorationConfig": { 
               "string" : "string" 
            }
         },
         "status": "string"
      },
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
}
```

## Response Elements<a name="API_DescribeRecommender_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ recommender ](#API_DescribeRecommender_ResponseSyntax) **   <a name="personalize-DescribeRecommender-response-recommender"></a>
The properties of the recommender\.  
Type: [Recommender](API_Recommender.md) object

## Errors<a name="API_DescribeRecommender_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** ResourceNotFoundException **   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeRecommender_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [ AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/DescribeRecommender) 
+  [ AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/DescribeRecommender) 
+  [ AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DescribeRecommender) 
+  [ AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DescribeRecommender) 
+  [ AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/DescribeRecommender) 
+  [ AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/DescribeRecommender) 
+  [ AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/DescribeRecommender) 
+  [ AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/DescribeRecommender) 
+  [ AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/DescribeRecommender) 