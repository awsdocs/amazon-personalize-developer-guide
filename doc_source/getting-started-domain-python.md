# Getting started with a Domain dataset group \(SDK for Python \(Boto3\)\)<a name="getting-started-domain-python"></a>

This tutorial shows you how to use the SDK for Python \(Boto3\) to create a Domain dataset group for the VIDEO\_ON\_DEMAND domain\. In this tutorial, you create a recommender for the *Top picks for you* use case\.

To avoid incurring unnecessary charges, when you finish this getting started exercise delete the resources you create in this tutorial\. For more information, see [Cleaning up resources](gs-cleanup.md)\. 

**Topics**
+ [Prerequisites](#gs-sdk-domain-prerequisites)
+ [Tutorial](#gs-python-tutorial)
+ [Getting started using Amazon Personalize APIs with Jupyter \(iPython\) notebooks](#gs-jupyter-domain-notebook)

## Prerequisites<a name="gs-sdk-domain-prerequisites"></a>

The following are prerequisite steps for using the Python examples in this guide:
+ Complete the [Getting started prerequisites](gs-prerequisites.md) to set up the required permissions and create the training data\. If you are using your own source data, make sure that your data is formatted like in the prerequisites\.
+ Set up your AWS SDK for Python \(Boto3\) environment as specified in [Setting up the AWS SDKs](aws-personalize-set-up-sdks.md)\.

## Tutorial<a name="gs-python-tutorial"></a>

In the following steps, you verify your environment and create SDK for Python \(Boto3\) clients for Amazon Personalize\. Then you import data, create a recommender for the *Top picks for you* use case, and get recommendations\.

### Step 1: Verify your Python environment and create boto3 clients<a name="gs-python-domain-verify-environment"></a>

After you complete the prerequisites, run the following Python example to confirm that your environment is configured correctly\. This code also creates the Amazon Personalize boto3 clients that you use in this tutorial\. If your environment is configured correctly, a list of the available recipes is displayed and you can run the other examples in this tutorial\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')
personalize = boto3.client('personalize')

response = personalize.list_recipes()

for recipe in response['recipes']:
    print (recipe)
```

### Step 2: Import data<a name="getting-started-python-domain-import-dataset"></a>

After you create Amazon Personalize boto3 clients and verify your environment, import the historical data you created when you completed the [Getting started prerequisites](gs-prerequisites.md)\. To import historical data into Amazon Personalize, do the following:

1. Use the following code to create a schema in Amazon Personalize\. Replace `gs-domain-interactions-schema` with a name for the schema\. 

   ```
   import json
   schema = {
     "type": "record",
     "name": "Interactions",
     "namespace": "com.amazonaws.personalize.schema",
     "fields": [
         {
             "name": "USER_ID",
             "type": "string"
         },
         {
             "name": "ITEM_ID",
             "type": "string"
         },
         {
             "name": "EVENT_TYPE",
             "type": "string"
         },
         {
             "name": "TIMESTAMP",
             "type": "long"
         }
     ],
     "version": "1.0"
   }
   
   create_interactions_schema_response = personalize.create_schema(
       name='gs-domain-interactions-schema',
       schema=json.dumps(schema),
       domain='VIDEO_ON_DEMAND'
   )
   
   interactions_schema_arn = create_interactions_schema_response['schemaArn']
   print(json.dumps(create_interactions_schema_response, indent=2))
   ```

1. Create a dataset group with the following code\. Replace `dataset group name` with a name for the dataset group\.

   ```
   response = personalize.create_dataset_group(
     name = 'dataset group name',
     domain = 'VIDEO_ON_DEMAND'
   )
   dsg_arn = response['datasetGroupArn']
   
   description = personalize.describe_dataset_group(datasetGroupArn = dsg_arn)['datasetGroup']
   
   print('Name: ' + description['name'])
   print('ARN: ' + description['datasetGroupArn'])
   print('Status: ' + description['status'])
   ```

1. Create an Interactions dataset in your new dataset group with the following code\. Give the dataset a name and provide the `schema_arn` and `dataset_group_arn` from the previous steps\.

   ```
   response = personalize.create_dataset(
       name = 'interactions-dataset-name',
       schemaArn = interactions_schema_arn,
       datasetGroupArn = dsg_arn,
       datasetType = 'INTERACTIONS'
   )
   
   dataset_arn = response['datasetArn']
   ```

1. Import your data with a dataset import job with the following code\. The code uses the describe\_dataset\_import\_job method to track the status of the job\. 

   Pass the following as parameters: a name for the job, the `dataset_arn` from the previous step, the Amazon S3 bucket path \(`s3://bucket name/folder name/ratings.csv`\) where you stored the training data, and your IAM service role's ARN\. You created this role as part of the [Getting started prerequisites](gs-prerequisites.md)\. Amazon Personalize needs permission to access the bucket\. For information on granting access, see [Giving Amazon Personalize access to Amazon S3 resources](granting-personalize-s3-access.md)\. 

   ```
   import time
   response = personalize.create_dataset_import_job(
       jobName = 'JobName',
       datasetArn = 'dataset_arn',
       dataSource = {'dataLocation':'s3://bucket/file.csv'},
       roleArn = 'role_arn'
   )
   
   dataset_interactions_import_job_arn = response['datasetImportJobArn']
   
   description = personalize.describe_dataset_import_job(
       datasetImportJobArn = dataset_interactions_import_job_arn)['datasetImportJob']
   
   print('Name: ' + description['jobName'])
   print('ARN: ' + description['datasetImportJobArn'])
   print('Status: ' + description['status'])
   
   max_time = time.time() + 3*60*60 # 3 hours
   while time.time() < max_time:
       describe_dataset_import_job_response = personalize.describe_dataset_import_job(
           datasetImportJobArn = dataset_interactions_import_job_arn
       )
       status = describe_dataset_import_job_response["datasetImportJob"]['status']
       print("Interactions DatasetImportJob: {}".format(status))
       
       if status == "ACTIVE" or status == "CREATE FAILED":
           break
           
       time.sleep(60)
   ```

### Step 4: Create a recommender<a name="domain-gs-py-create-recommender"></a>

After your dataset import job completes, you are ready create a recommender\. Use the following code to create a recommender\. Pass the following as parameters: a name for the recommender, your dataset group's Amazon Resource Name \(ARN\), and `arn:aws:personalize:::recipe/aws-vod-top-picks` for the recipe ARN\. The code uses the describe\_recommender method to track the status of the recommender\.

```
import time
create_recommender_response = personalize.create_recommender(
  name = 'gs-python-top-picks',
  recipeArn = 'arn:aws:personalize:::recipe/aws-vod-top-picks',
  datasetGroupArn = dsg_arn     
)
recommender_arn = create_recommender_response['recommenderArn']

print('Recommender ARN:' + recommender_arn)
max_time = time.time() + 3*60*60 # 3 hours
while time.time() < max_time:

    version_response = personalize.describe_recommender(
        recommenderArn = recommender_arn
    )
    status = version_response["recommender"]["status"]

    if status == "ACTIVE":
        print("Creation succeeded for {}".format(recommender_arn))
        
    elif status == "CREATE FAILED":
        print("Creation failed for {}".format(recommender_arn))

    if status == "ACTIVE":
        break
    else:
        print("Recommender creation is still in progress")
        
    time.sleep(60)
```

### Step 5: Get recommendations<a name="domain-gs-py-get-recommendations"></a>

After you create a recommender, you use it to get recommendations with the following code\. Pass as parameters the Amazon Resource Name \(ARN\) of the recommender you created in the previous step, and a user ID \(for example, `123`\)\. The method prints the list of recommended items\. 

```
response = personalizeRt.get_recommendations(
    recommenderArn = "arn:aws:personalize:us-west-2:014025156336:recommender/gs-python-top-picks-89",
    userId = '123'
)
print("Recommended items")
for item in response['itemList']:
    print (item['itemId'])
```

## Getting started using Amazon Personalize APIs with Jupyter \(iPython\) notebooks<a name="gs-jupyter-domain-notebook"></a>

 To get started creating Domain dataset groups with Jupyter notebooks, clone or download a series of notebooks found in the [notebooks\_managed\_domains](https://github.com/aws-samples/amazon-personalize-samples/tree/master/getting_started/notebooks_managed_domains) folder of the [Amazon Personalize samples](https://github.com/aws-samples/amazon-personalize-samples) repository\. The notebooks walk you through importing training data, creating a recommender, and getting recommendations with Amazon Personalize\.

**Note**  
 Before starting with the notebooks, make sure to build your environment following the steps in the [README\.md](https://github.com/aws-samples/amazon-personalize-samples/blob/master/getting_started/README.md) 