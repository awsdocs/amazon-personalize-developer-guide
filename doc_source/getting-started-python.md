# Getting started \(SDK for Python \(Boto3\)\)<a name="getting-started-python"></a>

This tutorial shows you how to complete the Amazon Personalize workflow from start to finish with the SDK for Python \(Boto3\)\. 

To avoid incurring unnecessary charges, when you finish this getting started exercise delete the resources you create in this tutorial\. For more information, see [Cleaning up resources](gs-cleanup.md)\. 

**Topics**
+ [Prerequisites](#gs-sdk-prerequisites)
+ [Tutorial](#gs-python-custom-tutorial)
+ [Getting started using Amazon Personalize APIs with Jupyter \(iPython\) notebooks](#gs-jupyter-notebook)

## Prerequisites<a name="gs-sdk-prerequisites"></a>

The following are prerequisite steps for using the Python examples in this guide:
+ Complete the [Getting started prerequisites](gs-prerequisites.md) to set up the required permissions and create the training data\. If you are using your own source data, make sure that your data is formatted like the prerequisites\.
+ Set up your AWS SDK for Python \(Boto3\) environment as specified in [Setting up the AWS SDKs](aws-personalize-set-up-sdks.md)\.

## Tutorial<a name="gs-python-custom-tutorial"></a>

In the following steps, you verify your environment and create SDK for Python \(Boto3\) clients for Amazon Personalize\. Then you import data, create and deploy a solution version with a campaign, and get recommendations\.

### Step 1: Verify your Python environment and create boto3 clients<a name="gs-python-example"></a>

After you complete the prerequisites, run the following Python example to confirm that your environment is configured correctly\. This code also creates the Amazon Personalize boto3 clients you use in this tutorial\. If your environment is configured correctly, a list of the available recipes is displayed, and you can run the other examples in this tutorial\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')
personalize = boto3.client('personalize')

response = personalize.list_recipes()

for recipe in response['recipes']:
    print (recipe)
```

### Step 2: Import data<a name="getting-started-python-import-dataset"></a>

After you create Amazon Personalize boto3 clients and verify your environment, import the historical data you created when you completed the [Getting started prerequisites](gs-prerequisites.md)\. To import historical data into Amazon Personalize, do the following:

1. Use the following code to create a schema in Amazon Personalize\. Replace `getting-started-schema` with a name for the schema\. 

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
             "name": "TIMESTAMP",
             "type": "long"
         }
     ],
     "version": "1.0"
   }
   
   create_interactions_schema_response = personalize.create_schema(
       name='getting-started-schema',
       schema=json.dumps(schema)
   )
   
   interactions_schema_arn = create_interactions_schema_response['schemaArn']
   print(json.dumps(create_interactions_schema_response, indent=2))
   ```

1. Create a dataset group with the following code\. Replace `dataset group name` with a name for the dataset group\.

   ```
   response = personalize.create_dataset_group(name = 'dataset group name')
   dataset_group_arn = response['datasetGroupArn']
   
   description = personalize.describe_dataset_group(datasetGroupArn = dataset_group_arn)['datasetGroup']
   
   print('Name: ' + description['name'])
   print('ARN: ' + description['datasetGroupArn'])
   print('Status: ' + description['status'])
   ```

1. Create an Interactions dataset in your new dataset group with the following code\. Give the dataset a name and provide the `schema_arn` and `dataset_group_arn` from the previous steps\.

   ```
   response = personalize.create_dataset(
       name = 'datase_name',
       schemaArn = 'schema_arn',
       datasetGroupArn = 'dataset_group_arn',
       datasetType = 'Interactions'
   )
   
   dataset_arn = response['datasetArn']
   ```

1. Import your data with a dataset import job with the following code\. The code uses the describe\_dataset\_import\_job method to track the status of the job\. 

   Pass the following as parameters: a name for the job, the `dataset_arn` from the previous step, the Amazon S3 bucket path \(`s3://bucket name/folder name/ratings.csv`\) where you stored the training data, and your IAM service role's ARN\. You created this role as part of the [Getting started prerequisites](gs-prerequisites.md)\. Amazon Personalize needs permission to access the bucket\. See [Giving Amazon Personalize access to Amazon S3 resources](granting-personalize-s3-access.md)\. 

   ```
   import time
   response = personalize.create_dataset_import_job(
       jobName = 'JobName',
       datasetArn = 'dataset_arn',
       dataSource = {'dataLocation':'s3://bucket/file.csv'},
       roleArn = 'role_arn',
       importMode = 'FULL'
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

### Step 3: Create a solution<a name="getting-started-python-create-solution"></a>

After importing your data, you create a solution and solution version as follows\. The *solution* contains the configurations to train a model and a *solution version* is a trained model\. 

1.  Create a new solution with the following code\. Pass the following as parameters: the `dataset_group_arn` from earlier, a name for the solution, and the ARN for the User\-Personalization recipe \(`arn:aws:personalize:::recipe/aws-user-personalization`\)\. Store the ARN of your new solution for later use\. 

   ```
   create_solution_response = personalize.create_solution(
     name='solution name', 
     recipeArn= 'arn:aws:personalize:::recipe/aws-user-personalization', 
     datasetGroupArn = 'dataset group arn'
   )
   solution_arn = create_solution_response['solutionArn']
   print('solution_arn: ', solution_arn)
   ```

1. Create a solution version with the following code\. Pass as a parameter the `solution_arn` from the previous step\. The following code creates a solution version\. During training, the code uses the [DescribeSolutionVersion](API_DescribeSolutionVersion.md) operation to retrieve the solution version's status\. When training is complete, the method returns the ARN of your new solution version\. Store it for later use\. 

   ```
   import time
   import json
   
   create_solution_version_response = personalize.create_solution_version(
       solutionArn = solution_arn
   )
   
   solution_version_arn = create_solution_version_response['solutionVersionArn']
   print(json.dumps(create_solution_version_response, indent=2))
   
   max_time = time.time() + 3*60*60 # 3 hours
   while time.time() < max_time:
       describe_solution_version_response = personalize.describe_solution_version(
           solutionVersionArn = solution_version_arn
       )
       status = describe_solution_version_response["solutionVersion"]["status"]
       print("SolutionVersion: {}".format(status))
       
       if status == "ACTIVE" or status == "CREATE FAILED":
           break
           
       time.sleep(60)
   ```

### Step 4: Create a campaign<a name="getting-started-python-deploy-solution"></a>

After you create your solution version, deploy it with an Amazon Personalize campaign\. Use the following code to create a campaign that deploys your solution version\. Pass the following as parameters: the `solution_version_arn`, and a name for the campaign\. The method returns the Amazon Resource Name \(ARN\) of your new campaign\. Store it for later use\.

```
response = personalize.create_campaign(
    name = 'campaign name',
    solutionVersionArn = 'solution version arn'
)

arn = response['campaignArn']

description = personalize.describe_campaign(campaignArn = arn)['campaign']
print('Name: ' + description['name'])
print('ARN: ' + description['campaignArn'])
print('Status: ' + description['status'])
```

### Step 5: Get recommendations<a name="getting-started-python-get-recommendations"></a>

After you create a campaign, you can use it to get recommendations\. The following code shows how to get recommendations from a campaign and print out each recommended item's ID\. Pass the ARN of the campaign you created in the previous step\. For user ID, you pass the ID of a user that from the training data, such as `123`\.

```
response = personalizeRt.get_recommendations(
    campaignArn = 'Campaign ARN',
    userId = '123',
    numResults = 10
)

print("Recommended items")
for item in response['itemList']:
    print (item['itemId'])
```

## Getting started using Amazon Personalize APIs with Jupyter \(iPython\) notebooks<a name="gs-jupyter-notebook"></a>

 To get started using Amazon Personalize using Jupyter notebooks, clone or download a series of notebooks found in the [getting\_started](https://github.com/aws-samples/amazon-personalize-samples/tree/master/getting_started) folder of the [Amazon Personalize samples](https://github.com/aws-samples/amazon-personalize-samples) repository\. The notebooks walk you through importing training data, creating a solution, creating a campaign, and getting recommendations using Amazon Personalize\.

**Note**  
 Before starting with the notebooks, make sure to build your environment following the steps in the [README\.md](https://github.com/aws-samples/amazon-personalize-samples/blob/master/getting_started/README.md) 