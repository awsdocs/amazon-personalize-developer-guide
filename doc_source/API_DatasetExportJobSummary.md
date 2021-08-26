# DatasetExportJobSummary<a name="API_DatasetExportJobSummary"></a>

Provides a summary of the properties of a dataset export job\. For a complete listing, call the [DescribeDatasetExportJob](API_DescribeDatasetExportJob.md) API\.

## Contents<a name="API_DatasetExportJobSummary_Contents"></a>

 **creationDateTime**   <a name="personalize-Type-DatasetExportJobSummary-creationDateTime"></a>
The date and time \(in Unix time\) that the dataset export job was created\.  
Type: Timestamp  
Required: No

 **datasetExportJobArn**   <a name="personalize-Type-DatasetExportJobSummary-datasetExportJobArn"></a>
The Amazon Resource Name \(ARN\) of the dataset export job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 **failureReason**   <a name="personalize-Type-DatasetExportJobSummary-failureReason"></a>
If a dataset export job fails, the reason behind the failure\.  
Type: String  
Required: No

 **jobName**   <a name="personalize-Type-DatasetExportJobSummary-jobName"></a>
The name of the dataset export job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: No

 **lastUpdatedDateTime**   <a name="personalize-Type-DatasetExportJobSummary-lastUpdatedDateTime"></a>
The date and time \(in Unix time\) that the dataset export job status was last updated\.  
Type: Timestamp  
Required: No

 **status**   <a name="personalize-Type-DatasetExportJobSummary-status"></a>
The status of the dataset export job\.  
A dataset export job can be in one of the following states:  
+ CREATE PENDING > CREATE IN\_PROGRESS > ACTIVE \-or\- CREATE FAILED
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

## See Also<a name="API_DatasetExportJobSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [ AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DatasetExportJobSummary) 
+  [ AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DatasetExportJobSummary) 
+  [ AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/DatasetExportJobSummary) 
+  [ AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/DatasetExportJobSummary) 