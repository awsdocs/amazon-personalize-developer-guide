# Uploading to an Amazon S3 Bucket<a name="data-prep-upload-s3"></a>

 After you format your historical input data \(see [Formatting Your Input Data](data-prep-formatting.md)\), you must upload the CSV file to an Amazon S3 bucket and give Amazon Personalize permission to access to your Amazon S3 resources: 

1. If you haven't already, follow the steps in [Setting Up Permissions](aws-personalize-set-up-permissions.md) to set up permissions so your IAM users can access Amazon Personalize and Amazon Personalize can access your resources\.

1. Upload your CSV files to an Amazon Simple Storage Service \( Amazon S3\) bucket\. This is the location that Amazon Personalize imports your data from\. For more information, see [Uploading Files and Folders by Using Drag and Drop](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) in the Amazon Simple Storage Service Console User Guide\.

1. Give Amazon Personalize access to your Amazon S3 resources by attaching access policies to your Amazon S3 bucket and Amazon Personalize service role\. See [Giving Amazon Personalize Access to Amazon S3 Resources](granting-personalize-s3-access.md)\.

**Note**  
 Amazon S3 buckets and objects must be either encryption free or, if you are using AWS Key Management Service \(AWS KMS\) for encryption, you must give your IAM user and Amazon Personalize IAM service role permission to use your key\. For more information see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\. 

After you upload your data to an Amazon S3 bucket and give Amazon Personalize access to Amazon S3, import your data into Amazon Personalize\. See [ Step 3: Importing Your Data](data-prep-importing.md)\. 