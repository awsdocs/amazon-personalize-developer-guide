# Step 3: Importing Your Data<a name="data-prep-importing"></a>

 When you have completed [Step 1: Creating a Dataset Group](data-prep-ds-group.md) and [Step 2: Creating a Dataset and a Schema](data-prep-creating-datasets.md), you are ready to import your training data into Amazon Personalize\. When you import data, you can choose to import records in bulk, import records individually, or both, depending on your business requirements and the amount of historical data you have collected\. If you have a large amount of historical records, we recommend you first import data in bulk and then add data incrementally as necessary\. 

**Important**  
Bulk imports in Amazon Personalize are a full refresh of bulk data\. Existing bulk data in the dataset is replaced\. This does not include records imported incrementally\.

 To import real\-time event data, record user activity \(events\) in real\-time using the AWS SDK, AWS Amplify, or AWS CLI\. For more information see [Recording Events](recording-events.md) 

**Topics**
+ [Importing Bulk Records](bulk-data-import.md)
+ [Importing Records Incrementally](incremental-data-updates.md)