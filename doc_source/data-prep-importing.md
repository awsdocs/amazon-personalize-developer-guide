# Step 3: Importing your historical data<a name="data-prep-importing"></a>

 When you have completed [Step 1: Creating a Custom dataset group](data-prep-ds-group.md) and [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md), you are ready to import your historical training data into Amazon Personalize\. 

The following covers importing only historical data\. If you don't have historical data, you can start with an empty Interactions dataset and record interactions data in real\-time as your users interact with your catalog\. For more information, see [Recording events](recording-events.md)\. 

When you import historical data, you can choose to import records in bulk, or individually, or both\. We recommend you use bulk imports when you have many new records that you want to import or update at once\. Use individual import operations to quickly add or update small groups of records\. For example, as customers create accounts, you might use the [PutUsers](API_UBS_PutUsers.md) operation to import small batches users into Amazon Personalize\. 

 As your catalog grows, we recommend that you complete additional imports to keep your data in Amazon Personalize up to date\. Keeping data current allows you to maintain recommendation relevance\. For more information, see [Maintaining recommendation relevance](maintaining-relevance.md)\. 

**Topics**
+ [Importing bulk records](bulk-data-import.md)
+ [Importing individual records](incremental-data-updates.md)