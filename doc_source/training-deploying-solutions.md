# Creating a solution<a name="training-deploying-solutions"></a>

Once you have finished [Preparing and importing data](data-prep.md), you are ready to create a Solution\. A *Solution* refers to the combination of an Amazon Personalize recipe, customized parameters, and one or more solution versions \(trained models\)\. Once you create a solution with a solution version, you can [create a campaign](campaigns.md) to deploy the solution version and get recommendations\.

To create a solution in Amazon Personalize, you do the following:

1. **Choose a recipe** – A *recipe* is an Amazon Personalize term specifying an appropriate algorithm to train for a given use case\. See [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\. 

1. **Configure a solution** – Customize solution parameters and recipe\-specific hyperparameters so the model meets your specific business needs\. See [Step 2: Configuring a solution](customizing-solution-config.md)\. 

1.  **Create a solution version \(train a model\)** – Train the machine learning model Amazon Personalize will use to generate recommendations for your customers\. See [Step 3: Creating a solution version](creating-a-solution-version.md)\. 

1.  **Evaluate the solution version** – Use the metrics Amazon Personalize generates from the new solution version to evaluate the performance of the model\. See [Step 4: Evaluating a solution version](working-with-training-metrics.md)\. 