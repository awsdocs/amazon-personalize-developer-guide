# Getting batch recommendations and user segments<a name="recommendations-batch"></a>

 To get recommendations and user segments for datasets that do not require real\-time updates, you can use an asynchronous batch workflow with historical data only\. For example, you might get product recommendations for all users on an email list, [item\-to\-item similarities](native-recipe-sims.md) across an inventory\. Or with the USER\_SEGMENTATION recipes, create user segments for data\-driven advertising based on items in your inventory and your user's interactions\. You don't need to create an Amazon Personalize campaign to get batch recommendations or create user segments\. 
+  To get batch recommendations, you use a batch inference job\. A *batch inference job* is a tool that imports your batch input data from an Amazon S3 bucket, uses your solution version to generate *item recommendations*, and then exports the recommendations to an Amazon S3 bucket\. 

   The input data can be a list of users or items or list of users each with a collection of items in JSON format\. Use a batch inference job when you want to get batch item recommendations for your users or find similar items across an inventory\. 
+  To get user segments, you use a batch segment job\. A *batch segment job* is a tool that imports your batch input data from an Amazon S3 bucket, uses your solution version trained with a USER\_SEGMENTATION recipe to generate * user segments* for each row of input data, and exports the segments to an Amazon S3 bucket\. Each user segment is sorted in descending order based on the probability that each user will interact with items in your inventory\. 

   Depending on the recipe, the input data is a list of items or item metadata attributes in JSON format\. For item attributes, your input data can include expressions to create user segments based on multiple metadata attributes\. Use a batch segment job when you create a solution with a USER\_SEGMENTATION recipe to get segments of users who will most likely interact with each of your items in your inventory\. 

 For both batch workflows, we recommend that you use a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. To use data you record in real time with the PutEvents API operation, you must retrain your solution version before creating a batch inference job or batch segment job\. 

The batch workflow is as follows:

1.  Prepare and upload your input data in JSON format to an Amazon S3 bucket\. The format of your input data depends on the recipe you use and the job you are creating\. See [Preparing and importing batch input data](batch-data-upload.md)\. 

1.  Create a separate location for your output data, either a folder or a different Amazon S3 bucket\. 

1.  Create a batch inference job or a batch segment job\. See [Creating a batch inference job \(console\)](creating-batch-inference-job.md#batch-console), [Creating a batch inference job \(AWS CLI\)](creating-batch-inference-job.md#batch-cli), or [Creating a batch inference job \(AWS SDKs\)](creating-batch-inference-job.md#batch-sdk)\. 

1.  When the batch inference or batch segment job is complete, retrieve the recommendations or user segments from your output location in Amazon S3\. 

**Topics**
+ [Batch workflow permissions requirements](#batch-permissions-req)
+ [Batch workflow scoring](#batch-scoring)
+ [Preparing and importing batch input data](batch-data-upload.md)
+ [Creating a batch inference job](creating-batch-inference-job.md)
+ [Creating a batch segment job](creating-batch-seg-job.md)

## Batch workflow permissions requirements<a name="batch-permissions-req"></a>

 For batch workflows, your Amazon Personalize IAM service role needs permission to access and add files to your Amazon S3 buckets\. For information on granting permissions, see [Service\-linked role policy for batch workflows](granting-personalize-s3-access.md#role-policy-for-batch-workflows)\. For more information on bucket permissions, see [User policy examples](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-policies-s3.html) in the *Amazon Simple Storage Service Developer Guide*\. 

 Amazon S3 buckets and objects must be either encryption free or, if you are using AWS Key Management Service \(AWS KMS\) for encryption, you must give your IAM user and Amazon Personalize service role permission to use your key\. You must also add Amazon Personalize as a Principle in your AWS KMS key policy\. For more information, see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.

## Batch workflow scoring<a name="batch-scoring"></a>

Amazon Personalize calculates batch inference job Item scores as described in [Getting real\-time recommendations](getting-real-time-recommendations.md)\. You can view scores in the batch inference job's output JSON file\. Scores are only returned by models trained with the HRNN and Personalize\-Ranking recipes\.