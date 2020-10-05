# Getting Started Prerequisites<a name="gs-prerequisites"></a>

The following steps are prerequisites for the getting started exercises\.

1. Create an AWS account and an AWS Identity and Access Management user, as specified in [Sign Up for AWS](setup.md#aws-personalize-set-up-aws-account)\. 

1. Create an IAM policy that provides users and Amazon Personalize full access to your Amazon Personalize resources\. Then attach the policy to your Amazon Personalize user or group\. See [Creating a New IAM Policy](aws-personalize-set-up-permissions.md#set-up-required-permissions)\. 

1. Create an AWS Identity and Access Management \(IAM\) service role, as specified in [Creating an IAM Role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\. Use the role ARN when you upload the movie training data\. 

1. Prepare your training data and upload the data to your Amazon S3 bucket, as specified in [Creating the Training Data](#gs-upload-to-bucket)\. Use the name of the Amazon S3 bucket when you upload the movie training data\. 

1.  Give your Amazon Personalize service role permission to access your Amazon S3 resources, as specified in [Giving Amazon Personalize Access to Amazon S3 Resources](data-prep-upload-s3.md#granting-personalize-s3-access)\. 

## Creating the Training Data<a name="gs-upload-to-bucket"></a>

To create training data, download, modify, and save the movie ratings data to an Amazon Simple Storage Service \(Amazon S3\) bucket\. Then give Amazon Personalize permission to read from the bucket\.

1. Download the movie ratings zip file, [ml\-latest\-small\.zip](http://files.grouplens.org/datasets/movielens/ml-latest-small.zip) from [MovieLens](https://grouplens.org/datasets/movielens) \(under *recommended for education and development*\)\. Unzip the file\. The user\-interactions data is in the file named `ratings.csv`\.

1. Open the `ratings.csv` file\.

   1. Delete the *rating* column\.

   1. Replace the header row with the following:

      **USER\_ID,ITEM\_ID,TIMESTAMP**

      These headers must be exactly as shown for Amazon Personalize to recognize the data\.

   Save the `ratings.csv` file\.

1. Upload `ratings.csv` to your Amazon S3 bucket\. For more information, see [Uploading Files and Folders by Using Drag and Drop](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) in the Amazon Simple Storage Service Console User Guide\.

1. Give Amazon Personalize permission to read the data in the bucket\. For more information, see [Giving Amazon Personalize Access to Amazon S3 Resources](data-prep-upload-s3.md#granting-personalize-s3-access)\.