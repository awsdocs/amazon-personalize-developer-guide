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

Identity\-based policies determine whether someone can create, access, or delete Amazon Personalize resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started with AWS managed policies and move toward least\-privilege permissions** – To get started granting permissions to your users and workloads, use the *AWS managed policies* that grant permissions for many common use cases\. They are available in your AWS account\. We recommend that you reduce permissions further by defining AWS customer managed policies that are specific to your use cases\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) or [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.
+ **Apply least\-privilege permissions** – When you set permissions with IAM policies, grant only the permissions required to perform a task\. You do this by defining the actions that can be taken on specific resources under specific conditions, also known as *least\-privilege permissions*\. For more information about using IAM to apply permissions, see [ Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.
+ **Use conditions in IAM policies to further restrict access** – You can add a condition to your policies to limit access to actions and resources\. For example, you can write a policy condition to specify that all requests must be sent using SSL\. You can also use conditions to grant access to service actions if they are used through a specific AWS service, such as AWS CloudFormation\. For more information, see [ IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.
+ **Use IAM Access Analyzer to validate your IAM policies to ensure secure and functional permissions** – IAM Access Analyzer validates new and existing policies so that the policies adhere to the IAM policy language \(JSON\) and IAM best practices\. IAM Access Analyzer provides more than 100 policy checks and actionable recommendations to help you author secure and functional policies\. For more information, see [IAM Access Analyzer policy validation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-validation.html) in the *IAM User Guide*\.
+ **Require multi\-factor authentication \(MFA\)** – If you have a scenario that requires IAM users or root users in your account, turn on MFA for additional security\. To require MFA when API operations are called, add MFA conditions to your policies\. For more information, see [ Configuring MFA\-protected API access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_configure-api-require.html) in the *IAM User Guide*\.

For more information about best practices in IAM, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

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
                "personalize:DescribeBatchSegmentJob",
                "personalize:DescribeCampaign",
                "personalize:DescribeDataset",
                "personalize:DescribeDatasetExportJob",
                "personalize:DescribeDatasetGroup",
                "personalize:DescribeDatasetImportJob",
                "personalize:DescribeEventTracker",
                "personalize:DescribeFeatureTransformation",
                "personalize:DescribeFilter",
                "personalize:DescribeRecipe",
                "personalize:DescribeRecommender",
                "personalize:DescribeSchema",
                "personalize:DescribeSolution",
                "personalize:DescribeSolutionVersion",
                "personalize:GetSolutionMetrics",
                "personalize:ListBatchInferenceJobs",
                "personalize:ListBatchSegmentJobs",
                "personalize:ListCampaigns",
                "personalize:ListDatasetExportJobs",
                "personalize:ListDatasetGroups",
                "personalize:ListDatasetImportJobs",
                "personalize:ListDatasets",
                "personalize:ListEventTrackers",
                "personalize:ListFilters",
                "personalize:ListRecipes",
                "personalize:ListRecommenders",
                "personalize:ListSchemas",
                "personalize:ListSolutions",
                "personalize:ListSolutionVersions"
            ],
            "Resource": "*"
        }
    ]
}
```