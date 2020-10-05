# Step 1: Creating a Dataset Group<a name="data-prep-ds-group"></a>

*Dataset groups* are domain\-specific containers for datasets\. For example, you might have an application that provides recommendations for streaming video and another that provides recommendations for audio books\. In Amazon Personalize, each application would have its own dataset group\. You can create a dataset group using the Amazon Personalize console, AWS SDK, or AWS Command Line Interface \(AWS CLI\)\.

**Topics**
+ [Creating a Dataset Group \(Console\)](#data-prep-creating-ds-group-console)
+ [Creating a Dataset Group \(AWS CLI\)](#data-prep-creating-ds-group-cli)
+ [Creating a Dataset Group \(AWS Python SDK\)](#data-prep-creating-ds-group-sdk)

## Creating a Dataset Group \(Console\)<a name="data-prep-creating-ds-group-console"></a>

Create a dataset group by specifying the dataset group name in the Amazon Personalize console\.

**To create a dataset group**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/](https://console.aws.amazon.com/personalize/) and sign in to your account\.

1. Choose **Create dataset group**\.

1. If this is your first time using Amazon Personalize, on the **Create dataset group** page, in **New dataset group**, choose **Get started**\.

1. In **Dataset group details**, for **Dataset group name**, specify a name for your dataset group\. 

1. Choose **Next**\. The **Create user\-item interaction data** page displays\. You are now ready to add a dataset with an associated schema to your dataset group\. See [Creating a Dataset and a Schema \(Console\)](data-prep-creating-datasets.md#data-prep-creating-ds-console)\. 

## Creating a Dataset Group \(AWS CLI\)<a name="data-prep-creating-ds-group-cli"></a>

Create a dataset group by running the following command\. For more information about the API, see [CreateDatasetGroup](API_CreateDatasetGroup.md)\.

```
aws personalize create-dataset-group --name dataset group name
```

The dataset group Amazon Resource Name \(ARN\) is displayed as shown in the following example\.

```
{
  "datasetGroupArn": "arn:aws:personalize:us-west-2:acct-id:dataset-group/DatasetGroupName"
}
```

Record this value for future use\. To display the dataset group that you created, use the `describe-dataset-group` command and specify the returned dataset group ARN\.

```
aws personalize describe-dataset-group \
--dataset-group-arn dataset group arn
```

The dataset group and its properties are displayed, as shown in the following example\.

```
{
    "datasetGroup": {
        "name": "DatasetGroupName",
        "datasetGroupArn": "arn:aws:personalize:us-west-2:acct-id:dataset-group/DatasetGroupName",
        "status": "ACTIVE",
        "creationDateTime": 1542392161.262,
        "lastUpdatedDateTime": 1542396513.377
    }
}
```

When the dataset group's `status` is ACTIVE, proceed to [Creating a Dataset and a Schema \(AWS CLI\)](data-prep-creating-datasets.md#data-prep-creating-ds-cli)\.

## Creating a Dataset Group \(AWS Python SDK\)<a name="data-prep-creating-ds-group-sdk"></a>

Create a dataset group using the [CreateDatasetGroup](API_CreateDatasetGroup.md) operation\.

```
import boto3

personalize = boto3.client('personalize')

response = personalize.create_dataset_group(name = 'YourDatasetGroup')
dsg_arn = response['datasetGroupArn']

description = personalize.describe_dataset_group(datasetGroupArn = dsg_arn)['datasetGroup']

print('Name: ' + description['name'])
print('ARN: ' + description['datasetGroupArn'])
print('Status: ' + description['status'])
```

The [DescribeDatasetGroup](API_DescribeDatasetGroup.md) operation returns the `datasetGroupArn` and the status of the operation\.

When the `status` is ACTIVE, proceed to [Creating a Dataset and a Schema \(AWS Python SDK\)](data-prep-creating-datasets.md#data-prep-creating-ds-sdk)\.