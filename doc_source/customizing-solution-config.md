# Step 2: Configuring a Solution<a name="customizing-solution-config"></a>

 Once you complete [Step 1: Choosing a Recipe](working-with-predefined-recipes.md), you are ready to configure a solution for training a model\. 

 Configuring a solution allows you customize training so the model meets your specific business needs\. To configure a solution, you specify the dataset group with the data to be used for training, the recipe to be used for training, and any additional solution parameters and recipe\-specific hyperparameters\. If your Interactions training data includes `EVENT_TYPE` and `EVENT_VALUE` data, when you configure a solution you can filter out Interactions data before training\. 

 You can create and configure a solution using the console, AWS Command Line Interface \(AWS CLI\), or AWS SDK\. 

**Topics**
+ [Configuring a Solution \(Console\)](#configure-solution-console)
+ [Configuring a Solution \(AWS CLI\)](#configure-solution-cli)
+ [Configuring a Solution \(AWS Python SDK\)](#configure-solution-sdk)
+ [Hyperparameters and HPO](customizing-solution-config-hpo.md)
+ [Filtering Interactions Data Before Training](event-values-types.md)

## Configuring a Solution \(Console\)<a name="configure-solution-console"></a>

 To configure a solution in the console, choose the dataset group containing the dataset you'll be using, and then specify a solution name, recipe, and optional recipe specific hyperparameters\. 

**To configure a solution using the console**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/](https://console.aws.amazon.com/personalize/) and sign in to your account\.

1. Choose the dataset group you want to use for training\.

1. On the dashboard, in the Create solutions section, choose the **Start** button\. 

    If you have already created a solution, choose the **Create solution** button\. 

1. For **Solution name**, specify a name for your solution\.

1. For **Recipe**, choose a recipe \(see [Step 1: Choosing a Recipe](working-with-predefined-recipes.md)\)\. 

1.  Expand the **Solution configuration** section and, if you have added the respective columns to your Interactions dataset, optionally provide **Event type** and **Event value threshold** values to filter interactions data before training \(for more information see [Filtering Interactions Data Before Training](event-values-types.md)\)\. 

    If your Interactions dataset has multiple event types in an EVENT\_TYPE column, and you do not provide an event type when you configure your Solution, Amazon Personalize uses all interactions data for training with equal weight regardless of type\. 

1. Configure any hyperparameter options based on your recipe and business needs\. Different recipes use different hyperparameters\. For the available hyperparameters, see the individual recipes in [Step 1: Choosing a Recipe](working-with-predefined-recipes.md)\. 

    An example of the Solution configuration options for the `aws-user-personalization` recipe is below\.   
![\[Solution configuration options for the aws-user-personalization recipe.\]](http://docs.aws.amazon.com/personalize/latest/dg/images/user-personalization-solution-configuration.png)

1. Choose **Next**\. The Create Solution Version page displays\. Proceed to [Creating a Solution Version \(Console\)](creating-a-solution-version.md#create-solution-version-console)\.

## Configuring a Solution \(AWS CLI\)<a name="configure-solution-cli"></a>

 To configure a solution using AWS CLI use the following `create-solution` operation\. Specify the `solution name`, `dataset group arn`, and `recipe arn` using the following code\. 

```
aws personalize create-solution \
  --name solution name \
  --dataset-group-arn dataset group arn \
  --recipe-arn recipe arn
```

The solution Amazon Resource Name \(ARN\) is displayed, for example:

```
{
  "solutionArn": "arn:aws:personalize:<region>:solution/<solution name>"
}
```

You can modify the above code to optimize recipe properties and hyperparameters \(see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\) or filter the Interactions data used for training \(see [Filtering Interactions Data Before Training](event-values-types.md)\)\.

Record the solution ARN for future use and proceed to [Creating a Solution Version \(AWS CLI\)](creating-a-solution-version.md#create-solution-version-cli)\.

## Configuring a Solution \(AWS Python SDK\)<a name="configure-solution-sdk"></a>

Create a new solution using the `create_solution` method\. Replace `solution name` with your solution name, `recipe arn` with the recipe Amazon Rescource Name \(ARN\) from [Step 1: Choosing a Recipe](working-with-predefined-recipes.md), and `dataset group arn` with the ARN of your dataset group\.

```
import boto3

personalize = boto3.client('personalize')

print('Creating solution')
create_solution_response = personalize.create_solution(
  name='solution name', 
  recipeArn= 'recipe arn', 
  datasetGroupArn = 'dataset group arn'
)
solution_arn = create_solution_response['solutionArn']
print('solution_arn: ', solution_arn)
```

You can modify the above code to optimize recipe properties and hyperparameters \(see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\) or filter the Interactions data used for training \(see [Filtering Interactions Data Before Training](event-values-types.md)\)\.

Record the solution ARN for future use and proceed to [Creating a Solution Version \(AWS Python SDK\)](creating-a-solution-version.md#create-solution-version-sdk)\.