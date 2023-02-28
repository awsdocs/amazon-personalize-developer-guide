# Getting started with a Domain dataset group \(console\)<a name="getting-started-console-domain"></a>

In this exercise, you use the Amazon Personalize console to create a Domain dataset group and a recommender that returns movie recommendations for a given user\.

Before you start this exercise, review the [Getting started prerequisites](gs-prerequisites.md)\.

When you finish the getting started exercise, to avoid incurring unnecessary charges, follow the steps in [Cleaning up resources](gs-cleanup.md) to delete the resources you created\. 

## Step 1: Create a Domain dataset group<a name="getting-started-console-import-dataset-domain"></a>

 In this procedure you create Domain dataset group for the VIDEO\_ON\_DEMAND domain, create an Interactions dataset with the default schema for the VIDEO\_ON\_DEMAND domain, and import the interactions data you created in [Creating the training data \(Domain dataset group\)](gs-prerequisites.md#gs-data-prep-domain)\. 

**To create a Domain dataset group**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. In the navigation pane, choose **Create dataset group**\.

1. In **Dataset group details**, specify a name for your dataset group\. 

1.  For **Domain**, choose **Video on demand**\. The domain you choose determines the default schema you use when importing data\. It also determines what use cases are available for recommenders\. Your screen should look similar to the following\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/gs-domain-1-create-dsg.png)

1. Choose **Create dataset group and continue**\. The **Create interactions dataset** page appears\. Proceed to [Step 2: Import data](#getting-started-import-data-domain)\.

## Step 2: Import data<a name="getting-started-import-data-domain"></a>

 In this procedure you create an Interactions dataset with the default VIDEO\_ON\_DEMAND domain schema\. Then you import the interactions data you created in [Creating the training data \(Domain dataset group\)](gs-prerequisites.md#gs-data-prep-domain)\. 

**To import data**

1. On the **Create interactions dataset** page, for **Dataset name** provide a name for your Interactions dataset\. 

1. For **Dataset schema**, choose **Create a new domain schema by modifying the existing default schema for your domain** and enter a name for the schema\. The **Schema definition** updates to display the default schema for the VIDEO\_ON\_DEMAND domain\. Leave the schema unchanged\. Your screen should look similar to the following\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/gs-domain-2-create-dataset.png)

1. Choose **Create dataset and continue**\.

1. On the **Import interactions data** page, leave the **Data import source** unchanged as **Import data from S3**\.

1. For **Dataset import job name**, give your import job a name\.

1. For **Data location**, specify where your data is stored in Amazon Simple Storage Service \(S3\)\. Use the following syntax:

   **s3://<name of your S3 bucket>/<folder path>/<CSV filename>**

1. In **IAM role**, for **IAM service role** choose **Enter a custom IAM role ARN** and enter the Amazon Resource Name \(ARN\) of the role you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\. Your screen should look similar to the following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/gs-domain-3-import-job.png)

1. Choose **Import data** to import data\. The **Overview** page for your Domain dataset group appears\. Note the status of the import in the **Set up datasets** section\. When the status is `Interaction data active` proceed to [Step 3: Create a recommender](#getting-started-console-create-recommenders)\.

## Step 3: Create a recommender<a name="getting-started-console-create-recommenders"></a>

In this procedure, you create a recommender for the *Top picks for you* use case for the VIDEO\_ON\_DEMAND domain\.

**To create a recommender**

1.  On the **Overview** page for your Domain dataset group, on the middle card, choose the **Use video on demand recommenders** tab and choose **Create recommenders**\. 

1. On the **Create recommenders** page, choose **Top picks for you** and provide a **Recommender name**\. Leave the remaining fields unchanged\. Your screen should appear similar to the following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/gs-domain-4-create-recommender.png)

1. Choose **Create recommenders** to create your recommender\.

   You can monitor the status of each recommender on the **Recommenders** page\. When your recommender status is Active, you can use it to get recommendations in [Step 4: Get recommendations](#getting-started-console-get-recommendations-domain)\.

## Step 4: Get recommendations<a name="getting-started-console-get-recommendations-domain"></a>

In this procedure you use the recommender that you created in the previous step to get recommendations\.

**To get recommendations**

1. On the Overview page for your Domain dataset group, in the navigation pane choose **Recommenders**\.

1.  On the **Recommenders** page, choose your recommender\. 

1.  At the top right, choose **Test recommender**\. 

1. In **Recommendation parameters**, enter a user ID\. Leave the other fields unchanged\.

1. Choose **Get recommendations**\. A table containing the user’s top 25 recommended items appears\. Your screen should look similar to the following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/gs-domain-5-get-recc.png)