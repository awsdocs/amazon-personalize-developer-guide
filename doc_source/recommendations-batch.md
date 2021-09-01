# Getting batch recommendations<a name="recommendations-batch"></a>

 To get recommendations for large datasets that do not require real\-time updates, you can use an asynchronous batch workflow with historical data only\. For example, you might get product recommendations for all users on an email list, or get [item\-to\-item similarities \(SIMS\)](native-recipe-sims.md) across an inventory\. 

 To get batch recommendations with Amazon Personalize, you use a batch inference job\. A *batch inference job* is a tool that imports your batch input data from an Amazon S3 bucket, uses your solution version to generate recommendations, and exports the recommendations to an Amazon S3 bucket\. We recommend that you use a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. To use data you record in real time with the PutEvents API operation, you must retrain your solution version before creating the batch inference job\. 

The batch workflow is as follows:

1.  Prepare and upload a list of users or items \(or both\) in JSON format to an Amazon S3 bucket\. The format of your input data depends on the recipe you use\. See [Preparing and importing batch input data](#batch-data-upload)\. 

1.  Create a separate location for your output data, either a folder or a different Amazon S3 bucket\. 

1.  Create a batch inference job\. See [Creating a batch inference job \(console\)](#batch-console), [Creating a batch inference job \(AWS CLI\)](#batch-cli), or [Creating a batch inference job \(AWS SDKs\)](#batch-sdk)\. 

1.  When the batch inference job is complete, retrieve the recommendations from your output location in Amazon S3\. 

**Topics**
+ [Batch workflow permissions requirements](#batch-permissions-req)
+ [Batch workflow scoring](#batch-scoring)
+ [Preparing and importing batch input data](#batch-data-upload)
+ [Creating a batch inference job \(console\)](#batch-console)
+ [Creating a batch inference job \(AWS CLI\)](#batch-cli)
+ [Creating a batch inference job \(AWS SDKs\)](#batch-sdk)

## Batch workflow permissions requirements<a name="batch-permissions-req"></a>

 For batch workflows, your Amazon Personalize IAM service role needs permission to access and add files to your Amazon S3 buckets\. For information on granting permissions, see [Service\-linked role policy for batch workflows](granting-personalize-s3-access.md#role-policy-for-batch-workflows)\. For more information on bucket permissions, see [User policy examples](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-policies-s3.html) in the *Amazon Simple Storage Service \(S3\) Developer Guide*\. 

 Amazon S3 buckets and objects must be either encryption free or, if you are using AWS Key Management Service \(AWS KMS\) for encryption, you must give your IAM user and Amazon Personalize service\-linked role permission to use your key\. You must also add Amazon Personalize as a Principle in your AWS KMS key policy\. For more information, see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.

## Batch workflow scoring<a name="batch-scoring"></a>

Item scores calculated by batch inference jobs are calculated just as described in [Getting real\-time recommendations](getting-real-time-recommendations.md)\. You can view scores in the batch inference job's output JSON file\. Scores are only returned by models trained with the HRNN and Personalize\-Ranking recipes\.

## Preparing and importing batch input data<a name="batch-data-upload"></a>

 A batch inference job uses a solution version to make recommendations based on the users or items \(or both\) that you provide in an input JSON file\. Before you can get batch recommendations, you must prepare and upload your JSON file to an Amazon S3 bucket\. We recommend that you create an output folder in your Amazon S3 bucket or use a separate output Amazon S3 bucket\. This allows you to run multiple batch inference jobs using the same input data location\. 

**To prepare and import data**

1. Format your batch input data depending on the recipe your solution uses\. Your input data is a JSON file with either a list of userIds \(USER\_PERSONALIZATION recipes\), a list of itemIds \(RELATED\_ITEMS recipes\), or both \(PERSONALIZED\_RANKING recipes\)\. Separate each user, item, or user and list of items with a new line\. For examples, see [Input and output JSON examples](#batch-recommendations-json-examples)\.

1.  Upload your input JSON to an input folder in your Amazon S3 bucket\. For more information, see [Uploading files and folders by using drag and drop](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) in the Amazon Simple Storage Service Console User Guide\. 

1.  Create a separate location for your output data, either a folder or a different Amazon S3 bucket\. Creating a separate location for the output JSON allows you to run multiple batch inference jobs using the same input data location\.

### Input and output JSON examples<a name="batch-recommendations-json-examples"></a>

How you format your input data depends on the recipe you use\. The following are correctly formatted JSON input and output examples for each recipe type\.

**User\-Personalization and legacy HRNN recipes**

------
#### [ Input ]

Separate each `userId` with a new line as follows\.

```
{"userId": "4638"}
{"userId": "663"}
{"userId": "3384"}
...
```

------
#### [ Output ]

```
{"input":{"userId":"4638"},"output":{"recommendedItems":["63992","115149","110102","148626","148888","31685","102445","69526","92535","143355","62374","7451","56171","122882","66097","91542","142488","139385","40583","71530","39292","111360","34048","47099","135137"],"scores":[0.0152238,0.0069081,0.0068222,0.006394,0.0059746,0.0055851,0.0049357,0.0044644,0.0042968,0.004015,0.0038805,0.0037476,0.0036563,0.0036178,0.00341,0.0033467,0.0033258,0.0032454,0.0032076,0.0031996,0.0029558,0.0029021,0.0029007,0.0028837,0.0028316]},"error":null}
{"input":{"userId":"663"},"output":{"recommendedItems":["368","377","25","780","1610","648","1270","6","165","1196","1097","300","1183","608","104","474","736","293","141","2987","1265","2716","223","733","2028"],"scores":[0.0406197,0.0372557,0.0254077,0.0151975,0.014991,0.0127175,0.0124547,0.0116712,0.0091098,0.0085492,0.0079035,0.0078995,0.0075598,0.0074876,0.0072006,0.0071775,0.0068923,0.0066552,0.0066232,0.0062504,0.0062386,0.0061121,0.0060942,0.0060781,0.0059263]},"error":null}
{"input":{"userId":"3384"},"output":{"recommendedItems":["597","21","223","2144","208","2424","594","595","920","104","520","367","2081","39","1035","2054","160","1370","48","1092","158","2671","500","474","1907"],"scores":[0.0241061,0.0119394,0.0118012,0.010662,0.0086972,0.0079428,0.0073218,0.0071438,0.0069602,0.0056961,0.0055999,0.005577,0.0054387,0.0051787,0.0051412,0.0050493,0.0047126,0.0045393,0.0042159,0.0042098,0.004205,0.0042029,0.0040778,0.0038897,0.0038809]},"error":null}
...
```

------

**Popularity\-Count**

------
#### [ Input ]

Separate each `userId` with a new line as follows\.

```
{"userId": "12"}
{"userId": "105"}
{"userId": "41"}
...
```

------
#### [ Output ]

```
{"input": {"userId": "12"}, "output": {"recommendedItems": ["105", "106", "441"]}}
{"input": {"userId": "105"}, "output": {"recommendedItems": ["105", "106", "441"]}}
{"input": {"userId": "41"}, "output": {"recommendedItems": ["105", "106", "441"]}}
...
```

------

**Personalize\-Ranking**

------
#### [ Input ]

Separate each `userId` and list of `itemIds` to be ranked with a new line as follows\.

```
{"userId": "891", "itemList": ["27", "886", "101"]}
{"userId": "445", "itemList": ["527", "55", "901"]}
{"userId": "71", "itemList": ["27", "351", "101"]}
...
```

------
#### [ Output ]

```
{"input":{"userId":"891","itemList":["27","886","101"]},"output":{"recommendedItems":["27","101","886"],"scores":[0.48421,0.28133,0.23446]}}
{"input":{"userId":"445","itemList":["527","55","901"]},"output":{"recommendedItems":["901","527","55"],"scores":[0.46972,0.31011,0.22017]}}
{"input":{"userId":"71","itemList":["29","351","199"]},"output":{"recommendedItems":["351","29","199"],"scores":[0.68937,0.24829,0.06232]}}
...
```

------

**SIMS**

------
#### [ Input ]

Separate each `itemId` with a new line as follows\.

```
{"itemId": "105"}
{"itemId": "106"}
{"itemId": "441"}
...
```

------
#### [ Output ]

```
{"input": {"itemId": "105"}, "output": {"recommendedItems": ["106", "107", "49"]}}
{"input": {"itemId": "106"}, "output": {"recommendedItems": ["105", "107", "49"]}}
{"input": {"itemId": "441"}, "output": {"recommendedItems": ["2", "442", "435"]}}
...
```

------

## Creating a batch inference job \(console\)<a name="batch-console"></a>

 After you have completed [Preparing and importing batch input data](#batch-data-upload), you are ready to create a batch inference job\. This procedure assumes that you have already created a solution and a solution version \(trained model\)\. To get batch recommendations, create a batch inference job and specify the job name, 

**To get batch recommendations \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. On the **Datasets groups** page, choose your dataset group\.

1. Choose **Batch inference jobs** in the navigation pane, then choose **Create batch inference job**\.

1. In **Batch inference job details**, in **Batch inference job name**, specify a name for your batch inference job\.

1. For **IAM service role**, choose the IAM service role you created for Amazon Personalize during set up\. This role must have read and write access to your input and output Amazon S3 buckets respectively\.

1. For **Solution**, choose the solution and then choose the **Solution version ID** that you want to use to generate the recommendations\. 

1.  For **Input data configuration**, specify the Amazon S3 path to your input file\. 

   Use the following syntax: **s3://<name of your S3 bucket>/<folder name>/<input JSON file name>**

    Your input data must be in the correct format for the recipe your solution uses\. For input data examples see [Input and output JSON examples](#batch-recommendations-json-examples)\. 

1. For **Output data configuration**, specify the path to your output location\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\.

    Use the following syntax: **s3://<name of your S3 bucket>/<output folder name>/** 

1.  For **Filter configuration** optionally choose a filter to apply a filter to the recommendations added to the output JSON file\. For more information see [Filtering batch recommendations](filter-batch.md)\. 

1.  Choose **Create batch inference job**\. Batch inference job creation starts and the **Batch inference jobs** page appears with the **Batch inference job detail** section displayed\.

1.  When the batch inference job's status changes to **Active**, you can retrieve the job's output from the designated output Amazon S3 bucket\. The output file's name will be of the format `input-name.out`\. 

## Creating a batch inference job \(AWS CLI\)<a name="batch-cli"></a>

After you have completed [Preparing and importing batch input data](#batch-data-upload), you are ready to create a batch inference job using the following `create-batch-inference-job` code\. Specify a job name, replace `Solution version ARN` with the Amazon Resource Name \(ARN\) of your solution version, and replace the `IAM service role ARN` with the ARN of the IAM service role you created for Amazon Personalize during set up\. This role must have read and write access to your input and output Amazon S3 buckets respectively\. 

Replace `S3 input path` and `S3 output path` with the Amazon S3 path to your input file and output locations\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. You can apply a filter to the recommendations added to the output JSON file\. For more information see [Filtering batch recommendations](filter-batch.md)\. 

Use the following syntax for input and output locations: **s3://<name of your S3 bucket>/<folder name>/<input JSON file name>** and **s3://<name of your S3 bucket>/<output folder name>/**\. 

The example includes optional User\-Personalization recipe specific `itemExplorationConfig` hyperparameters: `explorationWeight` and `explorationItemAgeCutOff`\. Optionally include `explorationWeight` and `explorationItemAgeCutOff` values to configure exploration\. For more information, see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

```
aws personalize create-batch-inference-job --job-name Batch job name \
                --solution-version-arn Solution version ARN \
                --job-input s3DataSource={path=s3://S3 input path} \
                --job-output s3DataDestination={path=s3://S3 output path} \
                --role-arn IAM service role ARN \
                --batch-inference-job-config "{\"itemExplorationConfig\":{\"explorationWeight\":\"0.3\",\"explorationItemAgeCutOff\":\"30\"}}"
{
   "batchInferenceJobArn": "arn:aws:personalize:us-west-2:acct-id:batch-inference-job/batchInferenceJobName"
}
```

## Creating a batch inference job \(AWS SDKs\)<a name="batch-sdk"></a>

After you have completed [Preparing and importing batch input data](#batch-data-upload), you are ready to create a batch inference job with the [ CreateBatchInferenceJob ](API_CreateBatchInferenceJob.md) operation\. The following code shows how to create a batch inference job using the AWS SDK for Python \(Boto3\) or AWS SDK for Java 2\.x\. 

Use the following syntax for input and output locations: **s3://<name of your S3 bucket>/<folder name>/<input JSON file name>** and **s3://<name of your S3 bucket>/<output folder name>/**\. 

------
#### [ SDK for Python \(Boto3\) ]

 Use the following `create_batch_inference_job` code to create a batch inference job\. Specify a `Batch job name`, replace `Solution version ARN` with the Amazon Resource Name \(ARN\) of your solution version, and replace the `IAM service role ARN` with the ARN of the IAM service role you created for Amazon Personalize during set up\. T his role must have read and write access to your input and output Amazon S3 buckets respectively\. 

Replace Amazon S3 data source and data destinations with the Amazon S3 path to your input file and output locations\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. You can apply a filter to the recommendations added to the output JSON file\. For more information see [Filtering batch recommendations](filter-batch.md)\. 

The example includes optional User\-Personalization recipe specific `itemExplorationConfig` hyperparameters: `explorationWeight` and `explorationItemAgeCutOff`\. Optionally include `explorationWeight` and `explorationItemAgeCutOff` values to configure exploration\. For more information, see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

```
import boto3

personalize_rec = boto3.client(service_name='personalize')

personalize_rec.create_batch_inference_job (
    solutionVersionArn = "Solution version ARN",
    jobName = "Batch job name",
    roleArn = "IAM service role ARN",
    batchInferenceJobConfig = {
        # optional USER_PERSONALIZATION recipe hyperparameters
        "itemExplorationConfig": {      
            "explorationWeight": "0.3",
            "explorationItemAgeCutOff": "30"
        }
    },
    jobInput = 
       {"s3DataSource": {"path": "s3://<name of your S3 bucket>/<folder name>/<input JSON file name>"}},
    jobOutput = 
       {"s3DataDestination": {"path": "s3://<name of your S3 bucket>/<output folder name>/"}}
)
```

------
#### [ SDK for Java 2\.x ]

 Use the following `createPersonalizeBatchInferenceJob` method to create a batch inference job\. Pass the following as parameters: an Amazon Personalize service client, the solution version's ARN \(Amazon Resource Name\), a name for the job, the Amazon S3 location where you stored your input data \(s3InputDataSourcePath\), the `bucket-name/folder name` of your output data location \(s3DataDestinationPath\), and your service\-linked role's ARN \(see [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\)\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. 

The example includes optional User\-Personalization recipe specific `itemExplorationConfig` fields: `explorationWeight` and `explorationItemAgeCutOff`\. Optionally include `explorationWeight` and `explorationItemAgeCutOff` values to configure exploration\. For more information, see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

```
public static String createPersonalizeBatchInferenceJob(PersonalizeClient personalizeClient,
                                                        String solutionVersionArn,
                                                        String jobName,
                                                        String s3InputDataSourcePath,
                                                        String s3DataDestinationPath,
                                                        String roleArn,
                                                        String explorationWeight,
                                                        String explorationItemAgeCutOff) {

    long waitInMilliseconds = 60 * 1000;
    String status;
    String batchInferenceJobArn;

    try {
        // Set up data input and output parameters.
        S3DataConfig inputSource = S3DataConfig.builder()
                .path(s3InputDataSourcePath)
                .build();
        S3DataConfig outputDestination = S3DataConfig.builder()
                .path(s3DataDestinationPath)
                .build();

        BatchInferenceJobInput jobInput = BatchInferenceJobInput.builder()
                .s3DataSource(inputSource)
                .build();
        BatchInferenceJobOutput jobOutputLocation = BatchInferenceJobOutput.builder()
                .s3DataDestination(outputDestination)
                .build();

        // Optional code to build the User-Personalization specific item exploration config.
        HashMap<String, String> explorationConfig = new HashMap<>();

        explorationConfig.put("explorationWeight", explorationWeight);
        explorationConfig.put("explorationItemAgeCutOff", explorationItemAgeCutOff);

        BatchInferenceJobConfig jobConfig = BatchInferenceJobConfig.builder()
                .itemExplorationConfig(explorationConfig)
                .build();
        // End optional User-Personalization recipe specific code.

        CreateBatchInferenceJobRequest createBatchInferenceJobRequest = CreateBatchInferenceJobRequest.builder()
                .solutionVersionArn(solutionVersionArn)
                .jobInput(jobInput)
                .jobOutput(jobOutputLocation)
                .jobName(jobName)
                .roleArn(roleArn)
                .batchInferenceJobConfig(jobConfig)   // Optional
                .build();

        batchInferenceJobArn = personalizeClient.createBatchInferenceJob(createBatchInferenceJobRequest)
                .batchInferenceJobArn();
        DescribeBatchInferenceJobRequest describeBatchInferenceJobRequest = DescribeBatchInferenceJobRequest.builder()
                .batchInferenceJobArn(batchInferenceJobArn)
                .build();

        long maxTime = Instant.now().getEpochSecond() + 3 * 60 * 60;

        // wait until the batch inference job is complete.
        while (Instant.now().getEpochSecond() < maxTime) {

            BatchInferenceJob batchInferenceJob = personalizeClient
                    .describeBatchInferenceJob(describeBatchInferenceJobRequest)
                    .batchInferenceJob();

            status = batchInferenceJob.status();
            System.out.println("Batch inference job status: " + status);

            if (status.equals("ACTIVE") || status.equals("CREATE FAILED")) {
                break;
            }
            try {
                Thread.sleep(waitInMilliseconds);
            } catch (InterruptedException e) {
                System.out.println(e.getMessage());
            }
        }
        return batchInferenceJobArn;

    } catch (PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
    return "";
}
```

------

Processing the batch job might take a while to complete\. You can check a job's status by calling [ DescribeBatchInferenceJob ](API_DescribeBatchInferenceJob.md) and passing a `batchRecommendationsJobArn` as the input parameter\. You can also list all Amazon Personalize batch inference jobs in your AWS environment by calling [ ListBatchInferenceJobs ](API_ListBatchInferenceJobs.md)\.