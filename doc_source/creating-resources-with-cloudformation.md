# Specifying resources with AWS CloudFormation<a name="creating-resources-with-cloudformation"></a>

 Amazon Personalize is integrated with AWS CloudFormation, a service that helps you to model and set up your AWS resources so that you can spend less time creating and managing your resources and infrastructure\. You create a template that describes all the AWS resources that you can specify \(such as Amazon Personalize dataset groups\)\. AWS CloudFormation then provisions and configures those resources for you\. 

 When you use AWS CloudFormation, you can reuse your template to set up your Amazon Personalize resources consistently and repeatedly\. Describe your resources once, and then provision the same resources over and over in multiple AWS accounts and Regions\. 

**Topics**
+ [Amazon Personalize and AWS CloudFormation templates](#working-with-templates)
+ [Example AWS CloudFormation templates for Amazon Personalize resources](#personalize-template-example)
+ [Learn more about AWS CloudFormation](#learn-more-cloudformation)

## Amazon Personalize and AWS CloudFormation templates<a name="working-with-templates"></a>

To provision and configure resources for Amazon Personalize and related services, you must understand [AWS CloudFormation templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html)\. Templates are formatted text files in JSON or YAML\. These templates describe the resources that you want to provision in your AWS CloudFormation stacks\. If you're unfamiliar with JSON or YAML, you can use AWS CloudFormation Designer to help you get started with AWS CloudFormation templates\. For more information, see [What is AWS CloudFormation Designer?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer.html) in the *AWS CloudFormation User Guide*\.

Amazon Personalize supports specifying datasets, dataset groups, dataset import jobs, schemas, and solutions in AWS CloudFormation\. For more information, see the [Amazon Personalize resource type reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_Personalize.html) in the *AWS CloudFormation User Guide*\.

## Example AWS CloudFormation templates for Amazon Personalize resources<a name="personalize-template-example"></a>

 The following AWS CloudFormation template examples show you how to specify different Amazon Personalize resources\. 

**Topics**
+ [CreateDatasetGroup](#cfn-create-dataset-group)
+ [CreateDataset](#cfn-create-dataset)
+ [CreateSchema](#cfn-create-schema)
+ [CreateSolution](#cfn-create-solution)

### CreateDatasetGroup<a name="cfn-create-dataset-group"></a>

------
#### [ JSON ]

```
{
   "AWSTemplateFormatVersion":"2010-09-09",
   "Resources":{
      "MyDatasetGroup": {
            "Type": "AWS::Personalize::DatasetGroup",
            "Properties": {
               "Name": "my-dataset-group-name"
            }
      }
   }
}
```

------
#### [ YAML ]

```
AWSTemplateFormatVersion: 2010-09-09
Resources:
  MyDatasetGroup:
    Type: 'AWS::Personalize::DatasetGroup'
    Properties:
      Name: my-dataset-group-name
```

------

### CreateDataset<a name="cfn-create-dataset"></a>

------
#### [ JSON ]

```
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Resources": {
    "MyDataset": {
      "Type": "AWS::Personalize::Dataset",
      "Properties": {
        "Name": "my-dataset-name",
        "DatasetType": "Interactions",
        "DatasetGroupArn": "arn:aws:personalize:us-west-2:123456789012:dataset-group/dataset-group-name",
        "SchemaArn": "arn:aws:personalize:us-west-2:123456789012:schema/schema-name",
        "DatasetImportJob": {
          "JobName": "my-import-job-name",
          "DataSource": {
            "DataLocation": "s3://bucket-name/file-name.csv"
          },
          "RoleArn": "arn:aws:iam::123456789012:role/personalize-role"
        }
      }
    }
  }
}
```

------
#### [ YAML ]

```
AWSTemplateFormatVersion: 2010-09-09
Resources:
  MyDataset:
    Type: 'AWS::Personalize::Dataset'
    Properties:
      Name: my-dataset-name
      DatasetType: Interactions
      DatasetGroupArn: 'arn:aws:personalize:us-west-2:123456789012:dataset-group/dataset-group-name'
      SchemaArn: 'arn:aws:personalize:us-west-2:123456789012:schema/schema-name'
      DatasetImportJob:
        JobName: my-import-job-name
        DataSource:
          DataLocation: 's3://bucket-name/file-name.csv'
        RoleArn: 'arn:aws:iam::123456789012:role/personalize-role'
```

------

### CreateSchema<a name="cfn-create-schema"></a>

------
#### [ JSON ]

```
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Resources": {
        "MySchema": {
            "Type": "AWS::Personalize::Schema",
            "Properties": {
                "Name": "my-schema-name",
                "Schema": "{\"type\": \"record\",\"name\": \"Interactions\", \"namespace\": \"com.amazonaws.personalize.schema\", \"fields\": [ { \"name\": \"USER_ID\", \"type\": \"string\" }, { \"name\": \"ITEM_ID\", \"type\": \"string\" }, { \"name\": \"TIMESTAMP\", \"type\": \"long\"}], \"version\": \"1.0\"}"
            }
        }
    }
}
```

------
#### [ YAML ]

```
AWSTemplateFormatVersion: 2010-09-09
Resources:
  MySchema:
    Type: AWS::Personalize::Schema
    Properties: 
      Name: "my-schema-name"
      Schema: >-
        {"type": "record","name": "Interactions", "namespace":
        "com.amazonaws.personalize.schema", "fields": [ { "name": "USER_ID",
        "type": "string" }, { "name": "ITEM_ID", "type": "string" }, { "name":
        "TIMESTAMP", "type": "long"}], "version": "1.0"}
```

------

### CreateSolution<a name="cfn-create-solution"></a>

------
#### [ JSON ]

```
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Resources": {
        "MySolution": {
            "Type": "AWS::Personalize::Solution",
            "Properties": {
               "Name": "my-solution-name",
               "DatasetGroupArn": "arn:aws:personalize:us-west-2:123456789012:dataset-group/my-dataset-group-name",
               "RecipeArn": "arn:aws:personalize:::recipe/aws-user-personalization",
               "SolutionConfig": {
                  "EventValueThreshold" : ".05"
                }
            }
         }
    }
}
```

------
#### [ YAML ]

```
AWSTemplateFormatVersion: 2010-09-09
Resources:
  MySolution:
    Type: 'AWS::Personalize::Solution'
    Properties:
      Name: my-solution-name
      DatasetGroupArn: >-
        arn:aws:personalize:us-west-2:123456789012:dataset-group/my-dataset-group-name
      RecipeArn: 'arn:aws:personalize:::recipe/aws-user-personalization'
      SolutionConfig:
        EventValueThreshold: '.05'
```

------

## Learn more about AWS CloudFormation<a name="learn-more-cloudformation"></a>

To learn more about AWS CloudFormation, see the following resources:
+ [AWS CloudFormation](http://aws.amazon.com/cloudformation/)
+ [AWS CloudFormation user guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
+ [AWS CloudFormation API reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/Welcome.html)
+ [AWS CloudFormation Command Line Interface user guide](https://docs.aws.amazon.com/cloudformation-cli/latest/userguide/what-is-cloudformation-cli.html)