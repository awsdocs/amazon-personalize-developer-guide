# Importing Bulk Records<a name="bulk-data-import"></a>

**Important**  
Bulk imports in Amazon Personalize are a full refresh of bulk data\. Existing bulk data in the dataset is replaced\. This does not include records imported incrementally\.

 After you have completed [Step 1: Creating a Dataset Group](data-prep-ds-group.md) and [Step 2: Creating a Dataset and a Schema](data-prep-creating-datasets.md), you can import bulk records, such as a large CSV file, into an Amazon Personalize dataset\.

Amazon Personalize updates any filters you created in the dataset group with your new item and user data within 15 minutes from the last bulk import\. This update allows your campaigns to use your most recent data when filtering recommendations for your users\. 

To import bulk records, you do the following:

1. Format your input data as a comma\-separated values \(CSV\)\.

1. Upload your CSV files to an Amazon Simple Storage Service \(Amazon S3\) bucket and give Amazon Personalize access to your Amazon S3 resources\.

1. Create a dataset import job that populates the dataset with data from your S3 bucket\.

**Topics**
+ [Formatting Your Input Data](data-prep-formatting.md)
+ [Uploading to an Amazon S3 Bucket](data-prep-upload-s3.md)
+ [Importing Bulk Records with a Dataset Import Job](bulk-data-import-step.md)