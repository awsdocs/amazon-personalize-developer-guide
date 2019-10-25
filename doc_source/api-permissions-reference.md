# Amazon Personalize API Permissions: Actions, Permissions, and Resources Reference<a name="api-permissions-reference"></a>

Use the following as a reference when you write a permissions policy to attach to an IAM identity \(identity\-based policy\)\. The each Amazon Personalize API operation\. For each operation, the required permissions to perform the operation \(action\) and the AWS resources acted on are given\. You specify the actions in the policy's `Action` field\. You specify the resource value in the policy's `Resource` field\.

To express conditions in your Amazon Personalize policies, use AWS\-wide condition keys\. For a complete list of AWS\-wide keys, see [Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_iam-condition-keys.html) in the *IAM User Guide*\.

**Note**  
To specify an action, use the `personalize` prefix followed by the API operation name \(for example, `personalize:DeleteDataset`\)\.