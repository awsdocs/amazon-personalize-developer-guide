# Creating a Domain dataset group and importing interaction data \(AWS CLI\)<a name="creating-domain-dataset-group-cli"></a>

 When you create a Domain dataset group, you choose a domain, create a schema and an Interactions dataset, and import historical data\. If you don't have historical data you can choose to record interactions data incrementally later\. 

 When you create an Interactions dataset, you can either use the default schema for your domain, or customize it to create your own schema\. A schema allows Amazon Personalize to read your data\. Use the fields and their types as a guide to determine what data to import into Amazon Personalize\. For more information about default schemas, see [Domain datasets and schemas](domain-datasets-and-schemas.md)\. 

**Topics**
+ [Step 1: Create a Domain dataset group](#create-domain-dsg-cli)
+ [Step 2: Create a schema and an Interactions dataset](#create-domain-interactions-dataset-cli)
+ [Step 3: Import interactions data](#import-domain-interactions-cli)

## Step 1: Create a Domain dataset group<a name="create-domain-dsg-cli"></a>

Create a Domain dataset group and choose your domain with the AWS CLI with the following command\. For domain, specify either `ECOMMERCE` or `VIDEO_ON_DEMAND`\. 

```
aws personalize create-dataset-group \
--name dataset group name \
--domain domain name
```

## Step 2: Create a schema and an Interactions dataset<a name="create-domain-interactions-dataset-cli"></a>

After you complete [Step 1: Create a Domain dataset group](#create-domain-dsg-cli), create a schema and an Interactions dataset to store data from interactions between your users and items in your catalog\.

**To create a schema and a dataset**

1. Create an Interactions schema file in Avro format and save it as a JSON file in your working directory\.

   The schema must match the columns in your data and the schema `name` must be `Interactions`\. The following is an example of the default Interactions schema for the VIDEO\_ON\_DEMAND domain\. For more information, see [Domain datasets and schemas](domain-datasets-and-schemas.md)\.

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

1. Create a schema in Amazon Personalize by running the following command\. Replace `schemaName` with the name of the schema, replace `business domain` with `VIDEO_ON_DEMAND` or `ECOMMERCE`, and replace `file://SchemaName.json` with the location of the JSON file you created in the previous step\. The example shows the file as belonging to the current folder\. After you create a schema, you can't make changes to the schema\. For more information about the API, see [CreateSchema](API_CreateSchema.md)\.

   ```
   aws personalize create-schema \
   --name schema name \
   --domain business domain \
   --schema file://SchemaName.json
   ```

   Amazon Personalize returns the ARN of the new schema\. Record it because you'll need it in the next step\.

1. Create a dataset using the [CreateDataset](API_CreateDataset.md) operation\. For information about the different types of datasets, see [Datasets and schemas](how-it-works-dataset-schema.md)\. 

   Use the following `create-dataset` command to create an Interactions dataset\. Give the dataset a name and specify the `datasetGroupArn` returned in [Step 1: Create a Domain dataset group](#create-domain-dsg-cli)\. Use the `schemaArn` from the schema you just created\.

   ```
   aws personalize create-dataset \
   --name Dataset Name \
   --dataset-group-arn Dataset Group ARN \
   --dataset-type INTERACTIONS \
   --schema-arn Schema Arn
   ```

   After you have created a dataset, you are ready to import data\. See [Step 3: Import interactions data](#import-domain-interactions-cli)\.

## Step 3: Import interactions data<a name="import-domain-interactions-cli"></a>

**Important**  
By default, a dataset import job replaces any existing data in the dataset that you imported in bulk\. To add new records without replacing existing data, specify INCREMENTAL for the `import-mode`\. For more information, see [Updating existing bulk data](updating-existing-bulk-data.md)\.

After you complete [Step 2: Create a schema and an Interactions dataset](#create-domain-interactions-dataset-cli), you are ready to import data\. If you have data in Amazon S3, import data with a dataset import job\. If you want to only incrementally import interactions data, you can skip this step and instead use the [PutEvents](API_UBS_PutEvents.md) operation until you have at least 1000 combined historical and incremental interactions\. For more information see [Recording events](recording-events.md)\. 

 For bulk imports, create a dataset import job by running the following command\. Provide the dataset Amazon Resource Name \(ARN\) for your Domain dataset group and specify the path to your Amazon S3 bucket where you stored the training data\. Use the following syntax for the path:

**s3://<name of your S3 bucket>/<folder path>/<CSV filename>**

Supply the AWS Identity and Access Management \(IAM\) role Amazon Resource Name \(ARN\) that you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\. The default `import-mode` is `FULL`\. For more information see [Updating existing bulk data](updating-existing-bulk-data.md)\. For more information about the API operation, see [CreateDatasetImportJob](API_CreateDatasetImportJob.md)\.

```
aws personalize create-dataset-import-job \
--job-name dataset import job name \
--dataset-arn dataset arn \
--data-source dataLocation=s3://bucketname/filename \
--role-arn roleArn
--import-mode FULL
```

The dataset import job ARN is displayed, as shown in the following example\.

```
{
  "datasetImportJobArn": "arn:aws:personalize:us-west-2:acct-id:dataset-import-job/DatasetImportJobName"
}
```

Check the status by using the `describe-dataset-import-job` command\. Provide the dataset import job ARN\. For more information about the operation, see [DescribeDatasetImportJob](API_DescribeDatasetImportJob.md)\.

```
aws personalize describe-dataset-import-job \
  --dataset-import-job-arn dataset import job arn
```

The properties of the dataset import job, including its status, are displayed\. Initially, the `status` shows as CREATE PENDING\.

```
{
  "datasetImportJob": {
      "jobName": "Dataset Import job name",
      "datasetImportJobArn": "arn:aws:personalize:us-west-2:acct-id:dataset-import-job/DatasetImportJobArn",
      "datasetArn": "arn:aws:personalize:us-west-2:acct-id:dataset/DatasetGroupName/INTERACTIONS",
      "dataSource": {
          "dataLocation": "s3://<bucketname>/ratings.csv"
      },
      "roleArn": "role-arn",
      "status": "CREATE PENDING",
      "importMode": "FULL",
      "creationDateTime": 1542392161.837,
      "lastUpdatedDateTime": 1542393013.377
  }
}
```

The dataset import is complete when the status shows as ACTIVE\.