# Uploading to an Amazon S3 bucket<a name="data-prep-upload-s3"></a>

 After you format your historical input data \(see [Formatting your input data](data-prep-formatting.md)\), you must upload the CSV file to an Amazon S3 bucket and give Amazon Personalize permission to access to your Amazon S3 resources: 

1. If you haven't already, follow the steps in [Setting up permissions](aws-personalize-set-up-permissions.md) to set up permissions so your IAM users can access Amazon Personalize and Amazon Personalize can access your resources\.

1. Upload your CSV files to an Amazon Simple Storage Service \( Amazon S3\) bucket\. This is the location that Amazon Personalize imports your data from\. For more information, see [Uploading Files and Folders by Using Drag and Drop](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) in the Amazon Simple Storage Service User Guide\.

1. Give Amazon Personalize access to your Amazon S3 resources by attaching access policies to your Amazon S3 bucket and Amazon Personalize service role\. See [Giving Amazon Personalize access to Amazon S3 resources](granting-personalize-s3-access.md)\.

    Amazon S3 buckets and objects must be either encryption free or, if you are using AWS Key Management Service \(AWS KMS\) for encryption, you must grant Amazon Personalize and your Amazon Personalize IAM service role permission to use your key\. For more information, see [Giving Amazon Personalize permission to use your AWS KMS key](granting-personalize-key-access.md)\.

After you upload your data to an Amazon S3 bucket and give Amazon Personalize access to Amazon S3, import your data into Amazon Personalize\. See [ Step 3: Importing your historical data](data-prep-importing.md)\. 