# Updating data with individual import operations<a name="updating-data-individually"></a>

After you import data into an Amazon Personalize dataset, you can update it by importing additional individual records, including interaction events, users, or items\. Importing data individually allows you to add small batches of records to your Amazon Personalize datasets as your catalog grows\. 

 When you import records individually, Amazon Personalize appends the new records to the dataset\. To update an individual item or user, you can import a record with the same ID but with the modified attributes\. You can import up to 10 records per individual import operation\. 

For more information on importing records individually, see [Importing individual records](incremental-data-updates.md)\. 