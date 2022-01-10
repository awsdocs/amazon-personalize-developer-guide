# Amazon Personalize identity\-based policy examples<a name="security_iam_id-based-policy-examples"></a>

By default, IAM users and roles don't have permission to create or modify Amazon Personalize resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform specific API operations on the specified resources they need\. The administrator must then attach those policies to the IAM users or groups that require those permissions\.

To learn how to create an IAM identity\-based policy using these example JSON policy documents, see [Creating policies on the JSON tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [AWS managed policies](#using-managed-policies)
+ [Using the Amazon Personalize console](#security_iam_id-based-policy-examples-console)
+ [Allow users to view their own permissions](#security_iam_id-based-policy-examples-view-own-permissions)
+ [Allowing full access to Amazon Personalize resources](#security_iam_id-based-policy-examples-full-access)
+ [Allowing read\-only access to Amazon Personalize resources](#security_iam_id-based-policy-examples-read-only)

## Policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies are very powerful\. They determine whether someone can create, access, or delete Amazon Personalize resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started using AWS managed policies** – To start using Amazon Personalize quickly, use AWS managed policies to give your employees the permissions they need\. These policies are already available in your account and are maintained and updated by AWS\. For more information, see [Get started using permissions with AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-use-aws-defined-policies) in the *IAM User Guide*\.
+ **Grant least privilege** – When you create custom policies, grant only the permissions required to perform a task\. Start with a minimum set of permissions and grant additional permissions as necessary\. Doing so is more secure than starting with permissions that are too lenient and then trying to tighten them later\. For more information, see [Grant least privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *IAM User Guide*\.
+ **Enable MFA for sensitive operations** – For extra security, require IAM users to use multi\-factor authentication \(MFA\) to access sensitive resources or API operations\. For more information, see [Using multi\-factor authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.
+ **Use policy conditions for extra security** – To the extent that it's practical, define the conditions under which your identity\-based policies allow access to a resource\. For example, you can write conditions to specify a range of allowable IP addresses that a request must come from\. You can also write conditions to allow requests only within a specified date or time range, or to require the use of SSL or MFA\. For more information, see [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## AWS managed policies<a name="using-managed-policies"></a>

 AWS managed polices are policies that are created and managed by AWS\. The following are examples of AWS managed policies you can attach to your IAM user or group to grant permissions for Amazon Personalize\. 

 For information on attaching a policy to a user, see [Changing permissions for an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\. For information on attaching a policy to a group, see [Attaching a policy to an IAM group](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_attach-policy.html) in the *IAM User Guide*\. 

**AmazonPersonalizeFullAccess Policy**

Instead of creating a new policy, you can attach the AWS managed `AmazonPersonalizeFullAccess` policy to your IAM users and roles\. However, `AmazonPersonalizeFullAccess` provides more permissions than are necessary to use Amazon Personalize\. 

 Instead of using the `AmazonPersonalizeFullAccess` policy, we recommend creating a new IAM policy that only grants the necessary permissions \(see [Creating a new IAM policy](aws-personalize-set-up-permissions.md#set-up-required-permissions)\)\. 

The `AmazonPersonalizeFullAccess` policy allows IAM users to perform the following actions:
+ Access all Amazon Personalize resources
+ Publish and list metrics on Amazon CloudWatch
+ List, read, write, and delete all objects in an Amazon S3 bucket that contains `Personalize` or `personalize` in the bucket name
+ Pass a role to Amazon Personalize

 **CloudWatchFullAccess** 

 To give users permission to monitor Amazon Personalize with CloudWatch, attach the `CloudWatchFullAccess` policy to your Amazon Personalize IAM users or groups\. For more information, see [Monitoring Amazon Personalize](personalize-monitoring.md)\. 

 The `CloudWatchFullAccess` policy is optional and allows IAM users to perform the following actions: 
+ Publish and list Amazon Personalize metrics in CloudWatch
+  View metrics and metric statistics\. 
+  Set metric based alarms\. 

## Using the Amazon Personalize console<a name="security_iam_id-based-policy-examples-console"></a>

To access the Amazon Personalize console, you must have a minimum set of permissions\. These permissions must allow you to list and view details about the Amazon Personalize resources in your AWS account\. If you create an identity\-based policy that is more restrictive than the minimum required permissions, the console won't function as intended for entities \(IAM users or roles\) with that policy\. 

## Allow users to view their own permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

This example shows how you might create a policy that allows IAM users to view the inline and managed policies that are attached to their user identity\. This policy includes permissions to complete this action on the console or programmatically using the AWS CLI or AWS API\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewOwnUserInfo",
            "Effect": "Allow",
            "Action": [
                "iam:GetUserPolicy",
                "iam:ListGroupsForUser",
                "iam:ListAttachedUserPolicies",
                "iam:ListUserPolicies",
                "iam:GetUser"
            ],
            "Resource": ["arn:aws:iam::*:user/${aws:username}"]
        },
        {
            "Sid": "NavigateInConsole",
            "Effect": "Allow",
            "Action": [
                "iam:GetGroupPolicy",
                "iam:GetPolicyVersion",
                "iam:GetPolicy",
                "iam:ListAttachedGroupPolicies",
                "iam:ListGroupPolicies",
                "iam:ListPolicyVersions",
                "iam:ListPolicies",
                "iam:ListUsers"
            ],
            "Resource": "*"
        }
    ]
}
```

## Allowing full access to Amazon Personalize resources<a name="security_iam_id-based-policy-examples-full-access"></a>

The following example gives an IAM user in your AWS account full access to all Amazon Personalize resources and actions\.

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

## Allowing read\-only access to Amazon Personalize resources<a name="security_iam_id-based-policy-examples-read-only"></a>

In this example, you grant an IAM user in your AWS account read\-only access to your Amazon Personalize resources, including Amazon Personalize datasets, dataset groups, solutions, and campaigns\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "personalize:DescribeAlgorithm",
                "personalize:DescribeBatchInferenceJob",
                "personalize:DescribeCampaign",
                "personalize:DescribeDataset",
                "personalize:DescribeDatasetGroup",
                "personalize:DescribeDatasetImportJob",
                "personalize:DescribeDatasetImportJobRun",
                "personalize:DescribeEventTracker",
                "personalize:DescribeFeatureExportJob",
                "personalize:DescribeFeatureTransformation",
                "personalize:DescribeRecipe",
                "personalize:DescribeSchema",
                "personalize:DescribeSolution",
                "personalize:DescribeSolutionVersion",
                "personalize:GetSolutionMetrics",
                "personalize:ListBatchInferenceJobs",
                "personalize:ListCampaigns",
                "personalize:ListDatasetGroups",
                "personalize:ListDatasetImportJobs",
                "personalize:ListDatasets",
                "personalize:ListEventTrackers",
                "personalize:ListFilters",
                "personalize:ListRecipes",
                "personalize:ListSchemas",
                "personalize:ListSolutions",
                "personalize:ListSolutionVersions"
            ],
            "Resource": "*"
        }
    ]
}
```
