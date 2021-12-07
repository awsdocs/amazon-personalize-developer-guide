# Creating a Domain dataset group and importing interaction data \(AWS SDKs\)<a name="creating-domain-dataset-group-sdk"></a>

 When you create a Domain dataset group, you choose a domain, create a schema and an Interactions dataset, and import historical data\. If you don't have historical data, you can choose to record interactions data incrementally later\. 

 When you create an Interactions dataset, you can either use the default schema for each dataset for your domain, or customize it to create your own schema\. A schema allows Amazon Personalize to read your data\. Use the fields and their types as a guide to determine what data to import into Amazon Personalize\. For more information about default schemas, see [Domain datasets and schemas](domain-datasets-and-schemas.md)\. 

**Topics**
+ [Step 1: Create a Domain dataset group](#create-domain-dsg-sdk)
+ [Step 2: Create a schema and an Interactions dataset](#create-domain-interactions-dataset-sdk)
+ [Step 3: Import interactions data](#import-domain-interactions-sdk)

## Step 1: Create a Domain dataset group<a name="create-domain-dsg-sdk"></a>

Create a Domain dataset group and choose your domain with the SDK for Python \(Boto3\) with the following code\. For domain, specify either `ECOMMERCE` or `VIDEO_ON_DEMAND`\. 

```
import boto3

personalize = boto3.client('personalize')

response = personalize.create_dataset_group(
  name = 'dataset group name'
  domain = 'business domain'
)
dsg_arn = response['datasetGroupArn']

description = personalize.describe_dataset_group(datasetGroupArn = dsg_arn)['datasetGroup']

print('Name: ' + description['name'])
print('ARN: ' + description['datasetGroupArn'])
print('Status: ' + description['status'])
```

## Step 2: Create a schema and an Interactions dataset<a name="create-domain-interactions-dataset-sdk"></a>

After you complete [Step 1: Create a Domain dataset group](#create-domain-dsg-sdk), create a schema and an Interactions dataset to store data from interactions between your users and items in your catalog\.

**To create a schema and Interactions dataset**

1. Create a schema file in Avro format and save it as a JSON file in your working directory\.

   The schema must match the columns in your data and the schema `name` must be `Interactions`\. The following SDK for Python \(Boto3\) code assigns the default Interactions schema for the VIDEO\_ON\_DEMAND domain to an `interaction_schema` variable\. For more information, see [Domain datasets and schemas](domain-datasets-and-schemas.md)\.

   ```
   interaction_schema = {
   
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
   ```

1. Create the schema using the [ CreateSchema ](API_CreateSchema.md) API operation\. Give the schema a name and replace `business domain` with either `VIDEO_ON_DEMAND` or `ECOMMERCE`\.

   ```
   import boto3
   
   personalize = boto3.client('personalize')
       createSchemaResponse = personalize.create_schema(
           domain = 'business domain'
           name = 'schema name',
           schema = json.dumps(interaction_schema)
       )
   
   schema_arn = createSchemaResponse['schemaArn']
   
   print('Schema ARN:' + schema_arn )
   ```

   Amazon Personalize returns the ARN of the new schema\. Record it because you'll need it in the next step\.

1. Create a dataset using the [ CreateDataset ](API_CreateDataset.md) operation\. For information about the different types of domain datasets, see [Domain datasets and schemas](domain-datasets-and-schemas.md)\. 

   Use the following `create_dataset` method to create an Interactions dataset\. Give the dataset a name and specify the `datasetGroupArn` returned in [Step 1: Create a Domain dataset group](#create-domain-dsg-sdk)\. Use the `schemaArn` created in the previous step\.

   ```
   import boto3
   
   personalize = boto3.client('personalize')
   
   response = personalize.create_dataset(
       name = 'datase_name',
       schemaArn = 'schema_arn',
       datasetGroupArn = 'dataset_group_arn',
       datasetType = 'INTERACTIONS'
   )
   interaction_dataset_arn = response['datasetArn']
   print ('Dataset Arn: ' + response['datasetArn'])
   ```

   After you have created a dataset, you are ready to import data\. See [Step 3: Import interactions data](#import-domain-interactions-sdk)\.

## Step 3: Import interactions data<a name="import-domain-interactions-sdk"></a>

After you complete [Step 2: Create a schema and an Interactions dataset](#create-domain-interactions-dataset-sdk), you are ready to import data\. If you have data in Amazon S3, import data with a dataset import job\. If you want to only incrementally import interactions data, you can skip this step and instead use the [ PutEvents ](API_UBS_PutEvents.md) operation until you have at least 1000 combined historical and incremental interactions\. For more information see [Recording events](recording-events.md)\. 

The following code shows how to import bulk data with the SDK for Python \(Boto3\)\. Specify the `datasetGroupArn` for your Domain dataset group and set the `dataLocation` to the path to your Amazon S3 bucket where you stored the training data\.

For the `roleArn`, specify the AWS Identity and Access Management \(IAM\) role that gives Amazon Personalize permissions to access your S3 bucket\. See [Creating an IAM service role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\.

```
import boto3

personalize = boto3.client('personalize')

response = personalize.create_dataset_import_job(
    jobName = 'YourImportJob',
    datasetArn = 'dataset_arn',
    dataSource = {'dataLocation':'s3://bucket/file.csv'},
    roleArn = 'role_arn'
)

dsij_arn = response['datasetImportJobArn']

print ('Dataset Import Job arn: ' + dsij_arn)

description = personalize.describe_dataset_import_job(
    datasetImportJobArn = dsij_arn)['datasetImportJob']

print('Name: ' + description['jobName'])
print('ARN: ' + description['datasetImportJobArn'])
print('Status: ' + description['status'])
```