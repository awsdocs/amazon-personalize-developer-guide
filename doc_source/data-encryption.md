# Data encryption<a name="data-encryption"></a>

The following information explains where Amazon Personalize uses data encryption to protect your data\.

## Encryption at rest<a name="data-protection-at-rest"></a>

Amazon Personalize uses the default Amazon S3 key \(SSE\-S3\) for server\-side encryption of Amazon Personalize data placed in your S3 buckets\. You can also use one of your own AWS Key Management Service \(AWS KMS\) keys\.

## Encryption in transit<a name="data-protection-in-transit"></a>

Amazon Personalize copies data out of your account and processes it in an internal AWS system\. By default, Amazon Personalize uses TLS 1\.2 with AWS certificates to encrypt data in transit\.

## Key management<a name="data-protection-keys"></a>

The default Amazon S3 key is managed by AWS\. It is the responsibility of the customer to manage any customer\-provided [AWS Key Management Service \(AWS KMS\)](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) keys\.