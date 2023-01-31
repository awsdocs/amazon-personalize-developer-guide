# Setting up Amazon Personalize<a name="setup"></a>

Before using Amazon Personalize, you must have an Amazon Web Services \(AWS\) account\. After you have an AWS account, you can access Amazon Personalize through the Amazon Personalize console, the AWS Command Line Interface \(AWS CLI\), or the AWS SDKs\.

This guide includes examples for AWS CLI, SDK for Python \(Boto3\), and SDK for Java 2\.x\.

**Topics**
+ [Sign up for AWS](#aws-personalize-set-up-aws-account)
+ [Regions and endpoints](#endpoints)
+ [Setting up permissions](aws-personalize-set-up-permissions.md)
+ [Setting up the AWS CLI](aws-personalize-set-up-aws-cli.md)
+ [Setting up the AWS SDKs](aws-personalize-set-up-sdks.md)

## Sign up for AWS<a name="aws-personalize-set-up-aws-account"></a>

When you sign up for AWS, your account is automatically signed up for all services in AWS, including Amazon Personalize\. You are charged only for the services that you use\.

If you have an AWS account already, skip to the next task\. If you don't have an AWS account, use the following procedure to create one\.

**To sign up for AWS**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)\.

1. Create an AWS Identity and Access Management \(IAM\) admin user\. See [Creating your first IAM user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *AWS Identity and Access Management User Guide* for instructions\.

   An IAM user with administrator permissions has unrestricted access to the AWS services in your account\. For information about restricting access to Amazon Personalize operations, see [Amazon Personalize identity\-based policies](security_iam_service-with-iam.md#security_iam_service-with-iam-id-based-policies)

1. Create an IAM user for use with Amazon Personalize\. The account requires certain permissions\. For more information, see [Setting up permissions](aws-personalize-set-up-permissions.md)\.

## Regions and endpoints<a name="endpoints"></a>

An endpoint is a URL that is the entry point for a web service\. Each endpoint is associated with a specific AWS region\. Pay attention to the default regions of the Amazon Personalize console, the AWS CLI, and the Amazon Personalize SDKs, as all Amazon Personalize components of a given campaign \(dataset, solution, campaign, event tracker\) must be created in the same region\. For the regions and endpoints supported by Amazon Personalize, see [Regions and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#personalize_region)\.