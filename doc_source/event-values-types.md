# Choosing the interactions data used for training<a name="event-values-types"></a>

You can choose the events in an Interactions dataset that Amazon Personalize uses when creating a solution version \(training a model\)\. Choosing interactions data before training allows you to use only a relevant subset of your data for training or remove noise to train a more optimized model\. For more information about Interactions datasets, see [Datasets and schemas](how-it-works-dataset-schema.md) and [Interactions data](interactions-datasets.md)\.

You can choose interactions data as follows:
+ **Choose records based on type** – When you configure a solution, if your Interactions dataset includes event types in an EVENT\_TYPE column, you can optionally specify an event type to use in training\. For example, if your Interactions dataset includes *purchase*, *click*, and *watch* event types, and you want Amazon Personalize to train the model with only *watch* events, when you configure your solution, you would provide *watch* as the `event type` that Amazon Personalize uses in training\. 

   If your Interactions dataset has multiple event types in an EVENT\_TYPE column, and you do not provide an event type when you configure your solution, Amazon Personalize uses all interactions data for training with equal weight regardless of type\. 
+ **Choose records based on type and value ** – When you configure a solution, if your Interactions dataset includes EVENT\_TYPE and EVENT\_VALUE fields, you can set a specific value as a threshold to exclude records from training\. For example, if your EVENT\_VALUE data for events with an EVENT\_TYPE of *watch* is the percentage of a video that a user watched, if you set the event value threshold to 0\.5, and the event type to *watch*, Amazon Personalize trains the model using only *watch* interaction events with an EVENT\_VALUE greater than or equal to 0\.5\. 

## Filtering records by event value and event type \(AWS SDK\)<a name="event-values-types-example"></a>

In the following procedure, you use the AWS SDK for Python \(Boto3\) to create an Interaction schema that filters a training dataset\. You can use a Jupyter \(iPython\) notebook to accomplish the same task\. For more information, see [Getting started using Amazon Personalize APIs with Jupyter \(iPython\) notebooks](getting-started-python.md#gs-jupyter-notebook)\.

**Prerequisites: **Complete the prerequisites and verify that your Python environment is set up as described in [Getting started \(SDK for Python \(Boto3\)\)](getting-started-python.md)\.

**To filter records used in a training dataset by event value or event type**

1. Create an Interaction schema and include the `EVENT_TYPE` and `EVENT_VALUE` fields using `"name"` and `"type"` key\-value pairs as shown\.

   ```
   import boto3
   import json
    
    
   personalize = boto3.client('personalize')
    
   # Create a name for your schema
   schema_name = 'YourSchemaName'
    
   # Define the schema for your dataset
   schema = {
       "type": "record",
       "name": "Interactions",
       "namespace": "com.amazonaws.personalize.schema",
       "fields": [
           {
               "name": "USER_ID",
               "type": "string"
           },
           {
               "name": "ITEM_ID",
               "type": "string"
           },
           {
               "name": "EVENT_VALUE",
               "type": "float"
           },
           {
               "name": "EVENT_TYPE",
               "type": "string"
           },
           { 
               "name": "TIMESTAMP",
               "type": "long"
           }
       ],
       "version": "1.0"
   }
    
   # Create the schema for Amazon Personalize
   create_schema_response = personalize.create_schema(
       name = schema_name,
       schema = json.dumps(schema)
   )
    
   #To get the schema ARN, use the following lines
   schema_arn = create_schema_response['schemaArn']
   print('Schema ARN:' + schema_arn )
   ```

1. Format your input data to match your schema\. For a code sample, see [Formatting your input data](data-prep-formatting.md)\.

1. Upload your data to an Amazon Simple Storage Service \(Amazon S3\) bucket\. For a code sample, see [Uploading to an Amazon S3 bucket](data-prep-upload-s3.md)\.

1. Import your data into Amazon Personalize with the [ CreateDatasetImportJob ](API_CreateDatasetImportJob.md) API\. Be sure to record your dataset group Amazon Resource Name \(ARN\) because you will need it when you create the solution\. For a code sample, see [Importing bulk records \(AWS SDKs\)](bulk-data-import-step.md#python-import-ex)\.

1. Get the ARN of the recipe that you want to use when you create your solution\. You'll need it when you create the solution\.

   ```
   import boto3
    
   personalize = boto3.client('personalize')
   
   # Display the ARNs of the recipes
   recipe_list = personalize.list_recipes()
   for recipe in recipe_list['recipes']:
       print(recipe['recipeArn'])
       
   # Store the ARN of the recipe that you want to use
    recipe_arn = "arn:aws:personalize:::recipe/aws-recipe-name"
   ```

1. Call the [ CreateSolution ](API_CreateSolution.md) API\. If you want to specify the event type, for example `“purchase”`, set it in the `eventType` parameter\. If you want to specify an event value, for example `10`, set it in the `eventValueThreshold` parameter\. You can also specify both an event type and an event value\.

   ```
   import boto3
    
   personalize = boto3.client('personalize')
   
   # Create the solution
   create_solution_response = personalize.create_solution(
       name = "your-solution-name",
       datasetGroupArn = dataset_group_arn,
       recipeArn = recipe_arn,
       eventType = 'watched',
       solutionConfig = {
           "eventValueThreshold": "0.5"
       }
   )
   
   # Store the solution ARN
   solution_arn = create_solution_response['solutionArn']
   
   # Use the solution ARN to get the solution status
   solution_description = personalize.describe_solution(solutionArn = solution_arn)['solution']
   print('Solution status: ' + solution_description['status'])
   ```

1. When you have the solution, use it to train a model by specifying its solution ARN in a [ CreateSolutionVersion ](API_CreateSolutionVersion.md) request\.

   ```
   import boto3
    
   personalize = boto3.client('personalize')
   
   # Create a solution version
   create_solution_version_response = personalize.create_solution_version(solutionArn = solution_arn)
   
   
   # Store the solution version ARN
   solution_version_arn = create_solution_version_response['solutionVersionArn']
       
   # Use the solution version ARN to get the solution version status.
   solution_version_description = personalize.describe_solution_version(
       solutionVersionArn = solution_version_arn)['solutionVersion']
   print('Solution version status: ' + solution_version_description['status'])
   ```

Training is complete when the status is `ACTIVE`\. For more information, see [Creating a solution](training-deploying-solutions.md)\.

After you train a model, you should evaluate its performance\. To optimize your model, you might want to adjust the `eventValueThreshold` or other hyperparameters\. For more information, see [Step 4: Evaluating a solution version with metrics](working-with-training-metrics.md)\. 