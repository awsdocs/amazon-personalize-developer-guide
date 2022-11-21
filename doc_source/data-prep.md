# Preparing and importing data<a name="data-prep"></a>

Amazon Personalize uses data that you provide to train a model\. When you import data, you can choose to import records in bulk, individually, or both\. With individual imports, you can import historical records or data from live events\. As your catalog grows, we recommend that you complete additional imports to keep your data in Amazon Personalize up to date\. For real\-time recommendations, keep your Interactions dataset up to date with your users' behavior by recording interaction live *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)* with an event tracker and the [PutEvents](API_UBS_PutEvents.md) operation\.

 For all recipes, your interactions data must have the following: 
+ At minimum 1000 interactions records from users interacting with items in your catalog\. These interactions can be from bulk imports, or streamed events, or both\.
+ At minimum 25 unique user IDs with at least 2 interactions for each\.

To import your training data into Amazon Personalize, you do the following:

1. Create an empty dataset group\. *Dataset groups* are containers for Amazon Personalize components\. For more information, see [Step 1: Creating a Custom dataset group](data-prep-ds-group.md)\.

1. For each type of dataset you are using, create an empty dataset with an associated schema\. *Datasets* are Amazon Personalize containers for data and schemas tell Amazon Personalize about the structure of your data\. For more information, see [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md)\. 

1. Import your data:
   + Import bulk records stored in an Amazon S3 bucket using a dataset import job\. See [Importing bulk records](bulk-data-import.md)\.
   + Import historical records individually with the Amazon Personalize console or the [PutUsers](API_UBS_PutUsers.md), [PutItems](API_UBS_PutItems.md) APIs\. See [Importing individual records](incremental-data-updates.md)\. 
   + Import data from user interactions in real time with the [PutEvents](API_UBS_PutEvents.md) API operation\.

This section provides information about importing historical data into Amazon Personalize\. For information about recording data from live events in real time, see [Recording events](recording-events.md)\.