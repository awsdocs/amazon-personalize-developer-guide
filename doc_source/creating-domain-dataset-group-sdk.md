# Creating a Domain dataset group and importing interaction data \(AWS SDKs\)<a name="creating-domain-dataset-group-sdk"></a>

 When you create a Domain dataset group, you choose a domain, create a schema and an Interactions dataset, and import historical data\. If you don't have historical data, you can choose to record interactions data incrementally later\. 

 When you create an Interactions dataset, you can either use the default schema for your domain, or customize it to create your own schema\. A schema allows Amazon Personalize to read your data\. Use the fields and their types as a guide to determine what data to import into Amazon Personalize\. For more information about default schemas, see [Domain datasets and schemas](domain-datasets-and-schemas.md)\. 

**Topics**
+ [Step 1: Create a Domain dataset group](#create-domain-dsg-sdk)
+ [Step 2: Create a schema and an Interactions dataset](#create-domain-interactions-dataset-sdk)
+ [Step 3: Import interactions data](#import-domain-interactions-sdk)

## Step 1: Create a Domain dataset group<a name="create-domain-dsg-sdk"></a>

Use the following code to create a Domain dataset group\. Give the Domain dataset group a name, and for `domain`, specify either `ECOMMERCE` or `VIDEO_ON_DEMAND`\. 

------
#### [ SDK for Python \(Boto3\) ]

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

------
#### [ SDK for Java 2\.x ]

```
public static String createDomainDatasetGroup(PersonalizeClient personalizeClient, 
                                              String datasetGroupName,
                                              String domain) {

    try {
        CreateDatasetGroupRequest createDatasetGroupRequest = CreateDatasetGroupRequest.builder()
                .name(datasetGroupName)
                .domain(domain)
                .build();
        return personalizeClient.createDatasetGroup(createDatasetGroupRequest).datasetGroupArn();
    } catch (PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
    return "";
}
```

------

Amazon Personalize returns the Amazon Resource Name \(ARN\) of the new Domain dataset group\. Record it because you'll need it when you create a dataset\.

## Step 2: Create a schema and an Interactions dataset<a name="create-domain-interactions-dataset-sdk"></a>

After you complete [Step 1: Create a Domain dataset group](#create-domain-dsg-sdk), create a schema and an Interactions dataset to store data from interactions between your users and items in your catalog\.

**To create a schema and Interactions dataset**

1. Create a schema file in Avro format and save it as a JSON file in your working directory\.

   The schema must match the columns in your data and the schema `name` must be `Interactions`\. The following is the default Interactions schema for the VIDEO\_ON\_DEMAND domain\. For more information, see [Domain datasets and schemas](domain-datasets-and-schemas.md)\.

   ```
   {
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

1. Create the schema with the [CreateSchema](API_CreateSchema.md) API operation\. 

------
#### [ SDK for Python \(Boto3\) ]

   Use the following SDK for Python \(Boto3\) code to create a schema\. Give the schema a name, replace `schema.json` with the filepath for your schema JSON file, and replace `business domain` with either `VIDEO_ON_DEMAND` or `ECOMMERCE`\.

   ```
   import boto3
   
   personalize = boto3.client('personalize')
   
   with open('schema.json') as f:
       createSchemaResponse = personalize.create_schema(
           name = 'schema name',
           domain = 'business domain'        
           schema = f.read()
       )
   
   schema_arn = createSchemaResponse['schemaArn']
   
   print('Schema ARN:' + schema_arn )
   ```

------
#### [ SDK for Java 2\.x ]

   Use the following `createSchema` method to create a domain schema\. Pass the following as parameters: a PersonalizeClient, the name for your schema, your business domain \(either ECOMMERCE or VIDEO\_ON\_DEMAND\) and the file path for your schema JSON file\.

   ```
   public static String createDomainSchema(PersonalizeClient personalizeClient, String schemaName, 
                                                             String domain, 
                                                             String filePath) {
   
       String schema = null;
       try {
           schema = new String(Files.readAllBytes(Paths.get(filePath)));
       } catch (IOException e) {
           System.out.println(e.getMessage());
       }
   
       try {
           CreateSchemaRequest createSchemaRequest = CreateSchemaRequest.builder()
                   .name(schemaName)
                   .domain(domain)
                   .schema(schema)
                   .build();
   
           String schemaArn = personalizeClient.createSchema(createSchemaRequest).schemaArn();
   
           System.out.println("Schema arn: " + schemaArn);
   
           return schemaArn;
   
       } catch (PersonalizeException e) {
           System.err.println(e.awsErrorDetails().errorMessage());
           System.exit(1);
       }
       return "";
   }
   ```

------

   Amazon Personalize returns the Amazon Resource Name \(ARN\) of the new schema\. Record it because you'll need it in the next step\.

1. Create an Interactions dataset with the [CreateDataset](API_CreateDataset.md) operation\. For information about the different types of domain datasets, see [Domain datasets and schemas](domain-datasets-and-schemas.md)\. 

   Use the following code to create an Interactions dataset with the AWS SDKs\. Give the dataset a name and specify the `datasetGroupArn` returned in [Step 1: Create a Domain dataset group](#create-domain-dsg-sdk)\. Use the `schemaArn` created in the previous step\. For `datasetType` specify `Interactions`\.

------
#### [ SDK for Python \(Boto3\) ]

   ```
   import boto3
   
   personalize = boto3.client('personalize')
   
   response = personalize.create_dataset(
       name = 'datase_name',
       schemaArn = 'schema_arn',
       datasetGroupArn = 'dataset_group_arn',
       datasetType = 'dataset_type'
   )
   
   print ('Dataset Arn: ' + response['datasetArn'])
   ```

------
#### [ SDK for Java 2\.x ]

   ```
   public static String createDataset(PersonalizeClient personalizeClient, 
                                       String datasetName, 
                                       String datasetGroupArn, 
                                       String datasetType, 
                                       String schemaArn) {
       try {
           CreateDatasetRequest request = CreateDatasetRequest.builder()
                   .name(datasetName)
                   .datasetGroupArn(datasetGroupArn)
                   .datasetType(datasetType)
                   .schemaArn(schemaArn).build();
       
           String datasetArn = personalizeClient.createDataset(request).datasetArn();
           System.out.println("Dataset " + datasetName + " created. Dataset ARN: " + datasetArn);
           
           return datasetArn;
           
       } catch(PersonalizeException e) {
           System.err.println(e.awsErrorDetails().errorMessage());
           System.exit(1);
       }
       return "";
   }
   ```

------

   Amazon Personalize returns the Amazon Resource Name \(ARN\) of the new dataset\. Record it because you'll need it when you import data\. After you have created a dataset, you are ready to import data\. See [Step 3: Import interactions data](#import-domain-interactions-sdk)\.

## Step 3: Import interactions data<a name="import-domain-interactions-sdk"></a>

After you complete [Step 2: Create a schema and an Interactions dataset](#create-domain-interactions-dataset-sdk), you are ready to import data\. If you have data in Amazon S3, import data with the [CreateDatasetImportJob](API_CreateDatasetImportJob.md) operation\. If you want to only incrementally import interactions data, you can skip this step and instead use the [PutEvents](API_UBS_PutEvents.md) operation until you have at least 1000 combined historical and incremental interactions\. For more information see [Recording events](recording-events.md)\. 

The following code shows how to import bulk data into a dataset with a dataset import job\.

------
#### [ SDK for Python \(Boto3\) ]

Specify the `datasetGroupArn` and set the `dataLocation` to the path to your Amazon S3 bucket where you stored the training data\. Use the following syntax:

**s3://<name of your S3 bucket>/<folder path>/<CSV filename>**

For the `roleArn`, specify the AWS Identity and Access Management \(IAM\) role that gives Amazon Personalize permissions to access your Amazon S3 bucket\. See [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\.

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

------
#### [ SDK for Java 2\.x ]

Use the following `createPersonalizeDatasetImportJob` method to create a dataset import job\. Pass the following as parameters: an Amazon Personalize service client, a name for the job, the dataset's Amazon Resource Name \(ARN\), the Amazon S3 bucket path \(`s3://<bucketname>/<file name.csv>`\) where you stored your training data, and your Amazon Personalize IAM service role's ARN \(see [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\)\. 

If your CSV files are in a folder in an Amazon S3 bucket, you can upload multiple CSV files to a dataset in one dataset import job\. For the bucket path, specify the `bucket-name/folder-name/` instead of the file name\.

```
public static String createPersonalizeDatasetImportJob(PersonalizeClient personalizeClient,
                                                      String jobName,
                                                      String datasetArn,
                                                      String s3BucketPath,
                                                      String roleArn) {

  long waitInMilliseconds = 60 * 1000;
  String status;
  String datasetImportJobArn;
  
  try {
      DataSource importDataSource = DataSource.builder()
              .dataLocation(s3BucketPath)
              .build();
      
      CreateDatasetImportJobRequest createDatasetImportJobRequest = CreateDatasetImportJobRequest.builder()
              .datasetArn(datasetArn)
              .dataSource(importDataSource)
              .jobName(jobName)
              .roleArn(roleArn)
              .build();
  
      datasetImportJobArn = personalizeClient.createDatasetImportJob(createDatasetImportJobRequest)
              .datasetImportJobArn();
      
      DescribeDatasetImportJobRequest describeDatasetImportJobRequest = DescribeDatasetImportJobRequest.builder()
              .datasetImportJobArn(datasetImportJobArn)
              .build();
  
      long maxTime = Instant.now().getEpochSecond() + 3 * 60 * 60;
  
      while (Instant.now().getEpochSecond() < maxTime) {
  
          DatasetImportJob datasetImportJob = personalizeClient
                  .describeDatasetImportJob(describeDatasetImportJobRequest)
                  .datasetImportJob();
  
          status = datasetImportJob.status();
          System.out.println("Dataset import job status: " + status);
  
          if (status.equals("ACTIVE") || status.equals("CREATE FAILED")) {
              break;
          }
          try {
              Thread.sleep(waitInMilliseconds);
          } catch (InterruptedException e) {
              System.out.println(e.getMessage());
          }
      }
      return datasetImportJobArn;
  
  } catch (PersonalizeException e) {
      System.out.println(e.awsErrorDetails().errorMessage());
  }
  return "";
}
```

------

Use the[DescribeDatasetImportJob](API_DescribeDatasetImportJob.md) operation to retrieve the status of the operation\. You must wait until the status changes to ACTIVE before you can use the data to create a recommender\.