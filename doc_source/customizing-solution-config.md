# Step 2: Configuring a solution<a name="customizing-solution-config"></a>

 Once you complete [Step 1: Choosing a recipe](working-with-predefined-recipes.md), you are ready to configure a solution for training a model\. 

 Configuring a solution allows you customize training so the model meets your specific business needs\. To configure a solution, you specify the dataset group with the data to be used for training, the recipe to be used for training, and any additional solution parameters and recipe\-specific hyperparameters\. If your Interactions training data includes `EVENT_TYPE` and `EVENT_VALUE` data, when you configure a solution you can filter out Interactions data before training\. 

You can create and configure a solution using the console, AWS Command Line Interface \(AWS CLI\), or AWS SDK\.

**Topics**
+ [Configuring a solution \(console\)](#configure-solution-console)
+ [Configuring a solution \(AWS CLI\)](#configure-solution-cli)
+ [Configuring a solution \(AWS SDKs\)](#configure-solution-sdk)
+ [Optimizing a solution for an additional objective](optimizing-solution-for-objective.md)
+ [Hyperparameters and HPO](customizing-solution-config-hpo.md)
+ [Choosing the interactions data used for training](event-values-types.md)

## Configuring a solution \(console\)<a name="configure-solution-console"></a>

 To configure a solution in the console, choose the dataset group containing the dataset you'll be using, and then specify a solution name, recipe, and optional recipe specific hyperparameters\. 

**To configure a solution \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. Go to the dataset groups page and choose the dataset group you want to use for training\.

1. On the **Overview** page choose the **Create solution** button\. 

1. For **Solution name**, specify a name for your solution\.

1. For **Solution type**, choose either **Item recommendation** to get item recommendations for your users, or choose **User segmentation** to get user segments \(groups of users\) based on your item data\. 

1. For **Recipe**, choose a recipe \(see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\)\. 

1. In **Solution configuration**, if your Interactions dataset has EVENT\_TYPE or both EVENT\_TYPE and EVENT\_VALUE columns, optionally use the **Event type** and **Event value threshold** fields to choose the interactions data that Amazon Personalize uses when training the model\. 

    For more information see [Choosing the interactions data used for training](event-values-types.md)\. 

1. If you use either the [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md) or [Personalized\-Ranking recipe](native-recipe-search.md) recipe, optionally specify an **Objective** and choose an **Objective sensitivity** to optimize your solution for an objective in addition to relevance\. For more information see [Optimizing a solution for an additional objective](optimizing-solution-for-objective.md)\.

1. Configure any hyperparameter options based on your recipe and business needs\. Different recipes use different hyperparameters\. For the available hyperparameters, see the individual recipes in [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\. 

1. For **Tags**, optionally add any tags\. For more information about tagging Amazon Personalize resources, see [Tagging Amazon Personalize resources](tagging-resources.md)\.

1. Choose **Create and train solution**\. The **Dashboard** page displays\. Proceed to [Creating a solution version \(console\)](creating-a-solution-version.md#create-solution-version-console)\.

## Configuring a solution \(AWS CLI\)<a name="configure-solution-cli"></a>

 To configure a solution using the AWS CLI use the following `create-solution` operation\. Specify the `solution name`, `dataset group arn`, and `recipe arn`\.

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

If you use either the [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md) or [Personalized\-Ranking recipe](native-recipe-search.md) recipe, you can optimize your solution for an objective in addition to relevance\. For more information see [Optimizing a solution for an additional objective](optimizing-solution-for-objective.md)\.

Record the solution ARN for future use and proceed to [Creating a solution version \(AWS CLI\)](creating-a-solution-version.md#create-solution-version-cli)\.

## Configuring a solution \(AWS SDKs\)<a name="configure-solution-sdk"></a>

The following code shows how to create an Amazon Personalize solution\. Give the solution a name, and specify the Amazon Resource Name \(ARN\) of your dataset group, and the ARN of the recipe to use\. For information on recipes, see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\. 

You can modify the following code to optimize recipe properties and hyperparameters \(see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\) or filter the Interactions data used for training \(see [Choosing the interactions data used for training](event-values-types.md)\)\. If you use either the [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md) or [Personalized\-Ranking recipe](native-recipe-search.md) recipe, you can optimize your solution for an objective in addition to relevance\. For more information see [Optimizing a solution for an additional objective](optimizing-solution-for-objective.md)\.

------
#### [ SDK for Python \(Boto3\) ]

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

------
#### [ SDK for Java 2\.x ]

```
    public static String createPersonalizeSolution(PersonalizeClient personalizeClient,
                                                 String datasetGroupArn,
                                                 String solutionName,
                                                 String recipeArn) {

        try {
            CreateSolutionRequest solutionRequest = CreateSolutionRequest.builder()
                .name(solutionName)
                .datasetGroupArn(datasetGroupArn)
                .recipeArn(recipeArn)
                .build();

            CreateSolutionResponse solutionResponse = personalizeClient.createSolution(solutionRequest);
            return solutionResponse.solutionArn();

        } catch (PersonalizeException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        return "";
    }
```

------
#### [ SDK for JavaScript v3 ]

```
// Get service clients module and commands using ES6 syntax.
import { CreateSolutionCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";
// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the solution parameters.
export const createSolutionParam = {
  datasetGroupArn: 'DATASET_GROUP_ARN', /* required */
  recipeArn: 'RECIPE_ARN', /* required */
  name: 'NAME' /* required */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateSolutionCommand(createSolutionParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

------

Record the solution ARN for future use and proceed to [Creating a solution version \(AWS SDKs\)](creating-a-solution-version.md#create-solution-version-sdk)\.