# Filtering batch recommendations<a name="filter-batch"></a>

Filtering batch recommendations works nearly the same as filtering real\-time recommendations\. To filter batch recommendations, you [create a filter](filter-real-time.md) and then apply it to a [CreateBatchInferenceJob](API_CreateBatchInferenceJob.md) operation or new batch inference job in the Amazon Personalize console\. Amazon Personalize then filters the recommendations from the batch job’s output JSON file\. For more information about batch inference jobs, see [Creating a batch inference job \(console\)](recommendations-batch.md#batch-console)\.

For filters with placeholder parameters, such as `$GENRE`, provide the values in a `filterValues` object in your input JSON\. For a `filterValues` object, each key is a parameter name and each value is the criteria that you are passing as a parameter\. For multiple values, separate each value with a comma\. The following is an example of a JSON input file with filter values\. The `GENRES` key corresponds to a `$GENRES` placeholder in the filter expression\.

```
{"userId": "5","filterValues":{"GENRES":"\"horror\",\"comedy\",\"drama\""}}
{"userId": "3","filterValues":{"GENRES":"\"horror\",\"comedy\""}}
{"userId": "34","filterValues":{"GENRES":"\"drama\""}}
```

## Filtering batch recommendations \(console\)<a name="filter-batch-recommendations-console"></a>

1.  Use the console or the SDK to [create a filter](filter-real-time.md)\. 

1. When you create the batch recommendation job, on the **Create batch inference job** page, for **Filter configuration \- optional**, **Filter name**, choose the filter\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/batch_filter.PNG)

## Filtering batch recommendations \(AWS SDK\)<a name="filter-batch-recommendations-sdk"></a>

1.  Use the console or the SDK to [create a filter](filter-real-time.md)\. 

1.  Include the `FilterArn` parameter in the [CreateBatchInferenceJob](API_CreateBatchInferenceJob.md) request\. 

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