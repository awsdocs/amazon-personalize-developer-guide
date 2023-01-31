# Giving Amazon Personalize permission to use your AWS KMS key<a name="granting-personalize-key-access"></a>

 If you use AWS Key Management Service \(AWS KMS\) for encryption, you must grant Amazon Personalize and your service role \(the role you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\) permissions for your key\. To grant permissions, add your service role and Amazon Personalize as principles in your key policy\. If you use an AWS KMS policy for Amazon S3 bucket encryption, you must also attach a policy to your service role that grants permission to use the key\.

 The following sections provide the required permissions and policy examples for different encryption scenarios\. For more information on AWS KMS key policies, see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\. 

**Topics**
+ [Granting permissions for an encrypted Amazon S3 bucket](#encrypted-bucket-policy)
+ [Granting permissions for an encrypted Amazon Personalize dataset group](#encrypted-dsg-policy)

## Granting permissions for an encrypted Amazon S3 bucket<a name="encrypted-bucket-policy"></a>

If you use an AWS KMS key to encrypt your Amazon S3 bucket, your AWS KMS key policy *and* IAM policy attached to your service role must grant Amazon Personalize permission to use your key\. This applies for the following Amazon Personalize operations\. 
+ Dataset import jobs
+ Dataset export jobs
+ Batch inference jobs
+ Publishing metric attribution reports to Amazon S3

 Both policies must include following actions: 
+  Decrypt 
+  GenerateDataKey 
+  DescribeKey 
+  ListGrant 

**Topics**
+ [Key policy example](#export-job-key-policy)
+ [IAM policy example](#export-job-iam-policy)

### Key policy example<a name="export-job-key-policy"></a>

The following AWS KMS policy example grants Amazon Personalize and your service role the minimum permissions required to export a dataset to a bucket encrypted by an AWS KMS key\. The IAM policy attached to your service role must also grant permissions\. For a policy example see [IAM policy example](#export-job-iam-policy)\. 

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
                "kms:ListGrants"
            ],
      "Resource": "*"
    }
  ]
}
```

### IAM policy example<a name="export-job-iam-policy"></a>

If you use an AWS KMS key for bucket encryption, you must attach a policy to your IAM service role granting the role permission to use your key\. The following IAM policy example grants a role the minimum AWS KMS permissions required to export a dataset to a bucket encrypted by an AWS KMS key\. For a key policy example see [Key policy example](#export-job-key-policy)\. Additional Amazon S3 permissions are required\. For more information see [Dataset export job permissions requirements](export-data.md#export-permissions)\.

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

## Granting permissions for an encrypted Amazon Personalize dataset group<a name="encrypted-dsg-policy"></a>

If you specify an AWS KMS key when you create your dataset group \(Custom dataset group or Domain dataset group\), your key policy `Action` must include the `kms:Decrypt` action\. If you want to export data from a dataset in the dataset group, you must add the `kms:GenerateDataKeyWithoutPlaintext` action\. If only your dataset group is encrypted \(and not your bucket\), you don't need to modify the IAM policy for your Amazon Personalize service role\. 

The following key policy example grants Amazon Personalize and your service role the minimum permissions to access a dataset group encrypted by an AWS KMS key\.

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
      "Action": "kms:Decrypt",
      "Resource": "*"
    }
  ]
}
```