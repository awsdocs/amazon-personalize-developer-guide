# Using Identity\-Based Policies \(IAM Policies\) for Amazon Personalize<a name="using-identity-based-policies"></a>

This topic provides examples of identity\-based policies that demonstrate how an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\) and thereby grant permissions to perform operations on Amazon Personalize resources\.

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your Amazon Personalize resources\. For more information, see [Overview of Managing Access Permissions to Your Amazon Personalize Resources](access-control-overview.md)\. 

**Topics**
+ [Permissions Required to Use the Amazon Personalize Console](#console-permissions)
+ [AWS Managed \(Predefined\) Policies for Amazon Personalize](#access-policy-aws-managed-policies)
+ [Customer Managed Policy Examples](#access-policy-customer-managed-examples)

The following shows an example of a permissions policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "personalize:ListDatasetGroups"
            ],
            "Resource": "*"
        }
    ]
}
```

This policy example grants a user permission to list the dataset groups in their account\.

For a table showing all of the Amazon Personalize API operations and the resources that they apply to, see [Amazon Personalize API Permissions: Actions, Permissions, and Resources Reference](api-permissions-reference.md)\. 

## Permissions Required to Use the Amazon Personalize Console<a name="console-permissions"></a>

The permissions reference table lists the Amazon Personalize API operations and shows the required permissions for each operation\. For more information about Amazon Personalize API operations, see [Amazon Personalize API Permissions: Actions, Permissions, and Resources Reference](api-permissions-reference.md)\. Amazon Personalize does not require any additional permissions when working with the Amazon Personalize console\.

## AWS Managed \(Predefined\) Policies for Amazon Personalize<a name="access-policy-aws-managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so that you can avoid having to investigate what permissions are needed\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. 

The following AWS managed policies, which you can attach to users in your account, are specific to Amazon Personalize:
+ **AmazonPersonalizeFullAccess** – Grants full access to Amazon Personalize resources\.

**Note**  
You can review these permissions policies by signing in to the IAM console and searching for specific policies there\.  
These policies work when you are using AWS SDKs or the AWS CLI\.

You can also create your own custom IAM policies to allow permissions for Amazon Personalize actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. 

## Customer Managed Policy Examples<a name="access-policy-customer-managed-examples"></a>

In this section, you can find example user policies that grant permissions for various Amazon Personalize actions\. These policies work when you are using AWS SDKs or the AWS CLI\. When you are using the console, you need to grant additional permissions specific to the console, which is discussed in [Permissions Required to Use the Amazon Personalize Console](#console-permissions)\.

**Topics**
+ [Example 1: Allow a User Read\-Only Access to Resources](#access-policy-customer-managed-first-example)
+ [Example 2: Allow a User Full Access to Resources](#access-policy-customer-managed-second-example)

### Example 1: Allow a User Read\-Only Access to Resources<a name="access-policy-customer-managed-first-example"></a>

The following example grants read\-only access to Amazon Personalize resources\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "personalize:DescribeAlgorithm",
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
                "personalize:ListCampaigns",
                "personalize:ListDatasetGroups",
                "personalize:ListDatasetImportJobs",
                "personalize:ListDatasets",
                "personalize:ListEventTrackers",
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

### Example 2: Allow a User Full Access to Resources<a name="access-policy-customer-managed-second-example"></a>

The following example grants full access to all Amazon Personalize resources\.

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