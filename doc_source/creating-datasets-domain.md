# Creating datasets<a name="creating-datasets-domain"></a>

After you create a Domain dataset group and an Interactions dataset in [Creating a Domain dataset group](create-domain-dataset-group.md), you can create an Items schema and dataset or a Users schema and dataset to store metadata about your users and items\. 

## Creating datasets for a Domain dataset group \(console\)<a name="create-domain-datasets-dataset-console"></a>

Create a dataset in a Domain dataset group with the Amazon Personalize console as follows\.

**To create a dataset \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1.  On the **Dataset groups** page, choose your Domain dataset group\. 

1. Start creating a dataset by choosing either of the following:
   + On the **Overview** page, in **Create datasets** choose **Import user data** or **Import item data**\. 
   + From the navigation pane, choose **Datasets** and choose **Create dataset**\. On the **Dataset selection** page choose User or Item and choose **Start import**\.

1.  Configure a schema for your dataset\. Follow the instructions in [Step 2: Create a schema and Interactions dataset](creating-domain-dataset-group-console.md#create-domain-interactions-dataset-console) to configure a schema and create the dataset\. The **Schema fields** are different for each domain and dataset type\. For information on domain datasets and schemas, see [Domain datasets and schemas](domain-datasets-and-schemas.md)\. 

1.  Choose **Next** to import data\. Follow the instructions in [Step 3: Import interactions data](creating-domain-dataset-group-console.md#import-domain-interactions-console) to import data\. 

## Creating datasets for a Domain dataset group \(AWS CLI\)<a name="create-domain-datsets-cli"></a>

To create a dataset with the AWS CLI, you first use the `create-schema` command to define a schema for the dataset\. Then the `create-dataset` command to create the dataset\. 

For code examples, see [Step 2: Create a schema and an Interactions dataset](creating-domain-dataset-group-cli.md#create-domain-interactions-dataset-cli)\. In each example, replace INTERACTIONS with your dataset's type \(ITEMS or USERS\)\.

## Creating datasets for a Domain dataset group \(AWS SDKs\)<a name="create-domain-datsets-sdk"></a>

To create a dataset with the AWS SDKs you first use the [ CreateSchema ](API_CreateSchema.md) operation to define a schema for the dataset\. Then use the [ CreateDataset ](API_CreateDataset.md) operation to create the dataset\. 

For examples that use the SDK for Python \(Boto3\), see [Step 2: Create a schema and an Interactions dataset](creating-domain-dataset-group-sdk.md#create-domain-interactions-dataset-sdk)\. In each example, replace INTERACTIONS with your dataset's type \(ITEMS or USERS\)\.