# Tagging Amazon Personalize resources<a name="tagging-resources"></a>

A *tag* is a label that you optionally define and associate with AWS resources, including certain types of Amazon Personalize resources\. A resource can have as many as 50 tags\. 

Tags can help you categorize and manage resources in different ways, such as by purpose, environment, or other criteria\. For example, you can use tags to split revenue between different functions, or identify development environments for different resources\. 

 To retrieve Amazon Personalizes resources by tag, you can use the filters in GetResources operation of Resource Groups Tagging API\. For more information, see [GetResources](https://docs.aws.amazon.com/resourcegroupstagging/latest/APIReference/API_GetResources.html) in the *Resource Groups Tagging API* API Reference guide\. 

You can add tags to the following types of Amazon Personalize resources: 
+  Batch inference jobs 
+  Batch segment jobs 
+ Campaigns
+ Datasets
+ Dataset groups
+ Dataset import and export jobs
+ Event trackers
+ Filters
+ Recommenders
+ Solutions
+ Solution versions

**Topics**
+ [Managing tags](personalize-managing-tags.md)
+ [Adding tags to Amazon Personalize resources](tags-add.md)
+ [Using tags in IAM policies](tags-iam.md)