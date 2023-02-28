# Giving Amazon Personalize permission to use your AWS KMS key<a name="granting-personalize-key-access"></a>

 If you specify a AWS Key Management Service \(AWS KMS\) key when you use the Amazon Personalize console or APIs, you must grant Amazon Personalize permission to use your key\. To grant permissions, your AWS KMS key policy *and* IAM policy attached to your service role must grant Amazon Personalize permission to use your key\. This applies for creating the following in Amazon Personalize\. 
+ Dataset groups
+ Dataset export jobs
+ Batch inference jobs
+ Batch segment jobs
+ Metric attributions

 Your AWS KMS key policy and IAM policies must grant permissions for the following actions: 
+  Decrypt 
+  GenerateDataKey 
+  DescribeKey 
+  CreateGrant \(only required in key policy\) 
+  ListGrants 

**Topics**
+ [Key policy example](#export-job-key-policy)
+ [IAM policy example](#export-job-iam-policy)

## Key policy example<a name="export-job-key-policy"></a>

The following key policy example grants Amazon Personalize and your role the minimum permissions for the preceding Amazon Personalize operations\.  If you specify a key when you create a dataset group and want to export data from a dataset, your key policy must include the `GenerateDataKeyWithoutPlaintext` action\. 

```
{
  "Version": "2012-10-17",
  "Id": "key-policy-123",
  "Statement": [
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<account-id>:role/<personalize-role-name>",
        "Service": "personalize.amazonaws.com"
      },
      "Action": [
                "kms:Decrypt",
                "kms:GenerateDataKey",
                "kms:DescribeKey",
                "kms:CreateGrant",
                "kms:ListGrants"
            ],
      "Resource": "*"
    }
  ]
}
```

## IAM policy example<a name="export-job-iam-policy"></a>

 The following IAM policy example grants a role the minimum AWS KMS permissions required for the preceding Amazon Personalize operations\.  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kms:Decrypt",
                "kms:GenerateDataKey",
                "kms:DescribeKey",
                "kms:ListGrants"
            ],
            "Resource": "*"
        }
    ]
}
```