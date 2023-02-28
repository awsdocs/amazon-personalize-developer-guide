# User\-Personalization recipe<a name="native-recipe-new-item-USER_PERSONALIZATION"></a>

The User\-Personalization \(aws\-user\-personalization\) recipe is optimized for all personalized recommendation scenarios\. It predicts the items that a user will interact with based on Interactions, Items, and Users datasets\. When recommending items, it uses automatic item exploration\.

With automatic exploration, Amazon Personalize automatically tests different item recommendations, learns from how users interact with these recommended items, and boosts recommendations for items that drive better engagement and conversion\. This improves item discovery and engagement when you have a fast\-changing catalog, or when new items, such as news articles or promotions, are more relevant to users when fresh\. 

You can balance how much to explore \(where items with less interactions data or relevance are recommended more frequently\) against how much to exploit \(where recommendations are based on what we know or relevance\)\. Amazon Personalize automatically adjusts future recommendations based on implicit user feedback\.

**Topics**
+ [Automatic updates](#automatic-updates)
+ [Working with impressions data](#working-with-impressions)
+ [Properties and hyperparameters](#bandit-hyperparameters)
+ [Training with the User\-Personalization recipe \(console\)](#training-user-personalization-recipe-console)
+ [Training with the User\-Personalization recipe \(Python SDK\)](#training-user-personalization-recipe)
+ [Getting recommendations and recording impressions \(SDK for Python \(Boto3\)\)](#user-personalization-get-recommendations-recording-impressions)
+ [Sample Jupyter notebook](#bandits-sample-notebooks)

## Automatic updates<a name="automatic-updates"></a>

 With User\-Personalization, Amazon Personalize automatically updates the latest model \(solution version\) every two hours behind the scenes to include new data\. There is no cost for automatic updates\. The solution version must be deployed with an [Amazon Personalize campaign](campaigns.md) for updates to occur\. Your campaign automatically uses the updated solution version\. No new solution version is created when an auto update completes and no new model metrics are generated\. This is because no full retraining occurs\. If you create a new solution version, Amazon Personalize will not automatically update older solution versions, even if you have deployed them in a campaign\. Updates also do not occur if you have deleted your dataset\. 

 With each update, Amazon Personalize updates the solution version with the latest item information and adjusts the exploration according to implicit feedback from users\. This allows Amazon Personalize to gauge item quality based on new interactions for already explored items and continually update item exploration\. This is not a full retraining; you should still train a new solution version weekly with `trainingMode` set to `FULL` so the model can learn from your users' behavior\. 

If every two hours is not frequent enough, you can manually create a solution version with `trainingMode` set to `UPDATE` to include those new items in recommendations\. Just remember that Amazon Personalize automatically updates only your latest fully trained solution version, so the manually updated solution version won't be automatically updated in the future\.

**Automatic update requirements**

Automatic update requirements include the following:
+ You must deploy the solution version with a campaign \(for more information see [Creating a campaign](campaigns.md)\)\. The campaign automatically uses the latest automatically updated solution version\. 
+ The solution version must be trained with `trainingMode` set to `FULL` \(this is the default when creating a solution version\)\.
+ You must provide new item or interactions data since the last automatic update\.
+  Amazon Personalize automatically updates only solution versions you created on or after November 17, 2020\. 

## Working with impressions data<a name="working-with-impressions"></a>

 Unlike other recipes, which solely use positive interactions \(clicking, watching, or purchasing\), the User\-Personalization recipe can also use impressions data\. Impressions are lists of items that were visible to a user when they interacted with \(clicked, watched, purchased, and so on\) a particular item\. 

Using this information, a solution created with the User\-Personalization recipe can calculate the suitability of new items based on how frequently an item has been ignored, and change recommendations accordingly\. For more information see [Impressions data](interactions-datasets.md#interactions-impressions-data)\. 

## Properties and hyperparameters<a name="bandit-hyperparameters"></a>

The User\-Personalization recipe has the following properties:
+  **Name** – `aws-user-personalization`
+  **Recipe Amazon Resource Name \(ARN\)** – `arn:aws:personalize:::recipe/aws-user-personalization`
+  **Algorithm ARN** – `arn:aws:personalize:::algorithm/aws-user-personalization`

For more information, see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\.

The following table describes the hyperparameters for the User\-Personalization recipe\. A *hyperparameter* is an algorithm parameter that you can adjust to improve model performance\. Algorithm hyperparameters control how the model performs\. Featurization hyperparameters control how to filter the data to use in training\. The process of choosing the best value for a hyperparameter is called hyperparameter optimization \(HPO\)\. For more information, see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\. 

The table also provides the following information for each hyperparameter:
+ **Range**: \[lower bound, upper bound\]
+ **Value type**: Integer, Continuous \(float\), Categorical \(Boolean, list, string\)
+ **HPO tunable**: Can the parameter participate in HPO?


| Name | Description | 
| --- | --- | 
| Algorithm hyperparameters | 
| hidden\_dimension |  The number of hidden variables used in the model\. *Hidden variables* recreate users' purchase history and item statistics to generate ranking scores\. Specify a greater number of hidden dimensions when your Interactions dataset includes more complicated patterns\. Using more hidden dimensions requires a larger dataset and more time to process\. To decide on the best value, use HPO\. To use HPO, set `performHPO` to `true` when you call [CreateSolution](API_CreateSolution.md) and [CreateSolutionVersion](API_CreateSolutionVersion.md) operations\. Default value: 149 Range: \[32, 256\] Value type: Integer HPO tunable: Yes  | 
| bptt |  Determines whether to use the back\-propagation through time technique\. *Back\-propagation through time* is a technique that updates weights in recurrent neural network\-based algorithms\. Use `bptt` for long\-term credits to connect delayed rewards to early events\. For example, a delayed reward can be a purchase made after several clicks\. An early event can be an initial click\. Even within the same event types, such as a click, it’s a good idea to consider long\-term effects and maximize the total rewards\. To consider long\-term effects, use larger `bptt` values\. Using a larger `bptt` value requires larger datasets and more time to process\. Default value: 32 Range: \[2, 32\] Value type: Integer HPO tunable: Yes  | 
| recency\_mask |  Determines whether the model should consider the latest popularity trends in the Interactions dataset\. Latest popularity trends might include sudden changes in the underlying patterns of interaction events\. To train a model that places more weight on recent events, set `recency_mask` to `true`\. To train a model that equally weighs all past interactions, set `recency_mask` to `false`\. To get good recommendations using an equal weight, you might need a larger training dataset\. Default value: `True` Range: `True` or `False` Value type: Boolean HPO tunable: Yes  | 
| Featurization hyperparameters | 
| min\_user\_history\_length\_percentile |  The minimum percentile of user history lengths to include in model training\. *History length* is the total amount of data about a user\. Use `min_user_history_length_percentile` to exclude a percentage of users with short history lengths\. Users with a short history often show patterns based on item popularity instead of the user's personal needs or wants\. Removing them can train models with more focus on underlying patterns in your data\. Choose an appropriate value after you review user history lengths, using a histogram or similar tool\. We recommend setting a value that retains the majority of users, but removes the edge cases\.  For example, setting `min_user_history_length_percentile to 0.05` and `max_user_history_length_percentile to 0.95` includes all users except those with history lengths at the bottom or top 5%\. Default value: 0\.0 Range: \[0\.0, 1\.0\] Value type: Float HPO tunable: No  | 
| max\_user\_history\_length\_percentile |  The maximum percentile of user history lengths to include in model training\. *History length* is the total amount of data about a user\. Use `max_user_history_length_percentile` to exclude a percentage of users with long history lengths because data for these users tend to contain noise\. For example, a robot might have a long list of automated interactions\. Removing these users limits noise in training\. Choose an appropriate value after you review user history lengths using a histogram or similar tool\. We recommend setting a value that retains the majority of users but removes the edge cases\. For example, setting `min_user_history_length_percentile to 0.05` and `max_user_history_length_percentile to 0.95` includes all users except those with history lengths at the bottom or top 5%\. Default value: 0\.99 Range: \[0\.0, 1\.0\] Value type: Float HPO tunable: No  | 
| Item exploration campaign configuration hyperparameters | 
| exploration\_weight |  Determines how frequently recommendations include items with less interactions data or relevance\. The closer the value is to 1\.0, the more exploration\. At zero, no exploration occurs and recommendations are based on current data \(relevance\)\. For more information see [CampaignConfig](API_CampaignConfig.md)\. Default value: 0\.3 Range: \[0\.0, 1\.0\] Value type: Float HPO tunable: No  | 
| exploration\_item\_age\_cut\_off |  Determines items to be explored based on time frame since latest interaction\. Provide the maximum item age, in days since the latest interaction, to define the scope of item exploration\. The larger the value, the more items are considered during exploration\. For more information see [CampaignConfig](API_CampaignConfig.md)\. Default value: 30\.0 Range: Positive floats Value type: Float HPO tunable: No  | 

## Training with the User\-Personalization recipe \(console\)<a name="training-user-personalization-recipe-console"></a>

To use the User\-Personalization recipe to generate recommendations in the console, first train a new solution version using the recipe\. Then deploy a campaign using the solution version and use the campaign to get recommendations\. 

**Training a new solution version with the User\-Personalization recipe \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\.

1. Create a Custom dataset group with a new schema and upload your dataset with impressions data\. Optionally include [CREATION\_TIMESTAMP]() and [Unstructured text metadata](items-datasets.md#text-data) data in your Items dataset so Amazon Personalize can more accurately calculate the age of an item and identify cold items\.

   For more information on importing data, see [Preparing and importing data](data-prep.md)\.

1. On the **Dataset groups** page, choose the new dataset group that contains the dataset or datasets with impressions data\.

1. In the navigation pane, choose **Solutions and recipes** and choose **Create solution**\.

1. On the **Create solution** page, for the **Solution name**, enter the name of your new solution\.

1. For **Solution type**, choose **Item recommendation** to get item recommendations for your users\. 

1. For **Recipe**, choose **aws\-user\-personalization**\. The **Solution configuration** section appears providing several configuration options\. 

1. In **Solution configuration**, if your Interactions dataset has EVENT\_TYPE or both EVENT\_TYPE and EVENT\_VALUE columns, optionally use the **Event type** and **Event value threshold** fields to choose the interactions data that Amazon Personalize uses when training the model\. 

    For more information see [Choosing the interactions data used for training](event-values-types.md)\. 

1. Optionally configure hyperparameters for your solution\. For a list of User\-Personalization recipe properties and hyperparameters, see [Properties and hyperparameters](#bandit-hyperparameters)\. 

1. Choose **Create and train solution** to start training\. The **Dashboard** page displays\.

   You can navigate to the solution details page to track training progress in the **Solution versions** section\. When training is complete, the status is **Active**\.

**Creating a campaign and getting recommendations \(console\)**

 When your solution version status is **Active** you are ready to create your campaign and get recommendations as follows: 

1. On either the solution details page or the **Campaigns** page, choose **Create new campaign**\.

1.  On the **Create new campaign** page, for **Campaign details**, provide the following information: 
   + **Campaign name:** Enter the name of the campaign\. The text you enter here appears on the Campaign dashboard and details page\.
   + **Solution:** Choose the solution that you just created\.
   + **Solution version ID:** Choose the ID of the solution version that you just created\.
   + **Minimum provisioned transactions per second:** Set the minimum provisioned transactions per second that Amazon Personalize supports\. For more information, see the [CreateCampaign](API_CreateCampaign.md) operation\.

1. For **Campaign configuration**, provide the following information:
   + **Exploration weight:** Configure how much to explore, where recommendations include items with less interactions data or relevance more frequently the more exploration you specify\. The closer the value is to 1, the more exploration\. At zero, no exploration occurs and recommendations are based on current data \(relevance\)\.
   + **Exploration item age cut off**: Enter the maximum item age, in days since the latest interaction, to define the scope of item exploration\. To increase the number of items Amazon Personalize considers during exploration, enter a greater value\. 

      For example, if you enter 10, only items with interactions data from the 10 days since the latest interaction in the dataset are considered during exploration\. 
**Note**  
Recommendations might include items without interactions data from outside this time frame\. This is because these items are relevant to the user's interests, and exploration wasn't required to identify them\.

1. Choose **Create campaign**\.

1. On the campaign details page, when the campaign status is **Active**, you can use the campaign to get recommendations and record impressions\. For more information, see [Step 5: Get recommendations](getting-started-console.md#getting-started-console-get-recommendations) in "Getting Started\." 

    Amazon Personalize automatically updates your latest solution version every two hours to include new data\. Your campaign automatically uses the updated solution version\. For more information see [Automatic updates](#automatic-updates)\. 

   To manually update the campaign, you first create and train a new solution version using the console or the [CreateSolutionVersion](API_CreateSolutionVersion.md) operation, with `trainingMode` set to `update`\. You then manually update the campaign on the **Campaign** page of the console or by using the [UpdateCampaign](API_UpdateCampaign.md) operation\. 
**Note**  
 Amazon Personalize doesn't automatically update solution versions you created before November 17, 2020\. 

## Training with the User\-Personalization recipe \(Python SDK\)<a name="training-user-personalization-recipe"></a>

When you have created a dataset group and uploaded your dataset\(s\) with impressions data, you can train a solution with the User\-Personalization recipe\. Optionally include [CREATION\_TIMESTAMP]() and [Unstructured text metadata](items-datasets.md#text-data) data in your Items dataset so Amazon Personalize can more accurately calculate the age of an item and identify cold items\. For more information on creating dataset groups and uploading training data see [Datasets and schemas](how-it-works-dataset-schema.md)\.

**To train a solution with the User\-Personalization recipe using the AWS SDK**

1. Create a new solution using the `create_solution` method\.

   Replace `solution name` with your solution name and `dataset group arn` with the Amazon Resource Name \(ARN\) of your dataset group\.

   ```
   import boto3
   
   personalize = boto3.client('personalize')
   
   print('Creating solution')
   create_solution_response = personalize.create_solution(name = 'solution name', 
                               recipeArn = 'arn:aws:personalize:::recipe/aws-user-personalization', 
                               datasetGroupArn = 'dataset group arn',
                               )
   solution_arn = create_solution_response['solutionArn']
   print('solution_arn: ', solution_arn)
   ```

   For a list of aws\-user\-personalization recipe properties and hyperparameters, see [Properties and hyperparameters](#bandit-hyperparameters)\.

1. Create a new *solution version* with the updated training data and set `trainingMode` to `FULL` using the following code snippet\. Replace the `solution arn` with the ARN of your solution\.

   ```
   import boto3
           
   personalize = boto3.client('personalize')
           
   create_solution_version_response = personalize.create_solution_version(solutionArn = 'solution arn', 
                                                                  trainingMode='FULL')
   
   new_solution_version_arn = create_solution_version_response['solutionVersionArn']
   print('solution_version_arn:', new_solution_version_arn)
   ```

1. When Amazon Personalize is finished creating your solution version, create your campaign with the following parameters:
   + Provide a new `campaign name` and the `solution version arn` generated in step 2\.
   + Modify the `explorationWeight` item exploration configuration hyperparameter to configure how much to explore\. Items with less interactions data or relevance are recommended more frequently the closer the value is to 1\.0\. The default value is 0\.3\.
   + Modify the `explorationItemAgeCutOff` item exploration configuration hyperparameter parameter to provide the maximum duration, in days relative to the latest interaction, for which items should be explored\. The larger the value, the more items are considered during exploration\.

   Use the following Python snippet to create a new campaign with an emphasis on exploration with exploration cut\-off at 30 days\. Creating a campaign usually takes a few minutes but can take over an hour\.

   ```
   import boto3
           
   personalize = boto3.client('personalize')
   
   create_campaign_response = personalize.create_campaign(
       name = 'campaign name',
       solutionVersionArn = 'solution version arn',
       minProvisionedTPS = 1,
       campaignConfig = {"itemExplorationConfig": {"explorationWeight": "0.3", "explorationItemAgeCutOff": "30"}}
   )
   
   campaign_arn = create_campaign_response['campaignArn']
   print('campaign_arn:', campaign_arn)
   ```

    With User\-Personalization, Amazon Personalize automatically updates your solution version every two hours to include new data\. Your campaign automatically uses the updated solution version\. For more information see [Automatic updates](#automatic-updates)\. 

   To manually update the campaign, you first create and train a new solution version using the console or the [CreateSolutionVersion](API_CreateSolutionVersion.md) operation, with `trainingMode` set to `update`\. You then manually update the campaign on the **Campaign** page of the console or by using the [UpdateCampaign](API_UpdateCampaign.md) operation\.
**Note**  
 Amazon Personalize doesn't automatically update solution versions you created before November 17, 2020\. 

## Getting recommendations and recording impressions \(SDK for Python \(Boto3\)\)<a name="user-personalization-get-recommendations-recording-impressions"></a>

When your campaign is created, you can use it to get recommendations for a user and record impressions\. For information on getting batch recommendations using the AWS SDKs see [Creating a batch inference job \(AWS SDKs\)](creating-batch-inference-job.md#batch-sdk)\.



**To get recommendations and record impressions**

1. Call the `get_recommendations` method\. Change the `campaign arn` to the ARN of your new campaign and `user id` to the userId of the user\.

   ```
   import boto3
               
   rec_response = personalize_runtime.get_recommendations(campaignArn = 'campaign arn', userId = 'user id')
   print(rec_response['recommendationId'])
   ```

1. Create a new event tracker for sending PutEvents requests\. Replace `event tracker name` with the name of your event tracker and `dataset group arn` with the ARN of your dataset group\.

   ```
   import boto3
           
   personalize = boto3.client('personalize')
   
   event_tracker_response = personalize.create_event_tracker( 
       name = 'event tracker name',
       datasetGroupArn = 'dataset group arn'
   )
   event_tracker_arn = event_tracker_response['eventTrackerArn']
   event_tracking_id = event_tracker_response['trackingId']
   print('eventTrackerArn:{},\n eventTrackingId:{}'.format(event_tracker_arn, event_tracking_id))
   ```

1.  Use the `recommendationId` from step 1 and the `event tracking id` from step 2 to create a new `PutEvents` request\. This request logs the new impression data from the user’s session\. Change the `user id` to the ID of the user\. 

   ```
   import boto3
               
   personalize_events.put_events(
        trackingId = 'event tracking id',
        userId= 'user id',
        sessionId = '1',
        eventList = [{
        'sentAt': datetime.now().timestamp(),
        'eventType' : 'click',
        'itemId' : rec_response['itemList'][0]['itemId'],        
        'recommendationId': rec_response['recommendationId'],
        'impression': [item['itemId'] for item in rec_response['itemList']],
        }]
   )
   ```

## Sample Jupyter notebook<a name="bandits-sample-notebooks"></a>

For a sample Jupyter notebook that shows how to use the User\-Personalization recipe, see [User Personalization with Exploration](https://github.com/aws-samples/amazon-personalize-samples/blob/master/next_steps/core_use_cases/user_personalization/user-personalization-with-exploration.ipynb)\.