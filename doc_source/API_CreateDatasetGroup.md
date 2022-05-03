# CreateDatasetGroup<a name="API_CreateDatasetGroup"></a>

Creates an empty dataset group\. A dataset group is a container for Amazon Personalize resources\. A dataset group can contain at most three datasets, one for each type of dataset:
+ Interactions
+ Items
+ Users

 A dataset group can be a Domain dataset group, where you specify a domain and use pre\-configured resources like recommenders, or a Custom dataset group, where you use custom resources, such as a solution with a solution version, that you deploy with a campaign\. If you start with a Domain dataset group, you can still add custom resources such as solutions and solution versions trained with recipes for custom use cases and deployed with campaigns\. 

A dataset group can be in one of the following states:
+ CREATE PENDING > CREATE IN\_PROGRESS > ACTIVE \-or\- CREATE FAILED
+ DELETE PENDING

To get the status of the dataset group, call [DescribeDatasetGroup](https://docs.aws.amazon.com/personalize/latest/dg/API_DescribeDatasetGroup.html)\. If the status shows as CREATE FAILED, the response includes a `failureReason` key, which describes why the creation failed\.

**Note**  
You must wait until the `status` of the dataset group is `ACTIVE` before adding a dataset to the group\.

You can specify an AWS Key Management Service \(KMS\) key to encrypt the datasets in the group\. If you specify a KMS key, you must also include an AWS Identity and Access Management \(IAM\) role that has permission to access the key\.

**APIs that require a dataset group ARN in the request**
+  [CreateDataset](https://docs.aws.amazon.com/personalize/latest/dg/API_CreateDataset.html) 
+  [CreateEventTracker](https://docs.aws.amazon.com/personalize/latest/dg/API_CreateEventTracker.html) 
+  [CreateSolution](https://docs.aws.amazon.com/personalize/latest/dg/API_CreateSolution.html) 

**Related APIs**
+  [ListDatasetGroups](https://docs.aws.amazon.com/personalize/latest/dg/API_ListDatasetGroups.html) 
+  [DescribeDatasetGroup](https://docs.aws.amazon.com/personalize/latest/dg/API_DescribeDatasetGroup.html) 
+  [DeleteDatasetGroup](https://docs.aws.amazon.com/personalize/latest/dg/API_DeleteDatasetGroup.html) 

## Request Syntax<a name="API_CreateDatasetGroup_RequestSyntax"></a>

```
{
   "domain": "string",
   "kmsKeyArn": "string",
   "name": "string",
   "roleArn": "string",
   "tags": [ 
      { 
         "tagKey": "string",
         "tagValue": "string"
      }
   ]
}
```

## Request Parameters<a name="API_CreateDatasetGroup_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [domain](#API_CreateDatasetGroup_RequestSyntax) **   <a name="personalize-CreateDatasetGroup-request-domain"></a>
The domain of the dataset group\. Specify a domain to create a Domain dataset group\. The domain you specify determines the default schemas for datasets and the use cases available for recommenders\. If you don't specify a domain, you create a Custom dataset group with solution versions that you deploy with a campaign\.   
Type: String  
Valid Values:` ECOMMERCE | VIDEO_ON_DEMAND`   
Required: No

 ** [kmsKeyArn](#API_CreateDatasetGroup_RequestSyntax) **   <a name="personalize-CreateDatasetGroup-request-kmsKeyArn"></a>
The Amazon Resource Name \(ARN\) of a AWS Key Management Service \(KMS\) key used to encrypt the datasets\.  
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `arn:aws.*:kms:.*:[0-9]{12}:key/.*`   
Required: No

 ** [name](#API_CreateDatasetGroup_RequestSyntax) **   <a name="personalize-CreateDatasetGroup-request-name"></a>
The name for the new dataset group\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: Yes

 ** [roleArn](#API_CreateDatasetGroup_RequestSyntax) **   <a name="personalize-CreateDatasetGroup-request-roleArn"></a>
The ARN of the AWS Identity and Access Management \(IAM\) role that has permissions to access the AWS Key Management Service \(KMS\) key\. Supplying an IAM role is only valid when also specifying a KMS key\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+`   
Required: No

 ** [tags](#API_CreateDatasetGroup_RequestSyntax) **   <a name="personalize-CreateDatasetGroup-request-tags"></a>
A list of [tags](https://docs.aws.amazon.com/personalize/latest/dev/tagging-resources.html) to apply to the dataset group\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 200 items\.  
Required: No

## Response Syntax<a name="API_CreateDatasetGroup_ResponseSyntax"></a>

```
{
   "datasetGroupArn": "string",
   "domain": "string"
}
```

## Response Elements<a name="API_CreateDatasetGroup_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [datasetGroupArn](#API_CreateDatasetGroup_ResponseSyntax) **   <a name="personalize-CreateDatasetGroup-response-datasetGroupArn"></a>
The Amazon Resource Name \(ARN\) of the new dataset group\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+` 

 ** [domain](#API_CreateDatasetGroup_ResponseSyntax) **   <a name="personalize-CreateDatasetGroup-response-domain"></a>
The domain for the new Domain dataset group\.  
Type: String  
Valid Values:` ECOMMERCE | VIDEO_ON_DEMAND` 

## Errors<a name="API_CreateDatasetGroup_Errors"></a>

 ** InvalidInputException **   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 ** LimitExceededException **   
The limit on the number of requests per second has been exceeded\.  
HTTP Status Code: 400

 ** ResourceAlreadyExistsException **   
The specified resource already exists\.  
HTTP Status Code: 400

 ** TooManyTagsException **   
You have exceeded the maximum number of tags you can apply to this resource\.   
HTTP Status Code: 400

## See Also<a name="API_CreateDatasetGroup_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/CreateDatasetGroup) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/CreateDatasetGroup) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/CreateDatasetGroup) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/CreateDatasetGroup) 
+  [AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/CreateDatasetGroup) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/CreateDatasetGroup) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/CreateDatasetGroup) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/CreateDatasetGroup) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/CreateDatasetGroup) 