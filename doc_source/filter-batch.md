# Filtering batch recommendations and user segments<a name="filter-batch"></a>

Filtering batch recommendations and user segments works nearly the same as filtering real\-time recommendations\. To filter batch recommendations or user segments, you [create a filter](filter-real-time.md)\. You can then apply it to a [ CreateBatchInferenceJob ](API_CreateBatchInferenceJob.md) or [ CreateBatchSegmentJob ](API_CreateBatchSegmentJob.md) operation, or a new batch inference job or batch segmentation job in the Amazon Personalize console\. Amazon Personalize then filters the recommendations from the batch job’s output JSON file\. For more information about batch workflows, see [Getting batch recommendations and user segments](recommendations-batch.md)\.

For filters with placeholder parameters, such as `$GENRE`, provide the values in a `filterValues` object in your input JSON\. For a `filterValues` object, each key is a parameter name and each value is the criteria that you are passing as a parameter\. For multiple values, separate each value with a comma\. The following is an example of a JSON input file with filter values\. The `GENRES` key corresponds to a `$GENRES` placeholder in the filter expression\.

```
{"userId": "5","filterValues":{"GENRES":"\"horror\",\"comedy\",\"drama\""}}
{"userId": "3","filterValues":{"GENRES":"\"horror\",\"comedy\""}}
{"userId": "34","filterValues":{"GENRES":"\"drama\""}}
```

 Filtering user segments works in the same way: 

```
{"itemAttributes": "ITEMS.genres = \"Comedy\" AND ITEMS.genres = \"Action\"","filterValues":{"COUNTRY":"\"Japan\""}}
{"itemAttributes": "ITEMS.genres = \"Horror\"","filterValues":{"COUNTRY":"\"United States\"\""}}
{"itemAttributes": "ITEMS.genres = \"Action\" AND ITEMS.genres = \"Adventure\"","filterValues":{"COUNTRY":"\"England\""}}
```

## Filtering batch workflows \(console\)<a name="filter-batch-recommendations-console"></a>

1.  Use the console or the SDK to [create a filter](filter-real-time.md)\. 

1. When you create the batch recommendation job or batch segment job, in **Filter configuration \- optional**, for **Filter name**, choose the filter\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/batch_filter.PNG)

## Filtering batch workflows \(AWS SDK\)<a name="filter-batch-recommendations-sdk"></a>

1. Use the console or the SDKs to [create a filter](filter-real-time.md)\. 

1.  Include the `FilterArn` parameter in the [ CreateBatchInferenceJob ](API_CreateBatchInferenceJob.md) or [ CreateBatchSegmentJob ](API_CreateBatchSegmentJob.md) request\. n example of a `create_batch_inference_job` method is below\.

```
import boto3
     
personalize = boto3.client("personalize")
 
personalize_rec.create_batch_inference_job (
    solutionVersionArn = "Solution version ARN",
    jobName = "Batch job name",
    roleArn = "IAM role ARN",
    filterArn = "Filter ARN",
    jobInput = 
        {"s3DataSource": {"path": "S3 input path"}},
    jobOutput = 
        {"S3DataDestination": {"path": "S3 output path"}}
)
```