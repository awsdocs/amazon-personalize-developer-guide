# Setting up Permissions<a name="aws-personalize-set-up-permissions"></a>

To use Amazon Personalize, you have to set up permissions that allow access to the Amazon Personalize console and API operations\. You also have to allow Amazon Personalize to perform tasks on your behalf and to access resources that you own\.

We recommend creating a user with access restricted to Amazon Personalize operations\. You can add other permissions as needed\. For more information, see [Amazon Personalize Identity\-Based Policies](security_iam_service-with-iam.md#security_iam_service-with-iam-id-based-policies.title)\.

**Topics**
+ [Required Permissions](#set-up-required-permissions)
+ [Creating an IAM Role](#set-up-create-role-with-permissions)

## Required Permissions<a name="set-up-required-permissions"></a>

The following policies provide the required permissions to use Amazon Personalize\.

**AmazonPersonalizeFullAccess Policy**  
Allows you to perform the following actions:  
+ Access all Amazon Personalize resources
+ Publish and list metrics on Amazon CloudWatch
+ List, read, write, and delete all objects in an Amazon S3 bucket that contains `Personalize` or `personalize` in the bucket name
+ Pass a role to Amazon Personalize
For a step\-by\-step procedure for creating an IAM role that passes these permissions to Amazon Personalize, see [Creating an IAM Role](#set-up-create-role-with-permissions)\.

**Amazon S3 Bucket Policy**  
Allows Amazon Personalize access to the S3 bucket that contains your training data\. For more information, see [Uploading to an S3 Bucket](data-prep-upload-s3.md)\.

**CloudWatchFullAccess Policy \(Optional\)**  
The `AmazonPersonalizeFullAccess` policy provides permissions to publish and list Amazon Personalize metrics in CloudWatch\. The `CloudWatchFullAccess` policy adds additional permissions, such as viewing metrics, displaying metric statistics, and setting metric based alarms\. For more information, see [Monitoring Amazon Personalize](personalize-monitoring.md)\.

## Creating an IAM Role<a name="set-up-create-role-with-permissions"></a>

In the following procedure, you create an IAM role that allows Amazon Personalize to access your resources and perform tasks on your behalf\.

A user needs permission to create the IAM role\. To give a user permission, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)\.

1. Sign in to the IAM console \([https://console\.aws\.amazon\.com/iam](https://console.aws.amazon.com/iam)\)\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. For **Select type of trusted entity**, choose **AWS service**\.

1. For **Choose the service that will use this role**, select **Amazon Personalize**\. If you don't see **Amazon Personalize** listed, choose **EC2** with **EC2** as your use case\.

1. Choose **Next: Permissions**\.

1. For **Attach permissions policies**, choose *AmazonPersonalizeFullAccess*\.

   1. To display the policy in the list, type part of the policy name in the **Filter policies** query filter\.

   1. Choose the check box next to **AmazonPersonalizeFullAccess**\.

   1. \(Optional\) Repeat steps a\. & b\. for **CloudWatchFullAccess**\.

1. Choose **Next: Tags**\. You don't need to add any tags\. Choose **Next: Review**\.

1. In the **Review** section, for **Role name**, enter a name for the role \(for example, `PersonalizeRole`\)\. Update the description for the role in **Role description**, then choose **Create role**\.

1. Choose the new role to open the role's summary page\.

1. Copy the **Role ARN** value and save it\. You need it in order to import a dataset into Amazon Personalize\.

1. If you didn't choose **Amazon Personalize** as the service that will use this role, perform the following additional steps\.

   1. Choose **Trust relationships**\.

   1. Choose **Edit trust relationship** and update the trust policy to match the following\.

      ```
      {
          "Version": "2012-10-17",
          "Statement": [
              {
                  "Effect": "Allow",
                  "Principal": {
                      "Service": "personalize.amazonaws.com"
                  },
                  "Action": "sts:AssumeRole"
              }
          ]
      }
      ```

   1. Choose **Update Trust Policy**\.

1. In the navigation pane, choose **Policies**, and choose **Create policy**\.

1. Choose the JSON tab, and update the policy as follows\.

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

1. Choose **Review policy**\.

1. For **Name**, enter `PersonalizeS3BucketAccessPolicy`\.

1. \(Optional\) For **Description**, enter a short sentence describing this policy, for example, Allow **Amazon Personalize to access its S3 bucket**\.

1. Choose **Create policy**\.

1. In the navigation pane, choose **Roles**, and choose the new role\.

1. For **Permissions**, choose **PersonalizeS3BucketAccessPolicy**\.

   1. To display the policy in the list, type part of the policy name in the **Filter policies** filter box\.

   1. Choose the check box next to **PersonalizeS3BucketAccessPolicy**\.

   1. Choose **Attach policy**\.

Your role is now ready for use with Amazon Personalize\. Record the role ARN\. You will need it for import jobs\.