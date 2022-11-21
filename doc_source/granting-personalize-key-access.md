# Giving Amazon Personalize permission to use your AWS KMS key<a name="granting-personalize-key-access"></a>

 If you use AWS Key Management Service \(AWS KMS\) for encryption, you must grant Amazon Personalize and your service role \(the role you created in [Creating an IAM role for Amazon Personalize](aws-personalize-set-up-permissions.md#set-up-create-role-with-permissions)\) decrypt permissions for your key\. To grant permissions, add your service role and Amazon Personalize as principles in your key policy\. For the `Action`, add the `kms:Decrypt` action\. For exporting data, if your dataset group uses an AWS KMS key for encryption, you must add the `kms:GenerateDataKeyWithoutPlaintext` action\. 

The following AWS KMS policy example grants Amazon Personalize and your service role the minimum permissions to access resources encrypted by your key\. For more information on AWS KMS key policies, see [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.

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