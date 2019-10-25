# Getting Started<a name="getting-started"></a>

This getting started guide shows you how to create a campaign that returns movie recommendations for a user, based on historical data that consists of 100,000 movie ratings on 9,700 movies from 600 users\.

**To simplify this guide:**
+ We rely on the fact that a user saw a movie and not on what they rated the movie\. This simplifies the preparation of the training data\.
+ We don't record live user interaction events\. For information on capturing user events, see [Recording Events](recording-events.md)\.

To begin, download and prepare the training data\. Next, create an AWS Identity and Access Management \(IAM\) role that allows Amazon Personalize to access the data on your behalf\. After creating the training data and role, proceed to either [Getting Started \(Console\)](getting-started-console.md) or [Getting Started \(AWS CLI\)](getting-started-cli.md)\.

When you finish the getting started exercise, you might want to delete the resources you created\. For more information, see [Clean Up Resources](#gs-cleanup)\.

**Topics**
+ [Prerequisites](#gs-prerequisites)
+ [Create the Training Data](#gs-upload-to-bucket)
+ [Getting Started \(Console\)](getting-started-console.md)
+ [Getting Started \(AWS CLI\)](getting-started-cli.md)
+ [Getting Started \(AWS SDK for Python\)](getting-started-python.md)
+ [Clean Up Resources](#gs-cleanup)

## Prerequisites<a name="gs-prerequisites"></a>

The following steps are prerequisites for the getting started exercises\.
+ Create an AWS account and an AWS Identity and Access Management user, as specified in [Sign Up for AWS](setup.md#aws-personalize-set-up-aws-account)\.
+ Ensure the IAM user that you are using has the [Required Permissions](aws-personalize-set-up-permissions.md#set-up-required-permissions)\.
+ Create an AWS Identity and Access Management \(IAM\) service role, as specified in [Creating an IAM Role](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\. Use the role ARN when you upload the movie training data\.
+ Prepare your training data and upload the data to your Amazon S3 bucket, as specified in [Create the Training Data](#gs-upload-to-bucket)\. Use the name of the Amazon S3 bucket when you upload the movie training data\.

## Create the Training Data<a name="gs-upload-to-bucket"></a>

To create training data, download, modify, and save the movie ratings data to an Amazon Simple Storage Service \(Amazon S3\) bucket\. Then give Amazon Personalize permission to read from the bucket\.

1. Download the movie ratings zip file, [ml\-latest\-small\.zip](http://files.grouplens.org/datasets/movielens/ml-latest-small.zip) from [MovieLens](https://grouplens.org/datasets/movielens) \(under *recommended for education and development*\)\. Unzip the file\. The user\-interactions data is in the file named `ratings.csv`\.

1. Open the `ratings.csv` file\.

   1. Delete the *rating* column\.

   1. Replace the header row with the following:

      **USER\_ID,ITEM\_ID,TIMESTAMP**

      These headers must be exactly as shown for Amazon Personalize to recognize the data\.

   Save the `ratings.csv` file\.

1. Upload `ratings.csv` to your Amazon S3 bucket\. For more information, see [Uploading Files and Folders by Using Drag and Drop](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) in the Amazon Simple Storage Service Console User Guide\.

1. Grant Amazon Personalize permission to read the data in the bucket\. For more information, see [Uploading to an S3 Bucket](data-prep-upload-s3.md)\.

## Clean Up Resources<a name="gs-cleanup"></a>

To avoid incurring unnecessary charges, delete the resources you created after you're done with the getting started exercise\. To delete the resources, use either the Amazon Personalize console or the `Delete` APIs from the SDKs or the AWS Command Line Interface \(AWS CLI\)\. For example, use the [DeleteCampaign](API_DeleteCampaign.md) API to delete a campaign\.

You can't delete a resource whose status is CREATE PENDING or IN PROGRESS\. The resource status must be ACTIVE or CREATE FAILED\. Check the status using the `Describe` APIs, for example, [DescribeCampaign](API_DescribeCampaign.md)\.

Some resources must be deleted before others, as shown in the following table\. This process can take some time\.

To delete the training data you uploaded, `ratings.csv`, see [How Do I Delete Objects from an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-objects.html)\.


| Resource to be Deleted | Delete This First | Notes | 
| --- | --- | --- | 
| [Campaign](API_Campaign.md) |  |  | 
| [DatasetImportJob](API_DatasetImportJob.md) |  | Can not be deleted\. | 
| [EventTracker](API_EventTracker.md) |  | The event\-interactions dataset that is associated with the event tracker is not deleted and continues to be used by the solution version\. | 
| [Dataset](API_Dataset.md) |  |  No associated `DatasetImportJob` can have a status of CREATE PENDING or IN PROGRESS\. No associated `SolutionVersion` can have a status of CREATE PENDING or IN PROGRESS\.  | 
| [DatasetSchema](API_DatasetSchema.md) | All datasets that reference the schema\. |  | 
| [Solution](API_Solution.md) | All campaigns based on the solution version\. | No associated SolutionVersion can have a status of CREATE PENDING or IN PROGRESS\. | 
| [SolutionVersion](API_SolutionVersion.md) |  | Deleted when the associated Solution is deleted\. | 
| [DatasetGroup](API_DatasetGroup.md) |  All associated event trackers\. All associated solutions\. All datasets in the dataset group\.  |  | 