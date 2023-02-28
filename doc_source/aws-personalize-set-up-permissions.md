# Setting up permissions<a name="aws-personalize-set-up-permissions"></a>

 You must give users, groups, or roles permission to interact with Amazon Personalize resources\. And you must give Amazon Personalize permission to access the resources you create in Amazon Personalize and to perform tasks on your behalf\. 

**To set up permissions**

1. Give your users, groups, or roles permission to interact with Amazon Personalize resources and pass a role to Amazon Personalize\. See [Giving users permission to access Amazon Personalize](#grant-user-permissions)\.

1.  Give Amazon Personalize permission to access your resources in Amazon Personalize and permission to perform tasks on your behalf\. See [Giving Amazon Personalize permission to access your Amazon Personalize resources](#set-up-required-permissions)\. 

1.  Modify your Amazon Personalize service role's trust policy so it prevents the [confused deputy problem](cross-service-confused-deputy-prevention.md)\. For a trust relationship policy example, see [Cross\-service confused deputy prevention](cross-service-confused-deputy-prevention.md)\. For information modifying a role's trust policy, see [Modifying a role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html)\. 

1. If you use AWS Key Management Service \(AWS KMS\) for encryption, you must grant Amazon Personalize and your Amazon Personalize IAM service role permission to use your key\. For more information, see [Giving Amazon Personalize permission to use your AWS KMS key](granting-personalize-key-access.md)\.

1.  Complete the steps in [Giving Amazon Personalize access to Amazon S3 resources](granting-personalize-s3-access.md) to use IAM and Amazon S3 bucket policies to give Amazon Personalize access to your Amazon S3 resources\. 

**Topics**
+ [Giving users permission to access Amazon Personalize](#grant-user-permissions)
+ [Giving Amazon Personalize permission to access your Amazon Personalize resources](#set-up-required-permissions)
+ [Giving Amazon Personalize access to Amazon S3 resources](granting-personalize-s3-access.md)
+ [Giving Amazon Personalize permission to use your AWS KMS key](granting-personalize-key-access.md)

## Giving users permission to access Amazon Personalize<a name="grant-user-permissions"></a>

 To provide your users access to Amazon Personalize, you create an IAM policy that grants permission to access your Amazon Personalize resources and pass a role to Amazon Personalize\. Then you use that policy when you add permissions to your users, groups or roles\. 

### Creating a new IAM policy for your users<a name="create-policy-for-users"></a>

Create an IAM policy that provides Amazon Personalize full access to your Amazon Personalize resources\. 

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

To grant only the permissions required to perform a task in Amazon Personalize, modify the preceding policy to include only the required actions for your user\. For a complete list of Amazon Personalize actions, see [Actions, resources, and condition keys for Amazon Personalize](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonpersonalize.html)\.

### Providing access to Amazon Personalize<a name="attach-policy-to-user"></a>

Attach the new IAM policy when you provide permissions to your users\.

To provide access, add permissions to your users, groups, or roles:
+ Users and groups in AWS IAM Identity Center \(successor to AWS Single Sign\-On\):

  Create a permission set\. Follow the instructions in [Create a permission set](https://docs.aws.amazon.com/singlesignon/latest/userguide/howtocreatepermissionset.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.
+ Users managed in IAM through an identity provider:

  Create a role for identity federation\. Follow the instructions in [Creating a role for a third\-party identity provider \(federation\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp.html) in the *IAM User Guide*\.
+ IAM users:
  + Create a role that your user can assume\. Follow the instructions in [Creating a role for an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html) in the *IAM User Guide*\.
  + \(Not recommended\) Attach a policy directly to a user or add a user to a user group\. Follow the instructions in [Adding permissions to a user \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*\.

## Giving Amazon Personalize permission to access your Amazon Personalize resources<a name="set-up-required-permissions"></a>

 To give Amazon Personalize permission to access your resources, you create an IAM policy that provides Amazon Personalize full access to your Amazon Personalize resources\. Then you create an IAM role for Amazon Personalize and attach the new policy to it\. 

**Topics**
+ [Creating a new IAM policy for Amazon Personalize](#create-role-policy)
+ [Creating an IAM role for Amazon Personalize](#set-up-create-role-with-permissions)

### Creating a new IAM policy for Amazon Personalize<a name="create-role-policy"></a>

Create an IAM policy that provides Amazon Personalize full access to your Amazon Personalize resources\.

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
           }
       ]
   }
   ```

1. Choose **Review policy**\.
**Note**  
You can switch between the **Visual editor** and **JSON** tabs any time\. However, if you make changes or choose **Review policy** in the **Visual editor** tab, IAM might restructure your policy to optimize it for the visual editor\. For more information, see [Policy restructuring](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_policies.html#troubleshoot_viseditor-restructure) in the *IAM User Guide*\.

1. On the **Review policy** page, enter a **Name** and an optional **Description** for the policy that you are creating\. Review the policy **Summary** to see the permissions that are granted by your policy\. Then choose **Create policy** to save your work\.

### Creating an IAM role for Amazon Personalize<a name="set-up-create-role-with-permissions"></a>

 To use Amazon Personalize, you must create an AWS Identity and Access Management service role for Amazon Personalize\.  A service role is an [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) that a service assumes to perform actions on your behalf\. An IAM administrator can create, modify, and delete a service role from within IAM\. For more information, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\. \. After you create a service role for Amazon Personalize, grant the role additional permissions listed in [Additional service role permissions](#additional-service-role-permissions) as necessary\. 

**To create the service role for Amazon Personalize \(IAM console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane of the IAM console, choose **Roles**, and then choose **Create role**\.

1. Choose the **AWS service** role type, and then choose Amazon Personalize\.

1. Choose the Personalize use case\. Then, choose **Next**\.

1. Chose the policy that you created in the previous procedure\.

1. \(Optional\) Set a [permissions boundary](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html)\. This is an advanced feature that is available for service roles, but not service\-linked roles\. 

   Expand the **Permissions boundary** section and choose **Use a permissions boundary to control the maximum role permissions**\. IAM includes a list of the AWS managed and customer managed policies in your account\. Select the policy to use for the permissions boundary or choose **Create policy** to open a new browser tab and create a new policy from scratch\. For more information, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-start) in the *IAM User Guide*\. After you create the policy, close that tab and return to your original tab to select the policy to use for the permissions boundary\.

1. Choose **Next**\.

1. If possible, enter a role name or role name suffix to help you identify the purpose of this role\. Role names must be unique within your AWS account\. They are not distinguished by case\. For example, you cannot create roles named both **PRODROLE** and **prodrole**\. Because various entities might reference the role, you cannot edit the name of the role after it has been created\.

1. \(Optional\) For **Description**, enter a description for the new role\.

1. Choose **Edit** in the **Step 1: Select trusted entities** or **Step 2: Select permissions** sections to edit the use cases and permissions for the role\. 

1. \(Optional\) Add metadata to the user by attaching tags as key\-value pairs\. For more information about using tags in IAM, see [Tagging IAM resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*\.

1. Review the role and then choose **Create role**\.

#### Additional service role permissions<a name="additional-service-role-permissions"></a>

After you create the role and grant it permissions to access your resources in Amazon Personalize, do the following:

1.  Modify your Amazon Personalize service role's trust policy so it prevents the [confused deputy problem](cross-service-confused-deputy-prevention.md)\. For a trust relationship policy example, see [Cross\-service confused deputy prevention](cross-service-confused-deputy-prevention.md)\. For information modifying a role's trust policy, see [Modifying a role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html)\. 

1.  If you use AWS Key Management Service \(AWS KMS\) for encryption, you must grant Amazon Personalize and your Amazon Personalize IAM service role permission to use your key\. For more information, see [Giving Amazon Personalize permission to use your AWS KMS key](granting-personalize-key-access.md)\. 