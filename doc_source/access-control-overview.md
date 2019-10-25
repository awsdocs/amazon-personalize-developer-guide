# Overview of Managing Access Permissions to Your Amazon Personalize Resources<a name="access-control-overview"></a>

Every AWS resource is owned by an AWS account, and permissions to create or access a resource are governed by permissions policies\. An account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\), and some services \(such as AWS Lambda\) also support attaching permissions policies to resources\. 

**Note**  
An *account administrator* \(or administrator user\) is a user with administrator privileges\. For more information, see [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

When granting permissions, you decide who is getting the permissions, the resources they get permissions for, and the specific actions that you want to allow on those resources\.

**Topics**
+ [Amazon Personalize Resources and Operations](#access-control-resources)
+ [Understanding Resource Ownership](#access-control-resource-ownership)
+ [Managing Access to Resources](#manage-access-overview)
+ [Specifying Policy Elements: Actions, Effects, and Principals](#specify-policy-elements)
+ [Specifying Conditions in a Policy](#specifying-conditions-overview)
+ [Specifying a Service Role](#specifying-service-roles-overview)

## Amazon Personalize Resources and Operations<a name="access-control-resources"></a>

In Amazon Personalize, there are many resource types such as *a dataset*, a *campaign*, an *event tracker*, and a *feature transformation*\. In a policy, you use an Amazon Resource Name \(ARN\) to identify the resource that the policy applies to\.

These resources have unique Amazon Resource Names \(ARNs\) associated with them, as shown in the following table\. 


****  

| Resource Type | ARN Format | 
| --- | --- | 
| Algorithm ARN | arn:aws:personalize:region:account\-id:algorithm/algorithm\-name | 
| Campaign ARN | arn:aws:personalize:region:account\-id:campaign/campaign\-name | 
| Dataset ARN | arn:aws:personalize:region:account\-id:dataset/dataset\-name | 
| Dataset group ARN | arn:aws:personalize:region:account\-id:dataset\-group/dataset\-group\-name | 
| Dataset import job ARN | arn:aws:personalize:region:account\-id:dataset\-import\-job/dataset\-import\-job\-name | 
| Event tracker ARN | arn:aws:personalize:region:account\-id:event\-tracker/event\-tracker\-name | 
| Feature transformation ARN | arn:aws:personalize:region:account\-id:feature\-transformation/feature\-transformation\-name | 
| Recipe ARN | arn:aws:personalize:region:account\-id:recipe/recipe\-name | 
| Schema ARN | arn:aws:personalize:region:account\-id:schema/schema\-name | 
| Solution ARN | arn:aws:personalize:region:account\-id:solution/solution\-name | 
| Solution Version ARN | arn:aws:personalize:region:account\-id:solution\-version/solution\-version\-name | 

Amazon Personalize provides a set of operations to work with Amazon Personalize resources\. For a list of available operations, see [Amazon Personalize API Permissions Reference](api-permissions-reference.md)\.

## Understanding Resource Ownership<a name="access-control-resource-ownership"></a>

An AWS account owns the resources that are created in the account, regardless of who created the resources\. Specifically, the resource owner is the AWS account of the [principal entity](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) \(that is, the root account or an IAM user\) that authenticates the resource creation request\. The following examples illustrate how this works:
+ If you use the root account credentials of your AWS account to create a dataset, your AWS account is the owner of the resource \(in Amazon Personalize, the resource is a dataset\)\.
+ If you create an IAM user in your AWS account and grant permissions to create a dataset to that user, the user can create a dataset\. However, your AWS account, to which the user belongs, owns the dataset resource\.

## Managing Access to Resources<a name="manage-access-overview"></a>

A *permissions policy* describes who has access to what\. The following section explains the available options for creating permissions policies\.

**Note**  
This section discusses using IAM in the context of Amazon Personalize\. It doesn't provide detailed information about the IAM service\. For complete IAM documentation, see [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\. For information about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

Policies attached to an IAM identity are referred to as *identity\-based* policies \(IAM polices\) and policies attached to a resource are referred to as *resource\-based* policies\. Amazon Personalize supports identity\-based policies\. 

**Topics**
+ [Identity\-Based Policies \(IAM Policies\)](#manage-access-iam-policies)
+ [Resource\-Based Policies](#manage-access-resource-policies)

### Identity\-Based Policies \(IAM Policies\)<a name="manage-access-iam-policies"></a>

You can attach policies to IAM identities\. For example, you can do the following:
+ **Attach a permissions policy to a user or a group in your account** – To grant a user permissions to create an Amazon Personalize resource, such as a dataset, you can attach a permissions policy to a user or group that the user belongs to\.
+ **Attach a permissions policy to a role \(grant cross\-account permissions\)** – You can attach an identity\-based permissions policy to an IAM role to grant cross\-account permissions\. For example, the administrator in account A can create a role to grant cross\-account permissions to another AWS account \(for example, account B\) or an AWS service as follows:

  1. Account A administrator creates an IAM role and attaches a permissions policy to the role that grants permissions on resources in account A\.

  1. Account A administrator attaches a trust policy to the role identifying account B as the principal who can assume the role\. 

  1. Account B administrator can then share the role with users in account B\. Doing this allows users in account B to create or access resources in account A on behalf of account A\. The principal in the trust policy can also be an AWS service principal if you want to grant an AWS service permissions to assume the role\.

  For more information about using IAM to delegate permissions, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

The following is an example policy that grants permissions to create a dataset group and a dataset\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "personalize:CreateDatasetGroup"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "personalize:CreateDataset"
            ],
            "Resource": [
                "arn:aws:personalize:region:acct-id:dataset-group/*",
                "arn:aws:personalize:region:acct-id:schema/*"
            ]
        }
    ]
}
```

The policy has two statements:
+ The first statement grants permission for the `personalize:CreateDatasetGroup` action\. Because the CreateDatasetGroup operation does not act on any particular Amazon Personalize resource, the action grants user permission to create any dataset group\. 
+ The second statement grants permission for the `personalize:CreateDataset` action\. To create a dataset, you need to specify a dataset group and schema\. The policy gives access to all dataset groups and schema resources in the specified AWS account and AWS Region\.

The policy doesn't specify the `Principal` element because in an identity\-based policy you don't specify the principal who gets the permission\. When you attach the policy to a user, the user is the implicit principal\. When you attach a permissions policy to an IAM role, the principal identified in the role's trust policy gets the permissions\. 

For more information about using identity\-based policies with Amazon Personalize, see [Using Identity\-Based Policies \(IAM Policies\) for Amazon Personalize](using-identity-based-policies.md)\. For more information about users, groups, roles, and permissions, see [Identities \(Users, Groups, and Roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\. 

### Resource\-Based Policies<a name="manage-access-resource-policies"></a>

Other services, such as Amazon S3, also support resource\-based permissions policies\. For example, you can attach a policy to an S3 bucket to manage access permissions to that bucket\. Amazon Personalize does not support resource\-based policies\.

## Specifying Policy Elements: Actions, Effects, and Principals<a name="specify-policy-elements"></a>

For each Amazon Personalize resource, the service defines a set of API operations\. To grant permissions for these API operations, Amazon Personalize defines a set of actions that you can specify in a policy\. Some API operations can require permissions for more than one action in order to perform the API operation\. For more information about resources and API operations, see [Amazon Personalize Resources and Operations](#access-control-resources) and [Amazon Personalize API Permissions Reference](api-permissions-reference.md)\.

The following are the most basic policy elements:
+ **Resource** – You use an Amazon Resource Name \(ARN\) to identify the resource that the policy applies to\. For more information, see [Amazon Personalize Resources and Operations](#access-control-resources)\.
+ **Action** – You use action keywords to identify resource operations that you want to allow or deny\. For example, you can use `personalize:ListDatasets` to list datasets\.
+ **Effect** – You specify the effect, either allow or deny, when the user requests the specific action\. If you don't explicitly grant access to \(allow\) a resource, access is implicitly denied\. You can also explicitly deny access to a resource, which you might do to make sure that a user cannot access it, even if a different policy grants access\.
+ **Principal** – In identity\-based policies \(IAM policies\), the user that the policy is attached to is the implicit principal\. For resource\-based policies, you specify the user, account, service, or other entity that you want to receive permissions \(applies to resource\-based policies only\)\. Amazon Personalize doesn't support resource\-based policies\.

To learn more about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a showing all of the Amazon Personalize API operations and the resources that they apply to, see [Amazon Personalize API Permissions: Actions, Permissions, and Resources Reference](api-permissions-reference.md)\.

## Specifying Conditions in a Policy<a name="specifying-conditions-overview"></a>

When you grant permissions, you can use the access policy language to specify the conditions when a policy should take effect\. For example, you might want a policy to be applied only after a specific date\. For more information about specifying conditions in a policy language, see [Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#Condition) in the *IAM User Guide*\.

To express conditions, you use predefined condition keys\. There are no condition keys specific to Amazon Personalize\. However, there are AWS\-wide condition keys that you can use as appropriate\. For a complete list of AWS\-wide keys, see [Available Keys for Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

## Specifying a Service Role<a name="specifying-service-roles-overview"></a>

Certain Amazon Personalize operations require permissions to to access AWS services on your behalf\. For example, to import your data into a dataset with [CreateDatasetImportJob](API_CreateDatasetImportJob.md), Amazon Personalize needs access to the Amazon S3 bucket that contains the data\. To give permissions you create an IAM service role\. For more information, see [Creating an IAM Role](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\.