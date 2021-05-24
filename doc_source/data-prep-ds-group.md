# Step 1: Creating a dataset group<a name="data-prep-ds-group"></a>

A *dataset group* is container for Amazon Personalize components, including datasets, event trackers, solutions, filters, campaigns, and batch inference jobs\. A dataset group organizes your resources into independent collections, so resources from one dataset group cannot influence resources in any other dataset group\. For example, you might have an application that provides recommendations for streaming video and another that provides recommendations for audio books\. In Amazon Personalize, each application would have its own dataset group\. You can create a dataset group using the Amazon Personalize console, AWS SDK, or AWS Command Line Interface \(AWS CLI\)\.

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

1. Choose **Next**\. The **Create user\-item interaction data** page displays\. You are now ready to add a dataset with an associated schema to your dataset group\. See [Creating a dataset and a schema \(console\)](data-prep-creating-datasets.md#data-prep-creating-ds-console)\. 

## Creating a dataset group \(AWS CLI\)<a name="data-prep-creating-ds-group-cli"></a>

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

When the dataset group's `status` is ACTIVE, proceed to [Creating a dataset and a schema \(AWS CLI\)](data-prep-creating-datasets.md#data-prep-creating-ds-cli)\.

## Creating a dataset group \(AWS SDKs\)<a name="data-prep-creating-ds-group-sdk"></a>

Create a dataset group using the [CreateDatasetGroup](API_CreateDatasetGroup.md) operation\.

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

The [DescribeDatasetGroup](API_DescribeDatasetGroup.md) operation returns the `datasetGroupArn` and the status of the operation\.

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