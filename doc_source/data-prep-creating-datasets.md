# Step 2: Creating a dataset and a schema<a name="data-prep-creating-datasets"></a>

After you have completed [Step 1: Creating a dataset group](data-prep-ds-group.md), you are ready to create a dataset\. *Datasets* are Amazon Personalize containers for data\. Datasets are organized within Amazon Personalize dataset groups\. 

You can create three types of historical datasets: Users, Items, and Interactions\. When you create a dataset, you also create a schema for the dataset\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\. 

You can create only one of each kind of dataset in a dataset group, and you must at minimum create an Interactions dataset\. You create datasets using the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDK\.

For more information, including dataset and schema requirements, see [Datasets and schemas](how-it-works-dataset-schema.md)\. 

**Topics**
+ [Creating a dataset and a schema \(console\)](#data-prep-creating-ds-console)
+ [Creating a dataset and a schema \(AWS CLI\)](#data-prep-creating-ds-cli)
+ [Creating a dataset and a schema \(AWS SDKs\)](#data-prep-creating-ds-sdk)

## Creating a dataset and a schema \(console\)<a name="data-prep-creating-ds-console"></a>

 If this is your first dataset in your dataset group, your first dataset type will be an Interactions dataset\. To create your Interactions dataset in the console, specify the dataset name and then specify a JSON schema in [Avro format](https://docs.oracle.com/database/nosql-12.1.3.0/GettingStartedGuide/avroschemas.html)\. If it is not your first dataset in this dataset group, choose the dataset type and then specify a name and a schema\. 

For information on Amazon Personalize datasets and schema requirements, see [Datasets and schemas](how-it-works-dataset-schema.md)\. 

**Note**  
 If you just completed [Step 1: Creating a dataset group](data-prep-ds-group.md) and you are already on the **user\-item interaction** page, skip to step 4 in this procedure\. 

**To create a dataset and a schema**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1.  On the **Dataset groups** page, choose the dataset group you created in [Step 1: Creating a dataset group](data-prep-ds-group.md)\. This displays the dataset group **Dashboard**\. 

1. In the **Upload datasets** section, for the type of dataset that you want to import \(Amazon Personalize datasets include Interactions, Users, or Items\), choose **Import**\. The **Configure < dataset type >** page is displayed\. 

1. In **Dataset details**, for **Dataset name**, specify a name for your dataset\.

1. In **Schema details**, for **Schema selection**, either choose an existing schema or choose **Create new schema**\.

1.  If you are creating a new schema, for **Schema definition**, paste in the schema JSON that matches your data\. Use the examples found in [Datasets and schemas](how-it-works-dataset-schema.md) as a guide\. 

1. For **New schema name**, specify a name for the new schema\.

1. Choose **Next** and follow the instructions in [ Step 3: Importing your data](data-prep-importing.md) to import your data\.

## Creating a dataset and a schema \(AWS CLI\)<a name="data-prep-creating-ds-cli"></a>

To create a dataset and a schema using the AWS CLI, you first define a schema in [Avro format](https://docs.oracle.com/database/nosql-12.1.3.0/GettingStartedGuide/avroschemas.html) and add it to Amazon Personalize using the [CreateSchema](API_CreateSchema.md) operation\. Then create a dataset using the [CreateDataset](API_CreateDataset.md) operation\. For information on Amazon Personalize datasets and schema requirements, see [Datasets and schemas](how-it-works-dataset-schema.md)\.

**To create a schema and dataset**

1. Create a schema file in Avro format and save it as a JSON file\. This file should be based on the type of dataset, such as Interactions, you are creating\.

   The schema must match the columns in your data and the schema `name` must match one of the three types of datasets recognized by Amazon Personalize\. The following is an example of a minimal Interactions dataset schema\. For more examples, see [Datasets and schemas](how-it-works-dataset-schema.md)\.

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
             "name": "TIMESTAMP",
             "type": "long"
         }
     ],
     "version": "1.0"
   }
   ```

1. Create a schema in Amazon Personalize by running the following command\. Replace `schemaName` with the name of the schema, and replace `file://SchemaName.json` with the location of the JSON file you created in the previous step\. The example shows the file as belonging to the current folder\. For more information about the API, see [CreateSchema](API_CreateSchema.md)\.

   ```
   aws personalize create-schema \
     --name SchemaName \
     --schema file://SchemaName.json
   ```

   The schema Amazon Resource Name \(ARN\) is displayed, as shown in the following example:

   ```
   {
     "schemaArn": "arn:aws:personalize:us-west-2:acct-id:schema/SchemaName"
   }
   ```

1. Create an empty dataset by running the following command\. Provide the dataset group Amazon Resource Name \(ARN\) from [Creating a dataset group \(AWS CLI\)](data-prep-ds-group.md#data-prep-creating-ds-group-cli) and schema ARN from the previous step\. The `dataset-type` must match the schema `name` from the previous step\. For more information about the API, see [CreateDataset](API_CreateDataset.md)\.

   ```
   aws personalize create-dataset \
     --name Dataset Name \
     --dataset-group-arn Dataset Group ARN \
     --dataset-type Dataset Type \
     --schema-arn Schema Arn
   ```

   The dataset ARN is displayed, as shown in the following example\.

   ```
   {
     "datasetArn": "arn:aws:personalize:us-west-2:acct-id:dataset/DatasetName/INTERACTIONS"
   }
   ```

1. Record the dataset ARN for later use\. After you have created a dataset, you are ready to import your training data\. See [ Step 3: Importing your data](data-prep-importing.md)\. 

## Creating a dataset and a schema \(AWS SDKs\)<a name="data-prep-creating-ds-sdk"></a>

To create a dataset and a schema using the AWS SDKs, you first define a schema in [Avro format](https://docs.oracle.com/database/nosql-12.1.3.0/GettingStartedGuide/avroschemas.html) and add it to Amazon Personalize using the [CreateSchema](API_CreateSchema.md) operation\. Then create a dataset using the [CreateDataset](API_CreateDataset.md) operation\. For information on Amazon Personalize datasets and schema requirements, see [Datasets and schemas](how-it-works-dataset-schema.md)\.

**To create a schema and a dataset**

1. Create a schema file in Avro format and save it as a JSON file in your working directory\.

   The schema must match the columns in your data and the schema `name` must match one of the three types of datasets recognized by Amazon Personalize\. The following is an example of a minimal Interactions dataset schema\. For more examples, see [Datasets and schemas](how-it-works-dataset-schema.md)\.

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
             "name": "TIMESTAMP",
             "type": "long"
         }
     ],
     "version": "1.0"
   }
   ```

1. Create the schema using the [CreateSchema](API_CreateSchema.md) API\.

------
#### [ SDK for Python \(Boto3\) ]

   Use the following `create_schema` method to create a schema\. Replace `schema name` with the name of your schema\.

   ```
   import boto3
   
   personalize = boto3.client('personalize')
   
   with open('schemaFile.json') as f:
       createSchemaResponse = personalize.create_schema(
           name = 'schema name',
           schema = f.read()
       )
   
   schema_arn = createSchemaResponse['schemaArn']
   
   print('Schema ARN:' + schema_arn )
   ```

------
#### [ SDK for Java 2\.x ]

   Use the following `createSchema` method to create a schema\. Pass the following as parameters: a PersonalizeClient, the name for your schema, and the file path for your schema JSON file\.

   ```
   public static String createSchema(PersonalizeClient personalizeClient, String schemaName, String filePath) {
       
       String schema = null;
       
       try {
           schema = new String(Files.readAllBytes(Paths.get(filePath)));
       } catch (IOException e) {
           System.out.println(e.getMessage());
       }
       
       try {
           CreateSchemaRequest createSchemaRequest = CreateSchemaRequest.builder()
                   .name(schemaName)
                   .schema(schema)
                   .build();
   
           String schemaArn = personalizeClient.createSchema(createSchemaRequest).schemaArn();    
           System.out.println("Schema arn: " + schemaArn);
   
           return schemaArn;
   
       } catch(PersonalizeException e) {
           System.err.println(e.awsErrorDetails().errorMessage());
           System.exit(1);
       }
       return "";
   }
   ```

------

   Amazon Personalize returns the ARN of the new schema\. Record it because you'll need it in the next step\.

1. Create a dataset using the [CreateDataset](API_CreateDataset.md) operation\. For types of datasets, see [Datasets and schemas](how-it-works-dataset-schema.md)\. 

------
#### [ SDK for Python \(Boto3\) ]

   Use the following `create_dataset` method to create an Amazon Personalize dataset\. Specify the `datasetGroupArn` returned in [Creating a dataset group \(AWS SDKs\)](data-prep-ds-group.md#data-prep-creating-ds-group-sdk)\. Use the `schemaArn` created in the previous step\. Replace `dataset type` with the type of dataset you are uploading \(Interactions, Users, or Items\)\.

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

   Use the following `createDataset` method to create an Amazon Personalize dataset\. Pass the following as parameters: a PersonalizeClient, the name for your dataset, the `schemaArn` created in the previous step, your dataset group ARN, and the dataset type \(Interactions, Users, or Items\)\.

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

   After you have created a dataset, you are ready to import your training data\. See [ Step 3: Importing your data](data-prep-importing.md)\.