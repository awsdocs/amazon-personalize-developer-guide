# Importing bulk records<a name="bulk-data-import"></a>

 After you have completed [Step 1: Creating a Custom dataset group](data-prep-ds-group.md) and [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md), you are ready to import bulk records, such as a large CSV file, into an Amazon Personalize dataset\. 

To import bulk records, you do the following:

1. Format your input data as a comma\-separated values \(CSV\)\.

1. Upload your CSV files to an Amazon Simple Storage Service \(Amazon S3\) bucket and give Amazon Personalize access to your Amazon S3 resources\.

1. Create a dataset import job that populates the dataset with data from your Amazon S3 bucket\. To create a dataset import job for interactions datasets, your CSV file must have at minimum 1000 interaction records\.

After you import data into an Amazon Personalize dataset, you can analyze it, export it to an Amazon S3 bucket, update it, or delete it by deleting the dataset\. For more information, see [Managing data](managing-data.md)\.

**Filter updates for bulk records**

Within 15 minutes of completing a bulk import, Amazon Personalize updates any filters you created in the dataset group with your new item and user data\. This update allows Amazon Personalize to use the most recent data when filtering recommendations for your users\. 

**How new records influence recommendations**

If you already created a solution version \(trained a model\), new bulk records influence recommendations as follows:
+  For *new interactions*, you must create a new solution version for bulk interactions data to influence recommendations\. 
+ For *new items*, if you trained the solution version with User\-Personalization, Amazon Personalize automatically updates the model every two hours\. After each update, the new items might be included in recommendations with exploration\. For information about automatic updates see [Automatic updates](native-recipe-new-item-USER_PERSONALIZATION.md#automatic-updates)\. 

   For any other recipe, you must create a new solution version for the new items to be included in recommendations\. 
+ For *new users* without interactions data, recommendations are initially for only popular items\. If you have metadata about the user in a Users dataset and you choose a recipe that uses metadata, such as User\-Personalization or Personalized\-Ranking, these popular items will be more relevant for the user\. To get relevant recommendations for a new user, you can import bulk interactions for the user and create a new solution version\. Or you can record events for the user in real time as they interact with your catalog\. Their recommendations will be more relevant as you record more events\. For more information, see [Recording events](recording-events.md)\. 

**Topics**
+ [Formatting your input data](data-prep-formatting.md)
+ [Uploading to an Amazon S3 bucket](data-prep-upload-s3.md)
+ [Importing bulk records with a dataset import job](bulk-data-import-step.md)