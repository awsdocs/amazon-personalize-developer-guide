# Data encryption<a name="data-encryption"></a>

The following information explains where Amazon Personalize uses data encryption to protect your data\.

## Encryption at rest<a name="data-protection-at-rest"></a>

Any data stored within Amazon Personalize is always encrypted at rest with Amazon Personalize managed AWS Key Management Service \(AWS KMS\) keys\. If you provide your own AWS KMS key during resource creation, Amazon Personalize uses the key to encrypt your data and store it\. For example, if you provide a AWS KMS ARN in the [CreateDatasetGroup](API_CreateDatasetGroup.md) operation, Amazon Personalize uses the key to encrypt and store data you import into any datasets that you create in that dataset group\. 

You must grant Amazon Personalize and your Amazon Personalize IAM service role decrypt permissions in your key policy\. For more information, see [Giving Amazon Personalize permission to use your AWS KMS key](granting-personalize-key-access.md)\.

For information about data encryption in Amazon S3 see [Protecting data using encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingEncryption.html) in the *Amazon Simple Storage Service User Guide*\. For information about managing your own AWS KMS key, see [Managing keys](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) in the *AWS Key Management Service Developer Guide*\. 

## Encryption in transit<a name="data-protection-in-transit"></a>

 Amazon Personalize uses TLS with AWS certificates to encrypt any data sent to other AWS services\. Any communication with other AWS services happens over HTTPS, and Amazon Personalize endpoints support only secure connections over HTTPS\. 

 Amazon Personalize copies data out of your account and processes it in an internal AWS system\. When processing data, Amazon Personalize encrypts data with either a Amazon Personalize AWS KMS key or any AWS KMS key you provide\.

## Key management<a name="data-protection-keys"></a>

AWS manages any default AWS KMS keys\. It is your responsibility to manage any AWS KMS keys that you own\. You must grant Amazon Personalize and your Amazon Personalize IAM service role decrypt permissions in your key policy\. For more information, see [Giving Amazon Personalize permission to use your AWS KMS key](granting-personalize-key-access.md)\.

For information about managing your own AWS KMS key, see [Managing keys](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) in the *AWS Key Management Service Developer Guide*\. 