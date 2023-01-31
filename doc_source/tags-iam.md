# Using tags in IAM policies<a name="tags-iam"></a>

After you start implementing tags, you can apply tag\-based, resource\-level permissions to AWS Identity and Access Management \(IAM\) policies and API operations\. This includes operations that support adding tags to resources when resources are created\. By using tags in this way, you can implement granular control of which groups and users in your AWS account have permission to create and tag resources, and which groups and users have permission to create, update, and remove tags more generally\.

For example, you can create a policy that allows a user to have full access to all of the Amazon Personalize resources where their name is a value in the `Owner` tag for the resource\.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Sid": "ModifyResourceIfOwner",
         "Effect": "Allow",
         "Action": "personalize:*",
         "Resource": "*",
         "Condition": {
            "StringEqualsIgnoreCase": {
               "aws:ResourceTag/Owner": "${aws:username}"
            }
         }
      }
   ]
}
```

The following example shows how to create a policy to allow creating and deleting a dataset\. These operations are allowed only if the user name is `johndoe`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "personalize:CreateDataset",
                "personalize:DeleteDataset"
            ],
            "Resource": "arn:aws:personalize:*:*:dataset/*",
            "Condition": {
                "StringEquals": {"aws:username" : "johndoe"}
            }
        },
        {
            "Effect": "Allow",
            "Action": "personalize:DescribeDataset",
            "Resource": "*"
        }
    ]
}
```

If you define tag\-based, resource\-level permissions, the permissions take effect immediately\. This means that your resources are more secure as soon as they're created, and you can quickly start enforcing the use of tags for new resources\. You can also use resource\-level permissions to control which tag keys and values can be associated with new and existing resources\. For more information, see [Controlling access to AWS resources using tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the *IAM User Guide*\.