# Error messages<a name="error-messages"></a>

 The following sections list and explain some of the messages that you might encounter when using Amazon Personalize\. 

**Topics**
+ [Data import and management](#data-import-troubleshooting)
+ [Creating a solution and solution version \(Custom dataset group\)](#training-troubleshooting)
+ [Model deployment \(Custom dataset group campaigns\)](#deployment-troubleshooting)
+ [Recommenders \(Domain dataset group\)](#recommender-errors)
+ [Recommendations](#recommendations-troubleshooting)
+ [Filtering recommendations](#filters-troubleshooting)

## Data import and management<a name="data-import-troubleshooting"></a>

**Error message:** *Invalid Data location\.*

Make sure you used the correct syntax for your Amazon S3 bucket location\. For dataset import jobs, use the following syntax for the location of your data in Amazon S3:

**s3://<name of your S3 bucket>/<folder path>/<CSVfilename>**

If your CSV files are in a folder and you want to upload multiple files with one dataset import job, use this syntax without the CSV file name\. 

**Error message:** *An error occurred \(LimitExceededException\) when calling the CreateDatasetImportJob operation: More than 5 resources with PENDING or IN\_PROGRESS status\.*

 You can have a total of 5 pending or in progress dataset import jobs per region\. This quota is not adjustable\. For a complete list of quotas for Amazon Personalize, see [Amazon Personalize endpoints and quotas](limits.md)\. 

**Error message:** *Failed to create a data import job for <dataset type> dataset\.\.\.\.Insufficient privileges for accessing data in Amazon S3\.*

 Give Amazon Personalize access to your Amazon S3 resources by attaching access policies to your Amazon S3 bucket and your Amazon Personalize service\-linked role\. See [Giving Amazon Personalize access to Amazon S3 resources](granting-personalize-s3-access.md)\.

 If you are using AWS Key Management Service \(AWS KMS\) for encryption, you must give your IAM user and Amazon Personalize IAM service role permission to use your key\. You must also add Amazon Personalize as a Principle in your AWS KMS key policy\. For more information see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.

**Error message:** *Failed to create a data import job <dataset type> dataset\.\.\.Input CSV is missing the following columns:\[COLUMN\_NAME, COLUMN\_NAME\]\.*

 The data that you import into Amazon Personalize, including attribute names and data types, must match the destination dataset's schema\. For more information, see [Datasets and schemas](how-it-works-dataset-schema.md)\. 

## Creating a solution and solution version \(Custom dataset group\)<a name="training-troubleshooting"></a>

**Error message:** *Create failed\. Dataset has fewer than 25 users with at least 2 interactions each\.*

 You must import more data before you can train the model\. The minimum data requirements to train a model are: 
+ 1000 records of combined interaction data
+ 25 unique users with at least 2 interactions each

For real\-time recommendations, record more interaction *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)* for your users with an event tracker and the [PutEvents](API_UBS_PutEvents.md) operation\. For more information, on recording real\-time events, see [Recording events](recording-events.md)\. 

 For batch recommendations, import your data with a dataset import job when you have more data\. Because existing bulk data in the dataset is replaced, you must include the data you already imported into Amazon Personalize in the import\. For more information, about importing historical data see [Preparing and importing data](data-prep.md)\. 

## Model deployment \(Custom dataset group campaigns\)<a name="deployment-troubleshooting"></a>

**Error:** *Cannot create a campaign\. More than 5 resources in ACTIVE state\. Please delete some and try again\.*

 You can have a total of 5 active Amazon Personalize campaigns per region\. This quota is adjustable and you can request a quota increase using the [Service Quotas console](https://console.aws.amazon.com/servicequotas/)\. For a complete list of limits and quotas for Amazon Personalize, see [Amazon Personalize endpoints and quotas](limits.md)\. 

## Recommenders \(Domain dataset group\)<a name="recommender-errors"></a>

**Error:** *Dataset has fewer than 1000 interactions after filtering by event type: <event type>*

 Different use cases require different event types\. Your data must have at minimum 1000 events with the required type for your use case\. For more information, see [Choosing recommender use cases](domain-use-cases.md) 

## Recommendations<a name="recommendations-troubleshooting"></a>

**Batch inference job error message:** *Invalid S3 input path * or *Invalid S3 output path*

Make sure you use the correct syntax for your Amazon S3 input or output locations\. Also make sure that your output location is different from your input data\. It should be a folder in the same Amazon S3 bucket or a different bucket\.

Use the following syntax for the *input* file location in Amazon S3: **s3://<name of your S3 bucket>/<folder name>/<input JSON file name>**

Use the following syntax for the *output* folder in Amazon S3: **s3://<name of your S3 bucket>/<output folder name>/**

## Filtering recommendations<a name="filters-troubleshooting"></a>

**Error message:** *Could not create filter\. Invalid input symbol\.\.\.Placeholders can only be used with IN or EQ operator*

 Placeholder parameters cannot be used in range expressions\. For expressions that use = and IN operators only, use the dollar sign \($\) and a parameter name to add a placeholder parameter as a value\. 

 For more information, see [Filter expression elements](filter-expressions.md#filter-expression-elements)\. 

**Error message:** *Could not create filter\. Invalid Expression\.\.\.* when filtering on Boolean type fields

 You can't create filter expressions that filter using values with a Boolean type in your schema\. To filter based on Boolean values, use a schema with a field of type `String` and use the values `True` and `False` in your data\. Or you can use type `int` or `long` and values `0` and `1`\. 

For more information, see [Filter expression elements](filter-expressions.md#filter-expression-elements)\.