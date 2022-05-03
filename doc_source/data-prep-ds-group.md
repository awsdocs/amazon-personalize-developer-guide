# Step 1: Creating a Custom dataset group<a name="data-prep-ds-group"></a>

A *Custom dataset group* is container for Amazon Personalize components and custom resources, including datasets, event trackers, solutions, filters, campaigns, and batch inference jobs\. A dataset group organizes your resources into independent collections, so resources from one dataset group cannot influence resources in any other dataset group\. 

For example, you might have an application that provides recommendations for streaming video and another that provides recommendations for audio books\. In Amazon Personalize, each application would have its own dataset group\. You can create a dataset group with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\) or AWS SDKs\.

**Topics**
+ [Creating a dataset group \(console\)](#data-prep-creating-ds-group-console)
+ [Creating a dataset group \(AWS CLI\)](#data-prep-creating-ds-group-cli)
+ [Creating a dataset group \(AWS SDKs\)](#data-prep-creating-ds-group-sdk)

## Creating a dataset group \(console\)<a name="data-prep-creating-ds-group-console"></a>

Create a dataset group by specifying the dataset group name in the Amazon Personalize console\.

**To create a dataset group**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. Choose **Create dataset group**\.

1. If this is your first time using Amazon Personalize, on the **Create dataset group** page, in **New dataset group**, choose **Get started**\.

1. In **Dataset group details**, for **Dataset group name**, specify a name for your dataset group\. 

1. For **Domain** choose **Custom**\.

1. For **Tags**, optionally add any tags\. For more information about tagging Amazon Personalize resources, see [Tagging Amazon Personalize resources](tagging-resources.md)\.

1. Choose **Next**\. The **Create user\-item interaction data** page displays\. You are now ready to add a dataset with an associated schema to your dataset group\. See [Creating a dataset and a schema \(console\)](data-prep-creating-datasets.md#data-prep-creating-ds-console)\. 

## Creating a dataset group \(AWS CLI\)<a name="data-prep-creating-ds-group-cli"></a>

Create a dataset group with the following command\. For more information about the CreateDatasetGroup API operation, see [CreateDatasetGroup](API_CreateDatasetGroup.md) in the API reference section\. You can use the Tags parameter to optionally tag resources in Amazon Personalize\. For a sample see [Adding tags \(AWS CLI\)](tags-add.md#add-tag-cli)\.

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

When the dataset group's `status` is ACTIVE, proceed to [Creating a dataset and a schema \(AWS CLI\)](data-prep-creating-datasets.md#data-prep-creating-ds-cli)\.

## Creating a dataset group \(AWS SDKs\)<a name="data-prep-creating-ds-group-sdk"></a>

The following code shows how to create a dataset group with the AWS SDK for Python \(Boto3\) or the SDK for Java 2\.x\. For more information about the API operation, see [CreateDatasetGroup](API_CreateDatasetGroup.md) in the API reference section\. You can use the Tags parameter to optionally tag resources in Amazon Personalize\. For a sample see [Adding tags \(AWS SDKs\)](tags-add.md#add-tag-sdk)\. 

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3

personalize = boto3.client('personalize')

response = personalize.create_dataset_group(name = 'dataset group name')
dsg_arn = response['datasetGroupArn']

description = personalize.describe_dataset_group(datasetGroupArn = dsg_arn)['datasetGroup']

print('Name: ' + description['name'])
print('ARN: ' + description['datasetGroupArn'])
print('Status: ' + description['status'])
```

------
#### [ SDK for Java 2\.x ]

```
public static void createDatasetGroup(PersonalizeClient personalizeClient, String datasetGroupName) {
        
    long waitInMilliseconds = 60 * 1000;

    try {
        CreateDatasetGroupRequest createDatasetGroupRequest = CreateDatasetGroupRequest.builder()
            .name(datasetGroupName)
            .build();
            
        String datasetGroupArn = personalizeClient.createDatasetGroup(createDatasetGroupRequest)
            .datasetGroupArn();

        long maxTime = Instant.now().getEpochSecond() + (15 * 60); // 15 minutes

        DescribeDatasetGroupRequest describeRequest = DescribeDatasetGroupRequest.builder()
            .datasetGroupArn(datasetGroupArn)
            .build();

        String status = null;
        
        while (Instant.now().getEpochSecond() < maxTime) {
            
            status = personalizeClient.describeDatasetGroup(describeRequest)
                .datasetGroup()
                .status();
            
            System.out.println("DatasetGroup status:" + status);

            if (status.equals("ACTIVE") || status.equals("CREATE FAILED")) {
                break;
            }
            
            try {
                Thread.sleep(waitInMilliseconds);
            } catch (InterruptedException e) {
                System.out.println(e.getMessage());
            }
        }
    } catch(PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
}
```

------

The [DescribeDatasetGroup](API_DescribeDatasetGroup.md) operation returns the `datasetGroupArn` and the status of the operation\. When the dataset group's `status` is ACTIVE, proceed to [Creating a dataset and a schema \(AWS SDKs\)](data-prep-creating-datasets.md#data-prep-creating-ds-sdk)\.