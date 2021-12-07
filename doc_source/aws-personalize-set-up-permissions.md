# Setting up permissions<a name="aws-personalize-set-up-permissions"></a>

 To use Amazon Personalize, you have to set up permissions that allow IAM users to access the Amazon Personalize console and API operations\. You also have to set up permissions that allow Amazon Personalize to perform tasks on your behalf and to access resources that you own\. 

We recommend creating an AWS Identity and Access Management \(IAM\) user with access restricted to Amazon Personalize operations\. You can add other permissions as needed\. For more information, see [Amazon Personalize identity\-based policies](security_iam_service-with-iam.md#security_iam_service-with-iam-id-based-policies)\. 

**Note**  
 We recommend [creating a new IAM policy](#set-up-required-permissions) that grants only the permissions necessary to use Amazon Personalize\. 

**To set up permissions**

1.  Attach a policy to your Amazon Personalize IAM user or group that allows full access to Amazon Personalize\. 
   +  Create a new IAM policy and attach it to your IAM user or group \(see [Creating a new IAM policy](#set-up-required-permissions)\)\. 

     **or**
   + Attach the `AmazonPersonalizeFullAccess` AWS managed policy to your IAM user or group \(see [AWS managed policies](security_iam_id-based-policy-examples.md#using-managed-policies)\)\.

1.  Attach the `AmazonS3FullAccess` AWS managed policy to your user or group to grant permissions to access Amazon S3 and create an Amazon S3 bucket\. For more information on granting permission to your Amazon S3 resources see [Using bucket policies and user policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the *Amazon S3 Developer Guide*\.

1.  Optionally attach the `CloudWatchFullAccess` AWS managed policy to your IAM user or group to grant permissions to monitor Amazon Personalize with CloudWatch\. See [AWS managed policies](security_iam_id-based-policy-examples.md#using-managed-policies)\. 

1.  Create an IAM role for Amazon Personalize and attach the policy from step 1 to the new role\. See [Creating an IAM service role for Amazon Personalize](#set-up-create-role-with-permissions)\. 

1. If you are using AWS Key Management Service \(AWS KMS\) for encryption, you must give your IAM user and Amazon Personalize IAM service\-linked role permission to use your key\. You must also add Amazon Personalize as a Principle in your AWS KMS key policy\. For more information see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.

## Creating a new IAM policy<a name="set-up-required-permissions"></a>

Create an IAM policy that provides users and Amazon Personalize full access to your Amazon Personalize resources\. Then attach the policy to your IAM user or group\. 

**To create and attach an IAM policy**

1. Sign in to the IAM console \([https://console\.aws\.amazon\.com/iam](https://console.aws.amazon.com/iam)\)\. 

1. In the navigation pane, choose **Policies**\. 

1. Choose **Create policy**\. 

1. Choose the **JSON** tab\. 

1.  Paste following JSON policy document in the text field\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "personalize:*"
               ],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "iam:PassRole"
               ],
               "Resource": "*",
               "Condition": {
                   "StringEquals": {
                       "iam:PassedToService": "personalize.amazonaws.com"
                   }
               }
           }
       ]
   }
   ```

1. Choose **Next: Tags**\. Optionally add any tags and choose **Review**\.

1. On the **Review policy** page, for **Name**, enter a name for the policy\. Optionally, enter a description for **Description**\. 

1. In **Summary**, review the policy to see the permissions it grants, then choose **Create policy**\.

1.  Attach the new policy to your IAM user or group\. 

   For information on attaching a policy to a user, see [Changing permissions for an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\. For information on attaching a policy to a group, see [Attaching a policy to an IAM group](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_attach-policy.html) in the *IAM User Guide*\. 

1. If you are using AWS KMS for encryption, give your user or group permission to use your key\. For more information see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\. 

## Creating an IAM service role for Amazon Personalize<a name="set-up-create-role-with-permissions"></a>

 To use Amazon Personalize, you must create an AWS Identity and Access Management service role for Amazon Personalize\. For information on how to create an IAM service role, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\. As you create your role, configure the following for Amazon Personalize: 
+ For **Choose the service that will use this role**, choose `Personalize`\.
+ For **Attach permissions policies**, either choose the policy you created in [Creating a new IAM policy](#set-up-required-permissions) or choose `AmazonPersonalizeFullAccess` \(see [AWS managed policies](security_iam_id-based-policy-examples.md#using-managed-policies)\)\.
+ If you are using AWS KMS for encryption, give your Amazon Personalize service\-linked role permission to use your key\. For more information see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\. 

Next, if you are completing the getting started exercise, you are ready create your training data and grant Amazon Personalize access to your Amazon S3 bucket\. See [Creating the training data \(custom resources\)](gs-prerequisites.md#gs-upload-to-bucket)\. 

If you are not completing the getting started exercise, you are ready to import your data\. See [Preparing and importing data](data-prep.md)\. 