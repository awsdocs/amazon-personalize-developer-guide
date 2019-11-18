# Getting Recommendations<a name="getting-recommendations"></a>

Amazon Personalize can supply recommendations and personalized rankings from a campaign\. For example, suppose you have a campaign that is designed to give movie recommendations\. You can use the following operations to give movie recommendations to users signed into your application or website\. For an example using the AWS CLI, see [Step 4: Get Recommendations](getting-started-cli.md#gs-test)\.

**Topics**
+ [Get Recommendations](#recommendations)
+ [Get Personalized Rankings](#rankings)
+ [Get Batch Recommendations](#recommendations-batch)

## Get Recommendations<a name="recommendations"></a>

After you have created a campaign, you can use it in your applications to get recommendations\.

To get recommendations, call the [GetRecommendations](API_RS_GetRecommendations.md) API\. Supply either the user ID or item ID, dependent on the recipe type used to create the solution the campaign is based on\.

**Note**  
The solution backing the campaign must have been created using a recipe of type USER\_PERSONALIZATION or RELATED\_ITEMS\. For more information, see [Using Predefined Recipes](working-with-predefined-recipes.md)\.

**Get therecommendations using the AWS Python SDK**

Use the following code to get a recommendation\. Change the value of `userId` to a user ID that is in the data you used to train the solution\. A list of recommended items for the user is displayed\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_recommendations(
    campaignArn = "Campaign ARN",
    userId = 'User ID')

print("Recommended items")
for item in response['itemList']:
    print (item['itemId'])
```

## Get Personalized Rankings<a name="rankings"></a>

A personalized ranking is a list of recommended items that are re\-ranked for a specific user\. To get personalized rankings, call the [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) API\.

**Note**  
The solution backing the campaign must have been created using a recipe of type PERSONALIZED\_RANKING\. For more information, see [Using Predefined Recipes](working-with-predefined-recipes.md)\.

**Get personalized rankings using the AWS Python SDK**

Use the following code to get a personalized ranking\. Change the value of `userId` and `inputList` to a user ID and list of item IDs that are in the data you used to train the solution\. A list of ranked recommendations is displayed\. The first item in the list is considered by Amazon Personalize to be of most interest to the user\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_personalized_ranking(
    campaignArn = "Campaign arn",
    userId = 'UserID',
    inputList = ['ItemID1','ItemID2'])

print("Personalized Ranking")
for item in response['personalizedRanking']:
    print (item['itemId'])
```

## Get Batch Recommendations<a name="recommendations-batch"></a>

You can also use a batch workflow to get recommendations from large datasets that do not require real\-time updates\. For instance, you might create a batch inference job to get product recommendations for all users on an email list, or to get [item\-to\-item similarities \(SIMS\)](native-recipe-sims.md) across an inventory\. To get batch recommendations, you can create a batch inference job by calling [CreateBatchInferenceJob](API_CreateBatchInferenceJob.md)\.

The [CreateBatchInferenceJob](API_CreateBatchInferenceJob.md) uses a chosen solution version to make recommendations based on data provided in an input JSON file\. The result is then returned as a JSON file to an Amazon S3 bucket\. The following tab list contains correctly formatted JSON input and output excerpts for each recipe type\.

**HRNN**

------
#### [ Input ]

```
{"userId": "4638"}
{"userId": "663"}
{"userId": "3384"}
...
```

------
#### [ Output ]

```
{"input":{"userId":"4638"}, "output": {"recommendedItems": ["296", "1", "260", "318"]}}
{"input":{"userId":"663"}, "output": {"recommendedItems": ["1393", "3793", "2701", "3826"]}}
{"input":{"userId":"3384"}, "output": {"recommendedItems": ["8368", "5989", "40815", "48780"]}}
...
```

------

**Popularity\-Count**

------
#### [ Input ]

```
{}
{"itemId": "105"}
{"itemId": "41"}
...
```

------
#### [ Output ]

```
{"input": {}, "output": {"recommendedItems": ["105", "106", "441"]}}
{"input": {"itemId": "105"}, "output": {"recommendedItems": ["105", "106", "441"]}}
{"input": {"itemId": "41"}, "output": {"recommendedItems": ["105", "106", "441"]}}
...
```

------

**Personalize\-Ranking**

------
#### [ Input ]

```
{"userId": "891", "itemIds": ["27", "886", "101"]}
{"userId": "445", "itemIds": ["527", "55", "901"]}
{"userId": "71", "itemIds": ["27", "351", "101"]}
...
```

------
#### [ Output ]

```
{"input": {"userId": "891", "itemIds": ["27", "886", "101"]}, "output": {"recommendedItems": ["28", "887", "102"]}}
{"input": {"userId": "445", "itemIds": ["527", "55", "901"]}, "output": {"recommendedItems": ["631", "56", "877"]}}
{"input": {"userId": "71", "itemIds": ["27", "351", "101"]}, "output": {"recommendedItems": ["25", "347", "105"]}}
...
```

------

**SIMS**

------
#### [ Input ]

```
{"itemId": "105"}
{"itemId": "106"}
{"items": "441"}
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

In order to get batch recommendations, the IAM user role that invokes the [CreateBatchInferenceJob](API_CreateBatchInferenceJob.md) operation must have read and write permissions to your input and output Amazon S3 buckets respectively\. For more information on bucket permissions, see [User Policy Examples](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-policies-s3.html) in the *Amazon Simple Storage Service \(S3\) Developer Guide*\.

You can perform batch inference operations with any of the following tools:
+ [Amazon Personalize console](#batch-console)
+ [AWS CLI](#batch-cli)
+ [AWS SDK](#batch-sdk)

### Getting Batch Recommendations \(Amazon Personalize Console\)<a name="batch-console"></a>

The following procedure outlines the batch inference workflow using the Amazon Personalize console\. This procedure assumes that you have already created a solution that is properly formatted to perform the desired batch job on your dataset\.

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. Choose **Batch inference jobs** in the navagation pane, then choose **Create batch inference job**\.

1. In **Batch inference job details**, in **Batch inference job name**, specify a name for your batch inference job\.

1. For **IAM service role**, choose the Amazon Personalize IAM service role that has read and write access to your input and output Amazon S3 buckets respectively\.

1. For **Solution**, choose the solution that you want to use to generate the recommendations The solution recipe must match the input data's format\.

1. In **Input data configuration**, specify the Amazon S3 path to your input file\. In **Output data configuration**, specify the path to your output Amazon S3 bucket\.

1. Choose **Create batch inference job**\. Batch inference job creation starts and the **Batch inference jobs** page appears with the **Batch inference job detail** section displayed\.

   Your screen should look similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/batch-job-result.png)
**Note**  
Creating a batch inference job takes time\.

1. When the batch inference job's status changes to **Active**, you can retreive the job's output from the designated output Amazon S3 bucket\. The output file's name will be of the format `input-name.out`\.

### Getting Batch Recommendations \(AWS CLI\)<a name="batch-cli"></a>

The following is an example of a batch inference workflow using the AWS CLI\. A JSON file called `batch.json` is passed as input, and the output file, `batch.json.out`, is returned to an Amazon S3 bucket\. If successful, the operation returns the ARN of the batch inference job\.

```
aws personalize create-batch-inference-job --job-name batchTest \
                --solution-version-arn arn:aws:personalize:us-west-2:000000000000:solution/batch-test-solution-version/1234abcd \
                --jobInput s3DataSource="s3://personalize/batch/input/batch-test-input.json" \
                --jobOutput s3DataSource="s3://personalize/batch/output/" \
                --role-arn arn:aws:iam::01234567891011:role/import-export-role
      
{
   "batchInferenceJobArn": "arn:aws:personalize:us-west-2:01234567891011:batch-inference-job/batch-test"
}
```

Once a batch inference job is created, you can inspect it further with the [DescribeBatchInferenceJob](API_DescribeBatchInferenceJob.md)operation\.

```
aws personalize describe-batch-inference-job --batch-inference-job-arn arn:aws:personalize:us-west-2:01234567891011:batch-inference-job/batch-test

{
  "jobName": "batchTest",
  "batchInferenceJobArn": "arn:aws:personalize:us-west-2:01234567891011:batch-inference-job/batch-test",
  "solutionVersionArn": "arn:aws:personalize:us-west-2:000000000000:solution/batch-test-solution-version/1234abcd",
  "jobInput": { 
      "s3InputPath": "s3://personalize/batch/input/batch.json"
  },
  "jobOutput": { 
      "s3OutputPath": "s3://personalize/batch/output/" 
  },
  "roleArn": "arn:aws:iam::01234567891011:role/import-export-role",
  "status": "ACTIVE",
  "creationDateTime": 1542392161.837,
  "lastUpdateDateTime: 1542393013.377
}
```

### Getting Batch Recommendations \(AWS Python SDK\)<a name="batch-sdk"></a>

Use the following code to get batch recommendations using the AWS Python SDK\. The operation reads an input JSON file from an Amazon S3 bucket and places an output JSON file \(`input-file-name.out`\) in an Amazon S3 bucket\.

 The first item in the response file is considered by Amazon Personalize to be of most interest to the user\.

```
personalize_rec = boto3.client(service_name='personalize-rec')
personalize_rec.create_batch_inference_job (
solutionVerisonArn = "Solution version ARN",
jobName = "Batch job name",
roleArn = "IAM role ARN
jobInput = 
 {"S3InputPath": "S3 path"},
jobOutput = 
 {"S3OutputPath": "S3 path"}
)
```

The command returns the ARN for the batch job \(the `batchRecommendationsJobArn`\)\.

Processing the batch job might take a while to complete\. You can check a job's status by calling [DescribeBatchInferenceJob](API_DescribeBatchInferenceJob.md) and passing a `batchRecommendationsJobArn` as the input parameter\. You can also list all Amazon Personalize batch inference jobs in your AWS environment by calling [ListBatchInferenceJobs](API_ListBatchInferenceJobs.md)\.