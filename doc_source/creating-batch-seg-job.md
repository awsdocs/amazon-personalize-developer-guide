# Creating a batch segment job<a name="creating-batch-seg-job"></a>

 If you used a USER\_SEGMENTATION recipe, you can create batch segment jobs to get user segments with your solution version\. Each user segment is sorted in descending order based on the probability that each user will interact with items in your inventory\. Depending on the recipe, your input data must be a list of items \([Item\-Affinity recipe](item-affinity-recipe.md)\) or item attributes \([Item\-Attribute\-Affinity recipe](item-attribute-affinity-recipe.md)\) in JSON format\. You can create a batch segment job with the Amazon Personalize console, the AWS Command Line Interface \(AWS CLI\), or AWS SDKs\. 

 For more information about the batch workflow in Amazon Personalize, including permissions requirements and preparing and importing input data, see [Getting batch recommendations and user segments](recommendations-batch.md)\. 

**Topics**
+ [Creating a batch segment job \(console\)](#batch-segment-console)
+ [Creating a batch segment job \(AWS CLI\)](#batch-segment-cli)
+ [Creating a batch segment job \(AWS SDKs\)](#batch-segment-sdk)

## Creating a batch segment job \(console\)<a name="batch-segment-console"></a>

 After you have completed [Preparing and importing batch input data](batch-data-upload.md), you are ready to create a batch segment job\. This procedure assumes that you have already created a solution and a solution version \(trained model\) with the Item\-Affinity recipe\. 

**To get create a batch segment job \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. On the **Datasets group** page, choose your dataset group\.

1. Choose **batch segment jobs** in the navigation pane, then choose **Create batch segment job**\.

1. In **batch segment job details**, for **Batch segment job name**, specify a name for your batch segment job\.

1. For **Solution**, choose the solution and then choose the **Solution version ID** that you want to use to generate the recommendations\. You can create batch segment jobs only if you used a USER\_SEGEMENTATION recipe\. 

1. For **Number of users**, optionally specify the number of users Amazon Personalize generates for each user segment\. The default is 25\.

1.  For **Input source**, specify the Amazon S3 path to your input file or use the **Browse S3** to choose your Amazon S3 bucket\.

   Use the following syntax: **s3://<name of your S3 bucket>/<folder name>/<input JSON file name>**

    Your input data must be in the correct format for the recipe your solution uses\. For input data examples see [Input and output JSON examples](batch-data-upload.md#batch-recommendations-json-examples)\. 

1. For **Output destination**, specify the path to your output location or use the **Browse S3** to choose your Amazon S3 bucket\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\.

    Use the following syntax: **s3://<name of your S3 bucket>/<output folder name>/** 

1. For **IAM role**, choose one of the following:
   +  Choose **Create and use new service role** and enter the **Service role name** to create a new role, or
   +  If you've already created a role with the correct permissions, choose **Use an existing service role** and choose the IAM role\. 

    The role you use must have read and write access to your input and output Amazon S3 buckets respectively\.

1.  For **Filters** optionally choose a filter to apply a filter to the recommendations added to the output JSON file\. For more information see [Filtering batch recommendations and user segments](filter-batch.md)\. 

1. For **Tags**, optionally add any tags\. For more information about tagging Amazon Personalize resources, see [Tagging Amazon Personalize resources](tagging-resources.md)\.

1.  Choose **Create batch segment job**\. Batch segment job creation starts and the **Batch segment jobs** page appears with the **Batch segment job detail** section displayed\.

1.  When the batch segment job's status changes to **Active**, you can retrieve the job's output from the designated output Amazon S3 bucket\. The output file's name will be of the format `input-name.out`\. 

## Creating a batch segment job \(AWS CLI\)<a name="batch-segment-cli"></a>

After you have completed [Preparing and importing batch input data](batch-data-upload.md), you are ready to create a batch segment job using the following `create-batch-segment-job` code\. Specify a job name, replace `Solution version ARN` with the Amazon Resource Name \(ARN\) of your solution version, and replace the `IAM service role ARN` with the ARN of the IAM service role you created for Amazon Personalize during set up\. This role must have read and write access to your input and output Amazon S3 buckets respectively\. For `num-results` specify the number of users you want Amazon Personalize to predict for each line of input data\. 

Replace `S3 input path` and `S3 output path` with the Amazon S3 path to your input file and output locations\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. You can apply a filter to the recommendations added to the output JSON file\. For more information see [Filtering batch recommendations and user segments](filter-batch.md)\. 

Use the following syntax for input and output locations: **s3://<name of your S3 bucket>/<folder name>/<input JSON file name>** and **s3://<name of your S3 bucket>/<output folder name>/**\. 

```
aws personalize create-batch-segment-job --job-name Job name \
                --solution-version-arn Solution version ARN \
                ----num-results The number of predicted users \
                --job-input s3DataSource={path=s3://S3 input path} \
                --job-output s3DataDestination={path=s3://S3 output path} \
                --role-arn IAM service role ARN
{
   "batchSegmentJobArn": "arn:aws:personalize:us-west-2:acct-id:batch-segment-job/batchSegmentJobName"
}
```

## Creating a batch segment job \(AWS SDKs\)<a name="batch-segment-sdk"></a>

After you have completed [Preparing and importing batch input data](batch-data-upload.md), you are ready to create a batch segment job with the `CreateBatchSegmentJob` operation\. The following code shows how to create a batch segment job using the AWS SDK for Python \(Boto3\) or AWS SDK for Java 2\.x\. 

Use the following syntax for input and output locations: **s3://<name of your S3 bucket>/<folder name>/<input JSON file name>** and **s3://<name of your S3 bucket>/<output folder name>/**\. 

------
#### [ SDK for Python \(Boto3\) ]

 Use the following `create_batch_segment_job` code to create a batch segment job\. Specify a `Job name`, replace `Solution version ARN` with the Amazon Resource Name \(ARN\) of your solution version, and replace the `IAM service role ARN` with the ARN of the IAM service role you created for Amazon Personalize during set up\. This role must have read and write access to your input and output Amazon S3 buckets respectively\. For `numResults` specify the number of users you want Amazon Personalize to predict for each line of input data\. 

Replace Amazon S3 data source and data destinations with the Amazon S3 path to your input file and output locations\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. You can apply a filter to the recommendations added to the output JSON file\. For more information see [Filtering batch recommendations and user segments](filter-batch.md)\. 

```
import boto3

personalize_rec = boto3.client(service_name='personalize')

personalize_rec.create_batch_segment_job (
    solutionVersionArn = "Solution version ARN",
    jobName = "Job name",
    numResults = Number of predicted users,
    roleArn = "IAM service role ARN",
    jobInput = 
       {"s3DataSource": {"path": "s3://<name of your S3 bucket>/<folder name>/<input JSON file name>"}},
    jobOutput = 
       {"s3DataDestination": {"path": "s3://<name of your S3 bucket>/<output folder name>/"}}
)
```

------
#### [ SDK for Java 2\.x ]

 Use the following `createBatchSegmentJob` method to create a batch segment job\. Pass the following as parameters: an Amazon Personalize service client, the solution version's ARN \(Amazon Resource Name\), a name for the job, the number of predicted users you want for each line of input data \(numResults\), the Amazon S3 location where you stored your input data \(s3InputDataSourcePath\), the `bucket-name/folder name` of your output data location \(s3DataDestinationPath\), and your service\-linked role's ARN \(see [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\)\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. 

```
public static String createBatchSegmentJob(PersonalizeClient personalizeClient,
                                                        String solutionVersionArn,
                                                        String jobName,
                                                        int numResults,
                                                        String s3InputDataSourcePath,
                                                        String s3DataDestinationPath,
                                                        String roleArn,
                                                        String explorationWeight,
                                                        String explorationItemAgeCutOff) {

    long waitInMilliseconds = 60 * 1000;
    String status;
    String batchSegmentJobArn;

    try {
        // Set up data input and output parameters.
        S3DataConfig inputSource = S3DataConfig.builder()
                .path(s3InputDataSourcePath)
                .build();
        S3DataConfig outputDestination = S3DataConfig.builder()
                .path(s3DataDestinationPath)
                .build();

        BatchSegmentJobInput jobInput = BatchSegmentJobInput.builder()
                .s3DataSource(inputSource)
                .build();
        BatchSegmentJobOutput jobOutputLocation = BatchSegmentJobOutput.builder()
                .s3DataDestination(outputDestination)
                .build();


        CreateBatchSegmentJobRequest createBatchSegmentJobRequest = CreateBatchSegmentJobRequest.builder()
                .solutionVersionArn(solutionVersionArn)
                .jobInput(jobInput)
                .jobOutput(jobOutputLocation)
                .jobName(jobName)
                .numResults(numResults)
                .roleArn(roleArn)
                .build();

        batchSegmentJobArn = personalizeClient.createBatchSegmentJob(createBatchSegmentJobRequest)
                .batchSegmentJobArn();
        DescribeBatchSegmentJobRequest describeBatchSegmentJobRequest = DescribeBatchSegmentJobRequest.builder()
                .batchSegmentJobArn(batchSegmentJobArn)
                .build();

        long maxTime = Instant.now().getEpochSecond() + 3 * 60 * 60;

        // wait until the batch segment job is complete.
        while (Instant.now().getEpochSecond() < maxTime) {

            BatchSegmentJob batchSegmentJob = personalizeClient
                    .describeBatchSegmentJob(describeBatchSegmentJobRequest)
                    .batchSegmentJob();

            status = batchSegmentJob.status();
            System.out.println("batch segment job status: " + status);

            if (status.equals("ACTIVE") || status.equals("CREATE FAILED")) {
                break;
            }
            try {
                Thread.sleep(waitInMilliseconds);
            } catch (InterruptedException e) {
                System.out.println(e.getMessage());
            }
        }
        return batchSegmentJobArn;

    } catch (PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
    return "";
}
```

------

Processing the batch job might take a while to complete\. You can check a job's status by calling `DescribeBatchSegmentJob` and passing a `batchSegmentJobArn` as the input parameter\. You can also list all Amazon Personalize batch segment jobs in your AWS environment by calling `ListBatchSegmentJobs`\. 