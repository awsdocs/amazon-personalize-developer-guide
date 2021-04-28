# Step 2: Configuring a solution<a name="customizing-solution-config"></a>

 Once you complete [Step 1: Choosing a recipe](working-with-predefined-recipes.md), you are ready to configure a solution for training a model\. 

 Configuring a solution allows you customize training so the model meets your specific business needs\. To configure a solution, you specify the dataset group with the data to be used for training, the recipe to be used for training, and any additional solution parameters and recipe\-specific hyperparameters\. If your Interactions training data includes `EVENT_TYPE` and `EVENT_VALUE` data, when you configure a solution you can filter out Interactions data before training\. 

 You can create and configure a solution using the console, AWS Command Line Interface \(AWS CLI\), or AWS SDK\. 

**Topics**
+ [Configuring a solution \(console\)](#configure-solution-console)
+ [Configuring a solution \(AWS CLI\)](#configure-solution-cli)
+ [Configuring a solution \(AWS Python SDK\)](#configure-solution-sdk)
+ [Hyperparameters and HPO](customizing-solution-config-hpo.md)
+ [Choosing the interactions data used for training](event-values-types.md)

## Configuring a solution \(console\)<a name="configure-solution-console"></a>

 To configure a solution in the console, choose the dataset group containing the dataset you'll be using, and then specify a solution name, recipe, and optional recipe specific hyperparameters\. 

**To configure a solution using the console**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. Choose the dataset group you want to use for training\.

1. On the dashboard, in the Create solutions section, choose the **Start** button\. 

    If you have already created a solution, choose the **Create solution** button\. 

1. For **Solution name**, specify a name for your solution\.

1. For **Recipe**, choose a recipe \(see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\)\. 

1.  In **Solution configuration** optionally specify the following: 
   +  **Event type:** If your data has multiple event types in an EVENT\_TYPE column, optionally enter an event type, such as click or download, to choose the events to train with based on type\. Amazon Personalize will use only events with this type when training a model\. If you don't provide an event type, Amazon Personalize trains the model with all interactions data regardless of type\. 
   +  **Event value threshold:** If your Interactions dataset has an EVENT\_VALUE\_THRESHOLD column, enter a value to choose the events to train with based on value\. Amazon Personalize will use only events with a value greater than or equal to this value to train the model\. If you don't provide a value, Amazon Personalize trains the model with all interactions data regardless of value\. 

    For more information see [Choosing the interactions data used for training](event-values-types.md)\. 

1. Configure any hyperparameter options based on your recipe and business needs\. Different recipes use different hyperparameters\. For the available hyperparameters, see the individual recipes in [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\. 

    An example of the Solution configuration options for the `aws-user-personalization` recipe is below\.   
![\[Solution configuration options for the aws-user-personalization recipe.\]](http://docs.aws.amazon.com/personalize/latest/dg/images/user-personalization-solution-configuration.png)

1. Choose **Next**\. The Create Solution Version page displays\. Proceed to [Creating a solution version \(console\)](creating-a-solution-version.md#create-solution-version-console)\.

## Configuring a solution \(AWS CLI\)<a name="configure-solution-cli"></a>

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

You can modify the above code to optimize recipe properties and hyperparameters \(see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\) or filter the Interactions data used for training \(see [Choosing the interactions data used for training](event-values-types.md)\)\.

Record the solution ARN for future use and proceed to [Creating a solution version \(AWS CLI\)](creating-a-solution-version.md#create-solution-version-cli)\.

## Configuring a solution \(AWS Python SDK\)<a name="configure-solution-sdk"></a>

Create a new solution using the `create_solution` method\. Replace `solution name` with your solution name, `recipe arn` with the recipe Amazon Rescource Name \(ARN\) from [Step 1: Choosing a recipe](working-with-predefined-recipes.md), and `dataset group arn` with the ARN of your dataset group\.

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

You can modify the above code to optimize recipe properties and hyperparameters \(see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\) or filter the Interactions data used for training \(see [Choosing the interactions data used for training](event-values-types.md)\)\.

Record the solution ARN for future use and proceed to [Creating a solution version \(AWS Python SDK\)](creating-a-solution-version.md#create-solution-version-sdk)\.