# Preparing and importing data<a name="data-prep"></a>

Amazon Personalize uses data that you provide to train a model\. When you import data, you can choose to import records in bulk or incrementally or both\. With incremental imports, you can add individual historical records or data from live events, or both, depending on your business requirements\. 

 The minimum data requirements to train a model are as follows: 
+  1000 records of combined interaction data \(after filtering by `eventType` and `eventValueThreshold`, if provided\)\.
+  25 unique users with at least 2 interactions each\. 

This section provides information about importing historical data into Amazon Personalize\. For information about recording live interactions data, see [Recording events](recording-events.md)\.

To import your historical training data into Amazon Personalize, you do the following:

1. Create an empty dataset group\.* Dataset groups* are domain\-specific containers for related datasets\. For more information, see [Step 1: Creating a Custom dataset group](data-prep-ds-group.md)\.

1. For each type of dataset you are using, create an empty dataset with an associated schema\. *Datasets* are Amazon Personalize containers for data and schemas that specify contents of a dataset\. For more information, see [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md)\. 

1. Import your data:
   +  Import bulk records stored in an Amazon S3 bucket using a dataset import job\. See [Importing bulk records](bulk-data-import.md)\. 
   +  Import records incrementally using the AWS python SDK or AWS Command Line Interface \(AWS CLI\)\. See [Importing records incrementally](incremental-data-updates.md)\. 