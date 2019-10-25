# DescribeDataset<a name="API_DescribeDataset"></a>

Describes the given dataset\. For more information on datasets, see [CreateDataset](API_CreateDataset.md)\.

## Request Syntax<a name="API_DescribeDataset_RequestSyntax"></a>

```
{
   "[datasetArn](#personalize-DescribeDataset-request-datasetArn)": "string"
}
```

## Request Parameters<a name="API_DescribeDataset_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [datasetArn](#API_DescribeDataset_RequestSyntax) **   <a name="personalize-DescribeDataset-request-datasetArn"></a>
The Amazon Resource Name \(ARN\) of the dataset to describe\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

## Response Syntax<a name="API_DescribeDataset_ResponseSyntax"></a>

```
{
   "[dataset](#personalize-DescribeDataset-response-dataset)": { 
      "[creationDateTime](API_Dataset.md#personalize-Type-Dataset-creationDateTime)": number,
      "[datasetArn](API_Dataset.md#personalize-Type-Dataset-datasetArn)": "string",
      "[datasetGroupArn](API_Dataset.md#personalize-Type-Dataset-datasetGroupArn)": "string",
      "[datasetType](API_Dataset.md#personalize-Type-Dataset-datasetType)": "string",
      "[lastUpdatedDateTime](API_Dataset.md#personalize-Type-Dataset-lastUpdatedDateTime)": number,
      "[name](API_Dataset.md#personalize-Type-Dataset-name)": "string",
      "[schemaArn](API_Dataset.md#personalize-Type-Dataset-schemaArn)": "string",
      "[status](API_Dataset.md#personalize-Type-Dataset-status)": "string"
   }
}
```

## Response Elements<a name="API_DescribeDataset_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [dataset](#API_DescribeDataset_ResponseSyntax) **   <a name="personalize-DescribeDataset-response-dataset"></a>
A listing of the dataset's properties\.  
Type: [Dataset](API_Dataset.md) object

## Errors<a name="API_DescribeDataset_Errors"></a>

 **InvalidInputException**   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeDataset_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/DescribeDataset) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/DescribeDataset) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DescribeDataset) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DescribeDataset) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/personalize-2018-05-22/DescribeDataset) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/DescribeDataset) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/DescribeDataset) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/DescribeDataset) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/personalize-2018-05-22/DescribeDataset) 