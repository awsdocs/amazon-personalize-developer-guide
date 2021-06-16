# Importing records incrementally<a name="incremental-data-updates"></a>

 After you have completed [Step 1: Creating a dataset group](data-prep-ds-group.md) and [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md), you can incrementally import one or more new records, including interaction *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)*, users, or items, to an existing dataset\. Incrementally importing records allows you to import one or more records into your Amazon Personalize datasets as your catalog grows\. If you have a large amount of historical records, we recommend that you first import data in bulk and then import data incrementally as necessary\. See [Importing bulk records](bulk-data-import.md)\. 

**Filter updates for incremental record imports**

Amazon Personalize updates any filters you created in the dataset group with your new interaction, item, and user data within 20 minutes from the last incremental import\. This update allows your campaigns to use your most recent data when filtering recommendations for your users\. 

**How new records influence recommendations**

If you have already created a solution version \(trained a model\), new records influence recommendations as follows:
+  For *new events*, Amazon Personalize immediately uses historical and real\-time interaction events between a user and existing items \(items you included in the data you used to train the latest model\) when generating recommendations for the same user\. Historical events that you import using the Amazon Personalize console and events that you record in real\-time influence recommendations in the same way\. For more information, see [How real\-time events influence recommendations](recording-events.md#recorded-events-influence-recommendations)\. 
+ For *new items*, if you trained the solution version with the User\-Personalization recipe, Amazon Personalize automatically updates the model every two hours\. After each update, the new items can be included in recommendations with exploration\. For information about exploration see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

   For any other recipe, you must retrain the model for the new items to be included in recommendations\. 
+  For *new users*, recommendations will initially be only for popular items\. Starting with the first event, user recommendations will be more relevant as you record events\. For more information, see [Recording events](recording-events.md)\. 

**Topics**
+ [Importing interactions incrementally](importing-interactions.md)
+ [Importing users incrementally](importing-users.md)
+ [Importing items incrementally](importing-items.md)