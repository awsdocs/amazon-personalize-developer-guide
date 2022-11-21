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

1.  Optionally attach the `CloudWatchFullAccess` AWS managed policy to your IAM user or group to grant permissions to monitor Amazon Personalize with CloudWatch\. See [AWS managed policies](security_iam_id-based-policy-examples.md#using-managed-policies)\. 

1.  Create an IAM role for Amazon Personalize and attach the policy from step 1 to the new role\. See [Creating an IAM role for Amazon Personalize](#set-up-create-role-with-permissions)\. 

1. If you use AWS Key Management Service \(AWS KMS\) for encryption, you must grant Amazon Personalize and your Amazon Personalize IAM service role decrypt permissions in your key policy\. For more information, see [Giving Amazon Personalize permission to use your AWS KMS key](granting-personalize-key-access.md)\.

1.  Complete the steps in [Giving Amazon Personalize access to Amazon S3 resources](granting-personalize-s3-access.md) to use IAM and Amazon S3 bucket policies to give Amazon Personalize access to your Amazon S3 resources\. 

## Creating a new IAM policy<a name="set-up-required-permissions"></a>

Create an IAM policy that provides users and Amazon Personalize full access to your Amazon Personalize resources\. Then attach the policy to your IAM user or group\. 

**To use the JSON policy editor to create a policy**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation column on the left, choose **Policies**\. 

   If this is your first time choosing **Policies**, the **Welcome to Managed Policies** page appears\. Choose **Get Started**\.

1. At the top of the page, choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Enter the following JSON policy document:

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

1. Choose **Review policy**\.
**Note**  
You can switch between the **Visual editor** and **JSON** tabs any time\. However, if you make changes or choose **Review policy** in the **Visual editor** tab, IAM might restructure your policy to optimize it for the visual editor\. For more information, see [Policy restructuring](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_policies.html#troubleshoot_viseditor-restructure) in the *IAM User Guide*\.

1. On the **Review policy** page, enter a **Name** and an optional **Description** for the policy that you are creating\. Review the policy **Summary** to see the permissions that are granted by your policy\. Then choose **Create policy** to save your work\.

## Creating an IAM role for Amazon Personalize<a name="set-up-create-role-with-permissions"></a>

 To use Amazon Personalize, you must create an AWS Identity and Access Management service role for Amazon Personalize\. For information on how to create an IAM role, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\. As you create your role, configure the following for Amazon Personalize: 
+ For **Choose the service that will use this role**, choose `Personalize`\.
+ For **Attach permissions policies**, either choose the policy you created in [Creating a new IAM policy](#set-up-required-permissions) or choose `AmazonPersonalizeFullAccess`\.
+  If you use AWS Key Management Service \(AWS KMS\) for encryption, you must grant Amazon Personalize and your Amazon Personalize IAM service role decrypt permissions in your key policy\. For more information, see [Giving Amazon Personalize permission to use your AWS KMS key](granting-personalize-key-access.md)\. 
+  To prevent the [confused deputy problem](cross-service-confused-deputy-prevention.md), in the role's trust relationship policy use `aws:SourceArn` and `aws:SourceAccount` global condition context keys\. For a trust relationship policy example, see [Cross\-service confused deputy prevention](cross-service-confused-deputy-prevention.md)\.

   For information modifying an IAM role's trust policy, see [Modifying a role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html)\. 

Next, if you are completing the getting started exercise, you are ready create your training data and grant Amazon Personalize access to your Amazon S3 bucket\. See [Creating the training data \(Custom dataset group\)](gs-prerequisites.md#gs-upload-to-bucket)\. 

If you are not completing the getting started exercise, you are ready to import your data\. See [Preparing and importing data](data-prep.md)\. 