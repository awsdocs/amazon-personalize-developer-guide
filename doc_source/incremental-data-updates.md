# Importing individual records<a name="incremental-data-updates"></a>

 After you have completed [Step 1: Creating a Custom dataset group](data-prep-ds-group.md) and [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md), you can import individual records, including interaction *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)*, users, or items, into an existing dataset\. Importing data individually allows you to add small batches of records to your Amazon Personalize datasets as your catalog grows\. You can import up to 10 records per individual import operation\. 

 If you have a large amount of historical records, we recommend that you first import data in bulk and then import data individually as necessary\. See [Importing bulk records](bulk-data-import.md)\. 

**Filter updates for individual record imports**

Amazon Personalize updates any filters you created in the dataset group with your new interaction, item, and user data within 15 minutes from the last individual import\. This update allows your campaigns to use your most recent data when filtering recommendations for your users\. 

**How new records influence recommendations**

If you already created a solution version \(trained a model\), new individual records influence recommendations as follows:
+  For *new interactions*, Amazon Personalize immediately uses real\-time interactions data involving existing items \(items you included in the data you used to train the latest model\)\. For information on interactions involving new items or users, see [How real\-time events influence recommendations](recording-events.md#recorded-events-influence-recommendations)\. 
+ For *new items* \(items not included in the data you used to train the latest model\), if you trained the solution version with User\-Personalization and deployed it in a campaign, Amazon Personalize automatically updates the model to use the new item data every two hours\. After each update, the new items can be included in recommendations with exploration\. For information about automatic updates, see [Automatic updates](native-recipe-new-item-USER_PERSONALIZATION.md#automatic-updates)\. 

   For any other recipe, you must create a new solution version for the new items to be included in recommendations\. 
+ For *new users* without interactions data, recommendations are initially for only popular items\. To get relevant recommendations for a new user, you can import bulk interactions for the user and create a new solution version\. Or you can record events for the user in real time as they interact with your catalog\. Their recommendations will be more relevant as you record more events\. For more information, see [Recording events](recording-events.md)\. 

**Topics**
+ [Importing interactions individually](importing-interactions.md)
+ [Importing users individually](importing-users.md)
+ [Importing items individually](importing-items.md)