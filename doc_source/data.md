# Types of data you can import into Amazon Personalize<a name="data"></a>

 The following topics introduce the different types of data that you can import into Amazon Personalize Interactions, Users, and Items datasets\. If you import data in bulk, your data must be stored in comma\-separated values \(CSV\) format\. The first row of your CSV file must contain column headers\.

Each type of dataset has different data requirements\. For both Domain dataset groups and Custom dataset groups, your interactions data must have the following before training: 
+ At minimum 1000 interactions records from users interacting with items in your catalog\. These interactions can be from bulk imports, or streamed events, or both\.
+ At minimum 25 unique user IDs with at least 2 interactions for each\.

If you create a Domain dataset group, each dataset has additional requirements depending on domain\. If you aren't sure what type of data you need, we recommend creating a Domain dataset group and using the default schemas for your domain as a guide\. For more information about dataset and schema requirements see [Datasets and schemas](how-it-works-dataset-schema.md)\. 

**Topics**
+ [Interactions data](interactions-datasets.md)
+ [User data](users-datasets.md)
+ [Item data](items-datasets.md)