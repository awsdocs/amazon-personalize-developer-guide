# Creating a batch inference job<a name="creating-batch-inference-job"></a>

 Create a batch inference job to get batch item recommendations for users based on input data from Amazon S3\. The input data can be a list of users or items \(or both\) in JSON format\. You can create a batch inference job with the Amazon Personalize console, the AWS Command Line Interface \(AWS CLI\), or AWS SDKs\. 

 For more information about the batch workflow in Amazon Personalize, including permissions requirements, recommendation scoring, and preparing and importing input data, see [Getting batch recommendations and user segments](recommendations-batch.md)\. 

**Topics**
+ [Creating a batch inference job \(console\)](#batch-console)
+ [Creating a batch inference job \(AWS CLI\)](#batch-cli)
+ [Creating a batch inference job \(AWS SDKs\)](#batch-sdk)

## Creating a batch inference job \(console\)<a name="batch-console"></a>

 After you have completed [Preparing and importing batch input data](batch-data-upload.md), you are ready to create a batch inference job\. This procedure assumes that you have already created a solution and a solution version \(trained model\)\. 

**To create a batch inference job \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. On the **Dataset groups** page, choose your dataset group\.

1. Choose **Batch inference jobs** in the navigation pane, then choose **Create batch inference job**\.

1. In **Batch inference job details**, in **Batch inference job name**, specify a name for your batch inference job\.

1. For **IAM service role**, choose the IAM service role you created for Amazon Personalize during set up\. This role must have read and write access to your input and output Amazon S3 buckets respectively\.

1. For **Solution**, choose the solution and then choose the **Solution version ID** that you want to use to generate the recommendations\. 

1. For **Number of results**, optionally specify the number of recommendations for each line of input data\. The default is 25\.

1.  For **Input data configuration**, specify the Amazon S3 path to your input file\. 

   Use the following syntax: **s3://<name of your S3 bucket>/<folder name>/<input JSON file name>**

    Your input data must be in the correct format for the recipe your solution uses\. For input data examples see [Input and output JSON examples](batch-data-upload.md#batch-recommendations-json-examples)\. 

1. For **Output data configuration**, specify the path to your output location\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\.

    Use the following syntax: **s3://<name of your S3 bucket>/<output folder name>/** 

1.  For **Filter configuration** optionally choose a filter to apply a filter to the batch recommendations\. If your filter uses placeholder parameters, make sure the values for the parameters are included in your input JSON\. For more information see [Providing filter values in your input JSON](filter-batch.md#providing-filter-values)\. 

1. For **Tags**, optionally add any tags\. For more information about tagging Amazon Personalize resources, see [Tagging Amazon Personalize resources](tagging-resources.md)\.

1.  Choose **Create batch inference job**\. Batch inference job creation starts and the **Batch inference jobs** page appears with the **Batch inference job detail** section displayed\.

1.  When the batch inference job's status changes to **Active**, you can retrieve the job's output from the designated output Amazon S3 bucket\. The output file's name will be of the format `input-name.out`\. 

## Creating a batch inference job \(AWS CLI\)<a name="batch-cli"></a>

After you have completed [Preparing and importing batch input data](batch-data-upload.md), you are ready to create a batch inference job using the following `create-batch-inference-job` code\. Specify a job name, replace `Solution version ARN` with the Amazon Resource Name \(ARN\) of your solution version, and replace the `IAM service role ARN` with the ARN of the IAM service role you created for Amazon Personalize during set up\. This role must have read and write access to your input and output Amazon S3 buckets respectively\. Optionally provide a filter ARN to filter recommendations\. If your filter uses placeholder parameters, make sure the values for the parameters are included in your input JSON\. For more information see [Filtering batch recommendations and user segments](filter-batch.md)\. 

Replace `S3 input path` and `S3 output path` with the Amazon S3 path to your input file and output locations\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. Use the following syntax for input and output locations: **s3://<name of your S3 bucket>/<folder name>/<input JSON file name>** and **s3://<name of your S3 bucket>/<output folder name>/**\. 

The example includes optional User\-Personalization recipe specific `itemExplorationConfig` hyperparameters: `explorationWeight` and `explorationItemAgeCutOff`\. Optionally include `explorationWeight` and `explorationItemAgeCutOff` values to configure exploration\. For more information, see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

```
aws personalize create-batch-inference-job \
                --job-name Batch job name \
                --solution-version-arn Solution version ARN \
                --filter-arn Filter ARN \
                --job-input s3DataSource={path=s3://S3 input path} \
                --job-output s3DataDestination={path=s3://S3 output path} \
                --role-arn IAM service role ARN \
                --batch-inference-job-config "{\"itemExplorationConfig\":{\"explorationWeight\":\"0.3\",\"explorationItemAgeCutOff\":\"30\"}}"
{
   "batchInferenceJobArn": "arn:aws:personalize:us-west-2:acct-id:batch-inference-job/batchInferenceJobName"
}
```

## Creating a batch inference job \(AWS SDKs\)<a name="batch-sdk"></a>

After you have completed [Preparing and importing batch input data](batch-data-upload.md), you are ready to create a batch inference job with the [CreateBatchInferenceJob](API_CreateBatchInferenceJob.md) operation\. 

 The following code shows how to create a batch inference job\. Specify a job name, the Amazon Resource Name \(ARN\) of your solution version, and the ARN of the IAM service role you created for Amazon Personalize during set up\. This role must have read and write access to your input and output Amazon S3 buckets\.

We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. Use the following syntax for input and output locations: **s3://<name of your S3 bucket>/<folder name>/<input JSON file name>** and **s3://<name of your S3 bucket>/<output folder name>/**\.

 For `numResults`, specify the number of items you want Amazon Personalize to predict for each line of input data\. Optionally provide a filter ARN to filter recommendations\. If your filter uses placeholder parameters, make sure the values for the parameters are included in your input JSON\. For more information see [Filtering batch recommendations and user segments](filter-batch.md)\.

 

------
#### [ SDK for Python \(Boto3\) ]

The example includes optional User\-Personalization recipe specific `itemExplorationConfig` hyperparameters: `explorationWeight` and `explorationItemAgeCutOff`\. Optionally include `explorationWeight` and `explorationItemAgeCutOff` values to configure exploration\. For more information, see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

```
import boto3

personalize_rec = boto3.client(service_name='personalize')

personalize_rec.create_batch_inference_job (
    solutionVersionArn = "Solution version ARN",
    jobName = "Batch job name",
    roleArn = "IAM service role ARN",
    filterArn = "Filter ARN",
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

The example includes optional User\-Personalization recipe specific `itemExplorationConfig` fields: `explorationWeight` and `explorationItemAgeCutOff`\. Optionally include `explorationWeight` and `explorationItemAgeCutOff` values to configure exploration\. For more information, see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

```
public static String createPersonalizeBatchInferenceJob(PersonalizeClient personalizeClient,
                                                        String solutionVersionArn,
                                                        String jobName,
                                                        String filterArn,
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
              .filterArn(filterArn)
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
#### [ SDK for JavaScript v3 ]

```
// Get service clients module and commands using ES6 syntax.
import { CreateBatchInferenceJobCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the batch inference job's parameters.

export const createBatchInferenceJobParam = {
  jobName: 'JOB_NAME',
  jobInput: {         /* required */
    s3DataSource: {   /* required */
      path: 'INPUT_PATH', /* required */
      // kmsKeyArn: 'INPUT_KMS_KEY_ARN' /* optional */'
    }
  },
  jobOutput: {         /* required */
    s3DataDestination: {   /* required */
      path: 'OUTPUT_PATH', /* required */
      // kmsKeyArn: 'OUTPUT_KMS_KEY_ARN' /* optional */'
    }
  },
  roleArn: 'ROLE_ARN', /* required */
  solutionVersionArn: 'SOLUTION_VERSION_ARN', /* required */
  numResults: 20 /* optional integer*/
};

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateBatchInferenceJobCommand(createBatchInferenceJobParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

------

Processing the batch job might take a while to complete\. You can check a job's status by calling [DescribeBatchInferenceJob](API_DescribeBatchInferenceJob.md) and passing a `batchRecommendationsJobArn` as the input parameter\. You can also list all Amazon Personalize batch inference jobs in your AWS environment by calling [ListBatchInferenceJobs](API_ListBatchInferenceJobs.md)\.