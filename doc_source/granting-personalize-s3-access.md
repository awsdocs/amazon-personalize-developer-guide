# Giving Amazon Personalize access to Amazon S3 resources<a name="granting-personalize-s3-access"></a>

To give Amazon Personalize access to your Amazon S3 bucket, do the following:

1. If you haven't already, follow the steps in [Setting up permissions](aws-personalize-set-up-permissions.md) to set up permissions so your IAM users can access Amazon Personalize and Amazon Personalize can access your resources\.

1.  Attach a policy to the Amazon Personalize service role \(see [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\) that allows access to your Amazon S3 bucket\. For more information, see [Attaching an Amazon S3 policy to your Amazon Personalize service role](#attaching-s3-policy-to-role)\. 

1.  Attach a bucket policy to the Amazon S3 bucket containing your data files so Amazon Personalize can access them\. For more information, see [Attaching an Amazon Personalize access policy to your Amazon S3 bucket](#attach-bucket-policy)\. 

1. If you are using AWS Key Management Service \(AWS KMS\) for encryption, you must give your IAM user and Amazon Personalize IAM service role permission to use your key\. You must also add Amazon Personalize as a Principle in your AWS KMS key policy\. For more information see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.

**Note**  
Because Amazon Personalize doesn’t communicate with AWS VPCs, Amazon Personalize can't interact with Amazon S3 buckets that allow only VPC access\.

**Topics**
+ [Attaching an Amazon S3 policy to your Amazon Personalize service role](#attaching-s3-policy-to-role)
+ [Attaching an Amazon Personalize access policy to your Amazon S3 bucket](#attach-bucket-policy)

## Attaching an Amazon S3 policy to your Amazon Personalize service role<a name="attaching-s3-policy-to-role"></a>

To attach an Amazon S3 policy to your Amazon Personalize role do the following:

1. Sign in to the IAM console \([https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\)\.

1. In the navigation pane, choose **Policies**, and choose **Create policy**\.

1. Choose the JSON tab, and update the policy as follows\. Replace `bucket-name` with the name of your bucket\. If you are using a batch workflow, Amazon Personalize needs additional permissions\. See [Service\-linked role policy for batch workflows](#role-policy-for-batch-workflows)\. 

   ```
   {
       "Version": "2012-10-17",
       "Id": "PersonalizeS3BucketAccessPolicy",
       "Statement": [
           {
               "Sid": "PersonalizeS3BucketAccessPolicy",
               "Effect": "Allow",
               "Action": [
                   "s3:GetObject",
                   "s3:ListBucket"
               ],
               "Resource": [
                   "arn:aws:s3:::bucket-name",
                   "arn:aws:s3:::bucket-name/*"
               ]
           }
       ]
   }
   ```

1. Choose **Next: Tags**\. Optionally add any tags and choose **Review**\.

1. Give the policy a name\.

1. \(Optional\) For **Description**, enter a short sentence describing this policy, for example, **Allow Amazon Personalize to access its Amazon S3 bucket\.**

1. Choose **Create policy**\.

1. In the navigation pane, choose **Roles**, and choose the role you created for Amazon Personalize\. See [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\.

1. For **Permissions**, choose **Attach policies**\.

1. To display the policy in the list, type part of the policy name in the **Filter policies** filter box\.

1. Choose the check box next to the policy you created earlier in this procedure\.

1. Choose **Attach policy**\.

   Before your role is ready for use with Amazon Personalize you must also attach a bucket policy to the Amazon S3 bucket containing your data\. See [Attaching an Amazon Personalize access policy to your Amazon S3 bucket](#attach-bucket-policy)\.

### Service\-linked role policy for batch workflows<a name="role-policy-for-batch-workflows"></a>

To complete a batch worklfow, Amazon Personalize needs permission to access and add files to your Amazon S3 bucket\. Follow the steps above to attach the following policy to your Amazon Personalize role\. Replace `bucket-name` with the name of your bucket\. For more information on batch workflows, see [Getting batch recommendations and user segments](recommendations-batch.md)\.

```
{
    "Version": "2012-10-17",
    "Id": "PersonalizeS3BucketAccessPolicy",
    "Statement": [
        {
            "Sid": "PersonalizeS3BucketAccessPolicy",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::bucket-name",
                "arn:aws:s3:::bucket-name/*"
            ]
        }
    ]
}
```

### Service\-linked role policy for exporting a dataset<a name="role-policy-for-export"></a>

To export a dataset, your Amazon Personalize service\-linked role needs permission to use the `PutObject` and `ListBucket` Actions on your Amazon S3 bucket\. The following example policy grants Amazon Personalize `PutObject` and `ListBucket` permissions\. Replace `bucket-name` with the name of your bucket and attach the policy to your service\-linked role\. For information about attaching policies to a service\-linked IAM role see [Attaching an Amazon S3 policy to your Amazon Personalize service role](#attaching-s3-policy-to-role)\. 

```
{
    "Version": "2012-10-17",
    "Id": "PersonalizeS3BucketAccessPolicy",
    "Statement": [
        {
            "Sid": "PersonalizeS3BucketAccessPolicy",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucket-name",
                "arn:aws:s3:::bucket-name/*"
            ]
        }
    ]
}
```

## Attaching an Amazon Personalize access policy to your Amazon S3 bucket<a name="attach-bucket-policy"></a>

Amazon Personalize needs permission to access the S3 bucket\. For non\-batch workflows, attach the following policy to your bucket\. Replace `bucket-name` with the name of your bucket\. For batch workflows, see [Amazon S3 bucket policy for batch workflows](#bucket-policy-for-batch-workflows)\. 

For more information on Amazon S3 bucket policies, see [How Do I Add an S3 Bucket Policy?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-bucket-policy.html)\. 

```
{
    "Version": "2012-10-17",
    "Id": "PersonalizeS3BucketAccessPolicy",
    "Statement": [
        {
            "Sid": "PersonalizeS3BucketAccessPolicy",
            "Effect": "Allow",
            "Principal": {
                "Service": "personalize.amazonaws.com"
            },
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucket-name",
                "arn:aws:s3:::bucket-name/*"
            ]
        }
    ]
}
```

### Amazon S3 bucket policy for batch workflows<a name="bucket-policy-for-batch-workflows"></a>

For batch workflows, Amazon Personalize needs permission to access and add files to your Amazon S3 bucket\. Attach the following policy to your bucket\. Replace `bucket-name` with the name of your bucket\.

For more information on adding an Amazon S3 bucket policy to a bucket, see [How Do I Add an S3 Bucket Policy?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-bucket-policy.html)\. For more information on batch workflows, see [Getting batch recommendations and user segments](recommendations-batch.md)\.

```
{
    "Version": "2012-10-17",
    "Id": "PersonalizeS3BucketAccessPolicy",
    "Statement": [
        {
            "Sid": "PersonalizeS3BucketAccessPolicy",
            "Effect": "Allow",
            "Principal": {
                "Service": "personalize.amazonaws.com"
            },
            "Action": [
                "s3:GetObject",
                "s3:ListBucket",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::bucket-name",
                "arn:aws:s3:::bucket-name/*"
            ]
        }
    ]
}
```

### Amazon S3 bucket policy for exporting a dataset<a name="bucket-policy-for-export"></a>

To export a dataset, Amazon Personalize needs permission to use the `PutObject` and `ListBucket` Actions on your Amazon S3 bucket\. The following example policy grants the Amazon Personalize principle `PutObject` and `ListBucket` permissions\. Replace `bucket-name` with the name of your bucket and attach the policy to your bucket\. For information on adding an Amazon S3 bucket policy to a bucket, see [How Do I Add an S3 Bucket Policy?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-bucket-policy.html) in the Amazon Simple Storage Service User Guide\. 

```
{
    "Version": "2012-10-17",
    "Id": "PersonalizeS3BucketAccessPolicy",
    "Statement": [
        {
            "Sid": "PersonalizeS3BucketAccessPolicy",
            "Effect": "Allow",
            "Principal": {
                "Service": "personalize.amazonaws.com"
            },
            "Action": [
                "s3:PutObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucket-name",
                "arn:aws:s3:::bucket-name/*"
            ]
        }
    ]
}
```