# Getting started prerequisites<a name="gs-prerequisites"></a>

The following steps are prerequisites for the getting started exercises\.

1. Create an AWS account and an AWS Identity and Access Management user, as specified in [Sign up for AWS](setup.md#aws-personalize-set-up-aws-account)\. 

1. Create an IAM policy that provides users and Amazon Personalize full access to your Amazon Personalize resources\. Then attach the policy to your Amazon Personalize user or group\. See [Creating a new IAM policy](aws-personalize-set-up-permissions.md#set-up-required-permissions)\. 

1. Create an AWS Identity and Access Management \(IAM\) service role, as specified in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\. Use the role ARN when you upload the movie training data\. 

1. Prepare your training data and upload the data to your Amazon S3 bucket: 
   +  For Domain dataset group tutorials, see [Creating the training data \(Domain dataset group\)](#gs-data-prep-domain) 
   +  For Custom dataset group tutorials, see [Creating the training data \(Custom dataset group\)](#gs-upload-to-bucket)\. 

1.  Give your Amazon Personalize service role permission to access your Amazon S3 resources, as specified in [Giving Amazon Personalize access to Amazon S3 resources](granting-personalize-s3-access.md)\. 

## Creating the training data \(Domain dataset group\)<a name="gs-data-prep-domain"></a>

To create training data, download, modify, and save the movie ratings data to an Amazon Simple Storage Service \(Amazon S3\) bucket\. Then give Amazon Personalize permission to read from the bucket\.

**To create the training data**

1. Download the movie ratings zip file, [ml\-latest\-small\.zip](http://files.grouplens.org/datasets/movielens/ml-latest-small.zip) from [MovieLens](https://grouplens.org/datasets/movielens) \(under *recommended for education and development*\)\. Unzip the file\. The user\-interactions data is in the file named `ratings.csv`\.

1. Open the `ratings.csv` file and modify the data as follows:

   1. Delete the *rating* column\.

   1. Rename the `userId` and `movieId` columns to `USER_ID` and `ITEM_ID` respectively\.

   1. Add an EVENT\_TYPE column set the value for every record to `watch`\. If you're using Microsoft Excel, you can set the EVENT\_TYPE for every record by entering `watch` in the first cell in the column and then double\-clicking the bottom\-right corner of the cell\. Your header should be the following:

      **USER\_ID,ITEM\_ID,TIMESTAMP,EVENT\_TYPE**

      These columns must be exactly as shown for Amazon Personalize to recognize the data\. The first few rows of your data should look as follows:

      ```
      USER_ID,ITEM_ID,TIMESTAMP,EVENT_TYPE
      1,1,964982703,watch
      1,3,964981247,watch
      1,6,964982224,watch
      1,47,964983815,watch
      1,50,964982931,watch
      ....
      ....
      ```

   Save the `ratings.csv` file\.

1. Upload `ratings.csv` to your Amazon S3 bucket\. For more information, see [Uploading files and folders by using drag and drop](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) in the Amazon Simple Storage Service User Guide\.

1. Give Amazon Personalize permission to read the data in the bucket\. For more information, see [Giving Amazon Personalize access to Amazon S3 resources](granting-personalize-s3-access.md)\.

## Creating the training data \(Custom dataset group\)<a name="gs-upload-to-bucket"></a>

To create training data, download, modify, and save the movie ratings data to an Amazon Simple Storage Service \(Amazon S3\) bucket\. Then give Amazon Personalize permission to read from the bucket\.

1. Download the movie ratings zip file, [ml\-latest\-small\.zip](http://files.grouplens.org/datasets/movielens/ml-latest-small.zip) from [MovieLens](https://grouplens.org/datasets/movielens) \(under *recommended for education and development*\)\. Unzip the file\. The user\-interactions data is in the file named `ratings.csv`\.

1. Open the `ratings.csv` file\.

   1. Delete the *rating* column\.

   1. Replace the header row with the following:

      **USER\_ID,ITEM\_ID,TIMESTAMP**

      These headers must be exactly as shown for Amazon Personalize to recognize the data\.

   Save the `ratings.csv` file\.

1. Upload `ratings.csv` to your Amazon S3 bucket\. For more information, see [Uploading files and folders by using drag and drop](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) in the Amazon Simple Storage Service User Guide\.

1. Give Amazon Personalize permission to read the data in the bucket\. For more information, see [Giving Amazon Personalize access to Amazon S3 resources](granting-personalize-s3-access.md)\.