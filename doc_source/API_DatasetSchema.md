# DatasetSchema<a name="API_DatasetSchema"></a>

Describes the schema for a dataset\. For more information on schemas, see [CreateSchema](API_CreateSchema.md)\.

## Contents<a name="API_DatasetSchema_Contents"></a>

 ** creationDateTime **   <a name="personalize-Type-DatasetSchema-creationDateTime"></a>
The date and time \(in Unix time\) that the schema was created\.  
Type: Timestamp  
Required: No

 ** domain **   <a name="personalize-Type-DatasetSchema-domain"></a>
The domain of a schema that you created for a dataset in a Domain dataset group\.  
Type: String  
Valid Values:` ECOMMERCE | VIDEO_ON_DEMAND`   
Required: No

 ** lastUpdatedDateTime **   <a name="personalize-Type-DatasetSchema-lastUpdatedDateTime"></a>
The date and time \(in Unix time\) that the schema was last updated\.  
Type: Timestamp  
Required: No

 ** name **   <a name="personalize-Type-DatasetSchema-name"></a>
The name of the schema\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: No

 ** schema **   <a name="personalize-Type-DatasetSchema-schema"></a>
The schema\.  
Type: String  
Length Constraints: Maximum length of 10000\.  
Required: No

 ** schemaArn **   <a name="personalize-Type-DatasetSchema-schemaArn"></a>
The Amazon Resource Name \(ARN\) of the schema\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

## See Also<a name="API_DatasetSchema_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [ AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DatasetSchema) 
+  [ AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DatasetSchema) 
+  [ AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/DatasetSchema) 
+  [ AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/DatasetSchema) 