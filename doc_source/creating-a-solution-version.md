# Step 3: Creating a Solution Version<a name="creating-a-solution-version"></a>

Once you have completed [Step 1: Choosing a Recipe](working-with-predefined-recipes.md) and [Step 2: Configuring a Solution](customizing-solution-config.md), you are ready to create a Solution Version\. 

 A *Solution Version* refers to a trained machine learning model you can deploy to get recommendations for customers\. You can create a solution version using the console, AWS Command Line Interface \(AWS CLI\), or AWS SDK\. 

**Topics**
+ [Creating a Solution Version \(Console\)](#create-solution-version-console)
+ [Creating a Solution Version \(AWS CLI\)](#create-solution-version-cli)
+ [Creating a Solution Version \(AWS Python SDK\)](#create-solution-version-sdk)

## Creating a Solution Version \(Console\)<a name="create-solution-version-console"></a>

If you just completed [Step 2: Configuring a Solution](customizing-solution-config.md) and the **Create solution version** is displayed, choose **FINISH** to create a solution version\.

On the solution details page, you can track training progress in the **Solution versions** section\. When training is complete, the status is **Active** and you are ready to deploy a campaign and get recommendations\. See step 3 in the [Getting Started \(Console\)](getting-started-console.md) tutorial\.

If you navigated away from the **Create solution version** page or want to create an additional solution version for an existing solution, create a new solution version from the solution overview page as follows\.

**Create a new solution version**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/](https://console.aws.amazon.com/personalize/) and sign into your account\. 

1. Navigate to the dataset groups page and choose the dataset group with your new solution\.

1. In the navigation pane, choose **Solutions and recipes**\. 

1. Choose the solution\. The solution overview page displays\.

1. Choose **Create solution version** to start training a new model\.

   On the solution details page, you can track training progress in the **Solution versions** section\. When training is complete, the status is **Active** and you are ready to deploy a campaign and get recommendations\. See step 3 in the [Getting Started \(Console\)](getting-started-console.md) tutorial\.

## Creating a Solution Version \(AWS CLI\)<a name="create-solution-version-cli"></a>

When your solution is ACTIVE, train the model by running the following command\. Replace `solution arn` with the solution Amazon Resource Name \(ARN\) from [Step 2: Configuring a Solution](customizing-solution-config.md)\.

```
aws personalize create-solution-version \
  --solution-arn solution arn
```

The solution version ARN is displayed, for example:

```
{
  "solutionVersionArn": "arn:aws:personalize:us-west-2:acct-id:solution/SolutionName/<version-id>"
}
```

Check the training status of the solution version by using the `describe-solution-version` command\. Provide the solution version ARN that was returned in the previous step\. For more information about the API, see [DescribeSolutionVersion](API_DescribeSolutionVersion.md)\.

```
aws personalize describe-solution-version \
  --solution-version-arn solution version arn
```

The properties of the solution version and the training `status` are displayed\. Initially, the status shows as CREATE PENDING, for example:

```
{
  "solutionVersion": {
      "solutionVersionArn": "arn:aws:personalize:us-west-2:acct-id:solution/solutionName/<version-id>",
      ...,
      "status": "CREATE PENDING"
  }
}
```

Training is complete when the `status` is `ACTIVE`\.

Now that you have created a solution version, evaluate it using metrics supplied by Amazon Personalize\. For more information, see [Step 4: Evaluating the Solution Version](working-with-training-metrics.md)\.

## Creating a Solution Version \(AWS Python SDK\)<a name="create-solution-version-sdk"></a>

 When your solution is ACTIVE, Create a solution version using the `create_solution` method\. Replace the `solution arn` with the Amazon Resource Name \(ARN\) of the solution from [Step 2: Configuring a Solution](customizing-solution-config.md)\. 

```
import boto3

personalize = boto3.client('personalize')
# Store the solution ARN
solution_arn = 'solution arn'
        
# Use the solution ARN to get the solution status.
solution_description = personalize.describe_solution(solutionArn = 'solution_arn')['solution']
print('Solution status: ' + solution_description['status'])

# Use the solution ARN to create a solution version.
print ('Creating solution version')
response = personalize.create_solution_version(solutionArn = solution_arn)
solution_version_arn = response['solutionVersionArn']
print('Solution version ARN: ' + solution_version_arn)

# Use the solution version ARN to get the solution version status.
solution_version_description = personalize.describe_solution_version(
    solutionVersionArn = solution_version_arn)['solutionVersion']
print('Solution version status: ' + solution_version_description['status'])
```

To check the current solution version status, call the [DescribeSolutionVersion](API_DescribeSolutionVersion.md) operation and pass the ARN of the solution version returned from the `CreateSolutionVersion` operation\. Training is complete when the `status` is `ACTIVE`\.

Now that you have a created solution version, evaluate it using metrics supplied by Amazon Personalize\. For more information, see [Step 4: Evaluating the Solution Version](working-with-training-metrics.md)\.