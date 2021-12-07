# BatchSegmentJobSummary<a name="API_BatchSegmentJobSummary"></a>

A truncated version of the [ BatchSegmentJob ](API_BatchSegmentJob.md) datatype\. The [ ListBatchSegmentJobs ](API_ListBatchSegmentJobs.md) operation returns a list of batch segment job summaries\.

## Contents<a name="API_BatchSegmentJobSummary_Contents"></a>

 ** batchSegmentJobArn **   <a name="personalize-Type-BatchSegmentJobSummary-batchSegmentJobArn"></a>
The Amazon Resource Name \(ARN\) of the batch segment job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** creationDateTime **   <a name="personalize-Type-BatchSegmentJobSummary-creationDateTime"></a>
The time at which the batch segment job was created\.  
Type: Timestamp  
Required: No

 ** failureReason **   <a name="personalize-Type-BatchSegmentJobSummary-failureReason"></a>
If the batch segment job failed, the reason for the failure\.  
Type: String  
Required: No

 ** jobName **   <a name="personalize-Type-BatchSegmentJobSummary-jobName"></a>
The name of the batch segment job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9][a-zA-Z0-9\-_]*`   
Required: No

 ** lastUpdatedDateTime **   <a name="personalize-Type-BatchSegmentJobSummary-lastUpdatedDateTime"></a>
The time at which the batch segment job was last updated\.  
Type: Timestamp  
Required: No

 ** solutionVersionArn **   <a name="personalize-Type-BatchSegmentJobSummary-solutionVersionArn"></a>
The Amazon Resource Name \(ARN\) of the solution version used by the batch segment job to generate batch segments\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: No

 ** status **   <a name="personalize-Type-BatchSegmentJobSummary-status"></a>
The status of the batch segment job\. The status is one of the following values:  
+ PENDING
+ IN PROGRESS
+ ACTIVE
+ CREATE FAILED
Type: String  
Length Constraints: Maximum length of 256\.  
Required: No

## See Also<a name="API_BatchSegmentJobSummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [ AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/BatchSegmentJobSummary) 
+  [ AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/BatchSegmentJobSummary) 
+  [ AWS SDK for Java V2](https://docs.aws.amazon.com/goto/SdkForJavaV2/personalize-2018-05-22/BatchSegmentJobSummary) 
+  [ AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/BatchSegmentJobSummary) 