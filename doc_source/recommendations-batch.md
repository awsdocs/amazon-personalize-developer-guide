# Getting batch recommendations<a name="recommendations-batch"></a>

 To get recommendations for large datasets that do not require real\-time updates, you can use an asynchronous batch workflow\. For example, you might get product recommendations for all users on an email list, or get [item\-to\-item similarities \(SIMS\)](native-recipe-sims.md) across an inventory\. 

 To get batch recommendations with Amazon Personalize, you use a batch inference job\. A *batch inference job* is a tool that imports your batch input data from an Amazon S3 bucket, uses your solution version to generate recommendations, and exports the recommendations to an Amazon S3 bucket\. 

The batch workflow is as follows:

1.  Prepare and upload a list of users or items \(or both\) in JSON format to an Amazon S3 bucket\. The format of your input data depends on the recipe you use\. See [Preparing and importing batch input data](#batch-data-upload)\. 

1.  Create a separate location for your output data, either a folder or a different Amazon S3 bucket\. 

1.  Create a batch inference job\. See [Creating a batch inference job \(console\)](#batch-console) or [Creating a batch inference job \(AWS CLI and AWS SDKs\)](#batch-sdk-cli)\. 

1.  When the batch inference job is complete, retrieve the recommendations from your output location in Amazon S3\. 

**Topics**
+ [Batch workflow permissions requirements](#batch-permissions-req)
+ [Batch workflow scoring](#batch-scoring)
+ [Preparing and importing batch input data](#batch-data-upload)
+ [Creating a batch inference job \(console\)](#batch-console)
+ [Creating a batch inference job \(AWS CLI and AWS SDKs\)](#batch-sdk-cli)

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

1. Choose **Batch inference jobs** in the navigation pane, then choose **Create batch inference job**\.

1. In **Batch inference job details**, in **Batch inference job name**, specify a name for your batch inference job\.

1. For **IAM service role**, choose the IAM service role you created for Amazon Personalize during set up\. This role must have read and write access to your input and output Amazon S3 buckets respectively\.

1. For **Solution**, choose the solution and then choose the **Solution version ID** that you want to use to generate the recommendations\. 

1.  For **Input data configuration**, specify the Amazon S3 path to your input file\. Your input data must be in the correct format for the recipe your solution uses\. For input data examples see [Input and output JSON examples](#batch-recommendations-json-examples)\. 

1. For **Output data configuration**, specify the path to your output location\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\.

1.  For **Filter configuration** optionally choose a filter to apply a filter to the recommendations added to the output JSON file\. For more information see [Filtering batch recommendations](filter-batch.md)\. 

1.  Choose **Create batch inference job**\. Batch inference job creation starts and the **Batch inference jobs** page appears with the **Batch inference job detail** section displayed\.

1.  When the batch inference job's status changes to **Active**, you can retrieve the job's output from the designated output Amazon S3 bucket\. The output file's name will be of the format `input-name.out`\. 

## Creating a batch inference job \(AWS CLI and AWS SDKs\)<a name="batch-sdk-cli"></a>

After you have completed [Preparing and importing batch input data](#batch-data-upload), you are ready to create a batch inference job using the following code\. Specify a `Batch job name`, replace `Solution version ARN` with the Amazon Resource Name \(ARN\) of your solution version, and replace the `IAM service role ARN` with the ARN of the IAM service role you created for Amazon Personalize during set up\. This role must have read and write access to your input and output Amazon S3 buckets respectively\. 

Replace `S3 input path` and `S3 output path` with the Amazon S3 path to your input file and output locations\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. You can apply a filter to the recommendations added to the output JSON file\. For more information see [Filtering batch recommendations](filter-batch.md)\. 

For `batch-inference-job-config`, the example includes optional `USER_PERSONALIZE` recipe specific `itemExplorationConfig` hyperparameters: `explorationWeight` and `explorationItemAgeCutOff`\. Optionally include `explorationWeight` and `explorationItemAgeCutOff` values to configure exploration\. For more information, see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

------
#### [ Python ]

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
       {"s3DataSource": {"path": "S3 input path"}},
    jobOutput = 
       {"s3DataDestination": {"path": "S3 output path"}}
)
```

------
#### [ CLI ]

```
aws personalize create-batch-inference-job --job-name Batch job name \
                --solution-version-arn Solution version ARN \
                --job-input s3DataSource={path=s3://S3 input path} \
                --job-output s3DataDestination={path=s3://S3 output path} \
                --role-arn IAM service role ARN \
                --batch-inference-job-config "{\"itemExplorationConfig\":{\"explorationWeight\":\"0.3\",\"explorationItemAgeCutOff\":\"30\"}}"
{
   "batchInferenceJobArn": "arn:aws:personalize:us-west-2:012345678901:batch-inference-job/batchTest"
}
```

------