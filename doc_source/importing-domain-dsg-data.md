# Importing data into Domain dataset group datasets<a name="importing-domain-dsg-data"></a>

 After you create a Domain dataset group and one or more datasets, you are ready to import data into Amazon Personalize\. When you import data, you can choose to import records in bulk, individually, or both\. With individual imports, you can import historical records or data from live events\. As your catalog grows, we recommend that you complete additional imports to keep your data in Amazon Personalize up to date\. For real\-time recommendations, keep your Interactions dataset up to date with your users' behavior by recording interaction live *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)* with an event tracker and the [PutEvents](API_UBS_PutEvents.md) operation\.

If you have a large amount of historical records, we recommend you first import data in bulk and then add data individually as your catalog grows\. Keeping your data current helps maintain the relevance of your recommendations\. If you completed [Creating a Domain dataset group](create-domain-dataset-group.md), you may have already imported interactions data\. 

**Topics**
+ [Importing bulk records](#importing-bulk-domain-dsg)
+ [Importing records individually](#incremental-import-domain-dsg)

## Importing bulk records<a name="importing-bulk-domain-dsg"></a>

**Important**  
By default, a dataset import job replaces any existing data in the dataset that you imported in bulk\. After you import bulk records, you can add new bulk records without replacing existing data by configuring the job's import mode\.

You import bulk records into an Amazon Personalize dataset with a dataset import job\. A *dataset import job* is a bulk import tool that populates your dataset with data from your Amazon S3 bucket\. You create a dataset import job and import bulk records with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\. 

 You create a dataset import job for a dataset in a Domain dataset group the same way you would for a dataset in a Custom dataset group\. For step by step instructions see [Importing bulk records with a dataset import job](bulk-data-import-step.md)\. 

 If you previously created a dataset import job for a dataset, you can use another import job to add to or replace the existing bulk data\. By default, a dataset import job replaces any existing data in the dataset that you imported in bulk\. You can instead append the new records to existing data by changing the job's [import mode](updating-existing-bulk-data.md#bulk-import-modes)\. 

 To append data to an Interactions dataset with a dataset import job, you must have at minimum 1000 new interaction records\. For more information about updating bulk data see [Updating existing bulk data](updating-existing-bulk-data.md)\. 

## Importing records individually<a name="incremental-import-domain-dsg"></a>

After you create a Domain dataset group and datasets, you can individually import one or more new records, including interaction *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)*, users, or items, to an existing dataset\. Individually importing records allows you to import one or more records into your Amazon Personalize datasets as your catalog grows\. You can individually import records to your dataset in bulk, or you can stream imports to your dataset in real time to update it\.

You can import records individually with the Amazon Personalize console, AWS CLI, or AWS SDKs\. You import records individually the same way you do for a dataset in a Custom dataset group\. For step by step instructions see [Importing individual records](incremental-data-updates.md)\. 