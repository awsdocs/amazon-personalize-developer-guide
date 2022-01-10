# Importing data into Domain dataset group datasets<a name="importing-domain-dsg-data"></a>

 After you create a Domain dataset group and one or more datasets, you are ready to import data into Amazon Personalize\. When you import data, you can choose to import records in bulk, import records individually, or both, depending on your business requirements and the amount of historical data you have collected\. If you have a large amount of historical records, we recommend you first import data in bulk and then add data incrementally as your catalog grows\. Keeping your data current helps maintain the relevance of your recommendations\. If you completed [Creating a Domain dataset group](create-domain-dataset-group.md), you may have already imported interactions data\. 

**Topics**
+ [Importing bulk records](#importing-bulk-domain-dsg)
+ [Importing records incrementally](#incremental-import-domain-dsg)

## Importing bulk records<a name="importing-bulk-domain-dsg"></a>

**Important**  
Bulk imports in Amazon Personalize are a full refresh of bulk data\. Existing bulk data in the dataset is replaced\. This does not include records imported incrementally\.

Import bulk records into an Amazon Personalize dataset with a dataset import job\. A *dataset import job* is a bulk import tool that populates your dataset with data from your Amazon S3 bucket\. You create a dataset import job and import bulk records with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\. 

 You create a dataset import job for a dataset in a Domain dataset group the same way you would for a dataset in a Custom dataset group\. For step by step instructions see [Importing bulk records with a dataset import job](bulk-data-import-step.md)\. 

## Importing records incrementally<a name="incremental-import-domain-dsg"></a>

After you create a Domain dataset group and datasets, you can incrementally import one or more new records, including interaction *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)*, users, or items, to an existing dataset\. Incrementally importing records allows you to import one or more records into your Amazon Personalize datasets as your catalog grows\. 

**Filter updates for incremental record imports**

Amazon Personalize updates any filters you created in the dataset group with your new interaction, item, and user data within 20 minutes from the last incremental import\. This update allows your campaigns to use your most recent data when filtering recommendations for your users\. 

**How new records influence recommendations**

If you have already created a solution version \(trained a model\), new records influence recommendations as follows:
+  For *new events*, Amazon Personalize immediately uses historical and real\-time interaction events between a user and existing items \(items you included in the data you used to train the latest model\) when generating recommendations for the same user\. Historical events that you import using the Amazon Personalize console and events that you record in real\-time influence recommendations in the same way\. For more information, see [How real\-time events influence recommendations](recording-events.md#recorded-events-influence-recommendations)\. 
+ For *new items*, if you trained the solution version with User\-Personalization, Amazon Personalize automatically updates the model every two hours\. After each update, the new items can be included in recommendations with exploration\. For information about exploration see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

   For any other recipe, you must retrain the model for the new items to be included in recommendations\. 
+  For *new users*, recommendations will initially be only for popular items\. Starting with the first event, user recommendations will be more relevant as you record events\. For more information, see [Recording events](recording-events.md)\. 

You incrementally import records with the Amazon Personalize console, AWS CLI, or AWS SDKs\. You incrementally import records for a dataset in a Domain dataset group the same way you incrementally import records for a Custom dataset group\. For step by step instructions see [Importing records incrementally](incremental-data-updates.md)\. 