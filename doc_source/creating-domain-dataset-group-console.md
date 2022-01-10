# Creating a Domain dataset group and importing interaction data \(console\)<a name="creating-domain-dataset-group-console"></a>

 When you create a Domain dataset group, you choose a domain, create a schema and an Interactions dataset, and import historical data\. If you don't have historical data, you can choose to record interactions data incrementally later\. 

 When you create an Interactions dataset, you choose to use the default schema for your domain and customize it, or choose a pre\-existing schema\. A schema allows Amazon Personalize to read your data\. Use the fields and their types as a guide to determine what data to import into Amazon Personalize\. For more information about default schemas, see [Domain datasets and schemas](domain-datasets-and-schemas.md) 

**Topics**
+ [Step 1: Create a Domain dataset group](#create-domain-dsg-console)
+ [Step 2: Create a schema and Interactions dataset](#create-domain-interactions-dataset-console)
+ [Step 3: Import interactions data](#import-domain-interactions-console)

## Step 1: Create a Domain dataset group<a name="create-domain-dsg-console"></a>

Create a Domain dataset group and choose your domain with the Amazon Personalize console as follows\.

**To create a domain dataset group**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. Choose **Create dataset group**\.

1. In **Dataset group details**, for **Name**, specify a name for your dataset group\. 

1.  For **Dataset group domain**, choose **E\-commerce** to create an ECOMMERCE Domain dataset group, or choose **Video on demand** to create a VIDEO\_ON\_DEMAND Domain dataset group\. The domain you choose determines the default schema you will use when importing data and determines what use cases are available for recommenders\. 

1. Choose **Create dataset group and continue**\. The **Create interactions dataset** page appears\. Proceed to [Step 2: Create a schema and Interactions dataset](#create-domain-interactions-dataset-console)\.

## Step 2: Create a schema and Interactions dataset<a name="create-domain-interactions-dataset-console"></a>

After you complete [Step 1: Create a Domain dataset group](#create-domain-dsg-console), create an Interactions dataset to store data from interactions between your users and items in your catalog\. 

**To create a schema and Interactions dataset**

1. On the **Create interactions dataset** page, for **Dataset name** provide a name for your Interactions dataset\. 

1. For **Dataset schema** choose which schema you want to use:
   + Choose **Create a new domain schema by modifying the existing default schema for your domain** and enter a schema name if you want use your domain's default schema as a template and optionally add any fields\.
   + Choose **Use an existing schema** and choose an existing schema that is compliant with your domain\. The **Existing schema** field is disabled if no eligible schemas exist\.

1. After you choose your schema, the **Schema fields** table updates to display the schema's fields\. Choose the **Tabular schema view** tab to modify the schema in a table, or choose the `JSON schema code` tab to view and modify the schema's JSON code\. 

1. For the **Tabular schema view**, use the following fields to modify your schema\.
   + To add custom fields, choose the **Add custom field** button and give the new field a name and choose its type\. 
   +  To remove optional and custom fields, select the check box for the field and choose the **Remove** button\. 

1. If you have bulk data in a CSV file in Amazon S3, make sure your data matches the schema definition here\. Your CSV file must have the same columns of data listed in the **Schema definition** and each field must have the same type\. 

1. Choose **Next**\. The **Import interactions data** page appears\. Proceed to [Step 3: Import interactions data](#import-domain-interactions-console)

## Step 3: Import interactions data<a name="import-domain-interactions-console"></a>

After you complete [Step 2: Create a schema and Interactions dataset](#create-domain-interactions-dataset-console), import your interactions data from Amazon S3 into your Interactions dataset\. If you don't have bulk data in Amazon S3, you can skip this step and incrementally import interactions data with the event ingestion SDK and the [PutEvents](API_UBS_PutEvents.md) operation\. For more information see [Recording events](recording-events.md)\.

**To import interactions data**

1.  On the **Import interactions data** page, for **Data import source** choose how you want to import your data from one of the following options: 
   + Choose **Import bulk data from S3** if you have bulk historical data stored in an Amazon S3 bucket\. Your bucket must have the correct permissions\. For more information on granting permissions, see [Giving Amazon Personalize access to Amazon S3 resources](granting-personalize-s3-access.md)\. You can still incrementally import data with APIs after you import data from Amazon S3\. 
   +  Or choose **Incrementally data with APIS** if you don't have historical data in Amazon S3 and want to incrementally import interactions data with the event ingestion SDK and the [PutEvents](API_UBS_PutEvents.md) operation\. If you choose this option, you will need to collect data until you have recorded the minimum 1000 interactions before you create a recommender\. For more information see [Recording events](recording-events.md)\. 

1. For **Dataset import job name**, give your import job a name\.

1. For **Data location**, specify where your movie data file is stored in Amazon Simple Storage Service \(S3\)\. Use the following syntax:

   **s3://<name of your S3 bucket>/<folder path>/<CSV filename>**
**Note**  
If your CSV files are in a folder in your S3 bucket and you want to upload multiple CSV files to a dataset with one dataset import job, use this syntax without the CSV file name\. 

1. In **IAM role**, for **IAM service role** choose one of the following:
   + Choose **Create and use new service role** and provide a **Service role name** to create a new service role with the `AmazonPersonalizeFullAccess` policy attached\.
   + If you already created a role, choose **Use an existing service role**\. For information on creating a role, see [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\.

1. Choose **Import data** to import data\. The **Overview** page for your Domain dataset group appears\. Note the status of the import in the **Set up datasets** section\.