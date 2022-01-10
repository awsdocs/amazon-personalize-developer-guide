# Cleaning up resources<a name="gs-cleanup"></a>

To avoid incurring unnecessary charges, delete the resources you created after you're done with the getting started exercise\. To delete the resources, use either the Amazon Personalize console or the `Delete` APIs from the SDKs or the AWS Command Line Interface \(AWS CLI\)\. For example, use the [DeleteCampaign](API_DeleteCampaign.md) API to delete a campaign\.

You can't delete a resource whose status is CREATE PENDING or IN PROGRESS\. The resource status must be ACTIVE or CREATE FAILED\. Check the status using the `Describe` APIs, for example, [DescribeCampaign](API_DescribeCampaign.md)\.

Some resources must be deleted before others, as shown in the following table\. This process can take some time\.

To delete the training data you uploaded, `ratings.csv`, see [How do I delete objects from an S3 bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-objects.html)\.

**Topics**
+ [Cleaning up domain\-based resources](#cleaning-up-domain-resources)
+ [Cleaning up custom resources](#cleaning-up-custom-resources)

## Cleaning up domain\-based resources<a name="cleaning-up-domain-resources"></a>

If you created a Domain dataset group, delete resources as follows:


| Resource to be deleted | Delete this first | Notes | 
| --- | --- | --- | 
| [Recommender](API_Recommender.md) |  |  | 
| [DatasetImportJob](API_DatasetImportJob.md) |  | Can't be deleted\. | 
| [Dataset](API_Dataset.md) |  |  No associated `DatasetImportJobs` can have a status of CREATE PENDING or IN PROGRESS\. No associated `Recommenders` can have a status of CREATE PENDING or IN PROGRESS\.  | 
| [DatasetSchema](API_DatasetSchema.md) | All datasets that reference the schema\. |  | 
| [DatasetGroup](API_DatasetGroup.md) |  All associated recommenders All datasets in the dataset group\.  | 

## Cleaning up custom resources<a name="cleaning-up-custom-resources"></a>

If you created a Custom dataset group, delete resources as follows:


| Resource to be deleted | Delete this first | Notes | 
| --- | --- | --- | 
| [Campaign](API_Campaign.md) |  |  | 
| [DatasetImportJob](API_DatasetImportJob.md) |  | Can not be deleted\. | 
| [EventTracker](API_EventTracker.md) |  | The event\-interactions dataset that is associated with the event tracker is not deleted and continues to be used by the solution version\. | 
| [Dataset](API_Dataset.md) |  |  No associated `DatasetImportJob` can have a status of CREATE PENDING or IN PROGRESS\. No associated `SolutionVersion` can have a status of CREATE PENDING or IN PROGRESS\.  | 
| [DatasetSchema](API_DatasetSchema.md) | All datasets that reference the schema\. |  | 
| [Solution](API_Solution.md) | All campaigns based on the solution version\. | No associated SolutionVersion can have a status of CREATE PENDING or IN PROGRESS\. | 
| [SolutionVersion](API_SolutionVersion.md) |  | Deleted when the associated Solution is deleted\. | 
| [DatasetGroup](API_DatasetGroup.md) |  All associated event trackers\. All associated solutions\. All datasets in the dataset group\.  |  | 