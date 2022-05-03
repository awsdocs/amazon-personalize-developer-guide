# Adding tags to resources<a name="tags-add"></a>

You can add, display, update, and remove tag keys and values from Amazon Personalize resources with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\. The following examples show how to add a tag to Amazon Personalize dataset group\. You can add tags to other Amazon Personalize resources in the same way: Either with the Tags option in the console or Tags parameter with the AWS CLI or SDKs\. 

**Topics**
+ [Adding tags \(console\)](#add-tag-console)
+ [Adding tags \(AWS CLI\)](#add-tag-cli)
+ [Adding tags \(AWS SDKs\)](#add-tag-sdk)

## Adding tags \(console\)<a name="add-tag-console"></a>



When you create a resource in Amazon Personalize, you can add optional tags with the Amazon Personalize console\. The following example adds a tag to a dataset group\. 

**To add tags to a new dataset group**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. Choose **Create dataset group**\.

1. For **Name**, enter a name\.

1. For **Domain**, choose a domain\.

1. Expand the **Tags** section and choose **Add new tag**\.

1. For **Key** and **Value**, enter appropriate values\.

   For example, **Environment** and **Test**, respectively\.

1. To add more tags, choose **Add new tag**\.

   You can add up to 50 tags to a resource\.

1. Choose **Next** to continue creating your resource\.

Adding tags to an existing resource is similar: Choose your resource and use the **Tags** fields to add your tags\. 

## Adding tags \(AWS CLI\)<a name="add-tag-cli"></a>

You can use the AWS Command Line Interface \(AWS CLI\) to add tags when you create a resource or add tags to an existing resource\.

**Topics**
+ [Adding tags when you create a resource](#create-resource-with-tags-cli)
+ [Adding tags to an existing resource](#add-tag-existing-resource-cli)

### Adding tags when you create a resource<a name="create-resource-with-tags-cli"></a>

To create a new resource and add a tag to it with the AWS CLI, use the appropriate `create` command for the resource and include the `tags` parameter and values\. For example, the following command creates a new Domain dataset group named `myDatasetGroup` for the ECOMMERCE domain, and adds the following tags: An `Environment` tag key with a `Test` tag value, and a `Owner` tag key and a `xyzCorp` value\. 

```
aws personalize create-dataset-group \
--name myDatasetGroup \
--domain ECOMMERCE \
--tags tagKey=Environment,tagValue=Test tagKey=Owner,tagValue=xyzCorp
```

For information about the commands that you can use to create an Amazon Personalize resource, see the [Amazon Personalize AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/personalize/)\.

### Adding tags to an existing resource<a name="add-tag-existing-resource-cli"></a>

To add a tag to an existing resource, use the `tag-resource` command and specify the ARN of the resource and provide the tag key and value in the `tags-model` parameter\.

```
aws personalize tag-resource \
--resource-arn resource ARN \
--tags tagKey=key,tagValue=value
```

## Adding tags \(AWS SDKs\)<a name="add-tag-sdk"></a>

You can use the AWS SDKs to add tags when you create a resource, or to add tags to an existing resource\.

**Topics**
+ [Adding tags when you create a resource](#create-resource-with-tags-sdk)
+ [Adding tags to an existing resource](#add-tag-existing-resource-sdk)

### Adding tags when you create a resource<a name="create-resource-with-tags-sdk"></a>

To create a new resource and add a tag to it with the AWS SDKs, use the appropriate `create` method\. Use the `tags` parameter to specify the key\-value pairs for each of your tags\. For example, the following code creates a new Domain dataset group named `myDatasetGroup` for the ECOMMERCE domain and adds the following tags: An `Environment` tag key with a `Test` tag value, and a `Owner` tag key and a `xyzCorp` value\. 

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3

personalize = boto3.client('personalize')

response = personalize.create_dataset_group(
  name = 'myDatasetGroup'
  domain = 'ECOMMERCE'
  tags = [
    {
      'tagKey': 'Environment',
      'tagValue': 'Test'
    },
    {
      'tagKey': 'Owner',
      'tagValue': 'xyzCorp'    
    }
  ]    
)
dsg_arn = response['datasetGroupArn']

description = personalize.describe_dataset_group(datasetGroupArn = dsg_arn)['datasetGroup']

print('Name: ' + description['name'])
print('ARN: ' + description['datasetGroupArn'])
print('Status: ' + description['status'])
```

------
#### [ SDK for Java 2\.x ]

```
public static String createDomainDatasetGroup(PersonalizeClient personalizeClient, 
                                              String datasetGroupName,
                                              String domain) {
    
    try {
         
        ArrayList <Tag> tags = new ArrayList<>();

        Tag tag1 = Tag.builder()
                .tagKey("Environment")
                .tagValue("Test")
                .build();
        tags.add(tag1);
        Tag tag2 = Tag.builder()
                .tagKey("Owner")
                .tagValue("xyzCorp")
                .build();
        tags.add(tag2);
    
        CreateDatasetGroupRequest createDatasetGroupRequest = CreateDatasetGroupRequest.builder()
                .name(datasetGroupName)
                .domain(domain)
                .tags(tags)
                .build();
        return personalizeClient.createDatasetGroup(createDatasetGroupRequest).datasetGroupArn();
    } catch (PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
    return "";
}
```

------

### Adding tags to an existing resource<a name="add-tag-existing-resource-sdk"></a>

The following code shows how to add a tag to an existing resource\. Specify the Amazon Resource Name \(ARN\) of the resource that you want to add tags to and specify key\-value pairs for each of your tags\.

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3
personalize = boto3.client('personalize')

add_tags_response = personalize tag_resource(
  resourceArn = "resourceArn",
  tags = [
    {
      'tagKey': 'Environment',
      'tagValue': 'Test'
    },
    {
      'tagKey': 'Owner',
      'tagValue': 'xyzCorp'    
    }
  ]    
)
```

------
#### [ SDK for Java 2\.x ]

```
public static void tagResource(PersonalizeClient personalizeClient, 
                                              String resourceArn,
                                              String domain) {
    
    try {
         
         ArrayList <Tag> tagList = new ArrayList<>();

          Tag tag1 = Tag.builder()
                  .tagKey("Environment")
                  .tagValue("Test")
                  .build();
          tags.add(tag1);
          Tag tag2 = Tag.builder()
                  .tagKey("Owner")
                  .tagValue("xyzCorp")
                  .build();
          tags.add(tag2);
    
        TagResourceRequest tagResourceRequest = TagResourceRequest.builder()
                .resourceArn(resourceArn)
                .tags(tagList)
                .build();
                
        personalizeClient.tagResource(tagResourceRequest);
        System.out.println("Tags have been added to "+ resourceArn);
        
    } catch (PersonalizeException e) {
        System.out.println(e.awsErrorDetails().errorMessage());
    }
    return "";
}
```

------