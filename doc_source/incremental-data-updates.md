# Importing Records Incrementally<a name="incremental-data-updates"></a>

 Once you have completed [Step 1: Creating a Dataset Group](data-prep-ds-group.md) and [Step 2: Creating a Dataset and a Schema](data-prep-creating-datasets.md), you can incrementally add one or more new items or users to an existing dataset\. 

**Filter Updates for Incremental Record Imports**

Amazon Personalize updates any filters you created in the dataset group with your new item and user data within 20 minutes from the last incremental import\. This update allows your campaigns to use your most recent data when filtering recommendations for your users\. 

**How New Records Influence Recommendations**

If you have already created a solution version \(trained a model\), new items and users influence recommendations as follows:
+  For *new items*, if you trained the model using the User\-Personalization recipe, Amazon Personalize automatically updates the model every two hours, and after each update the new items influence recommendations\. See [User\-Personalization Recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

   For any other recipe, you must re\-train the model for the new items to influence recommendations\. 
+  For *new users*, recommendations will initially be for popular items only\. As you record events for the user, recommendations will be more relevant\. For more information, see [Recording Events](recording-events.md)\. 

**Topics**
+ [Importing Interactions Incrementally](importing-interactions.md)
+ [Importing Users Incrementally](importing-users.md)
+ [Importing Items Incrementally](importing-items.md)