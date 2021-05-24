# Step 4: Evaluating a solution version with metrics<a name="working-with-training-metrics"></a>

 You can evaluate the performance of your solution version through offline and online metrics\. *Online metrics* are the empirical results you observe in your users' interactions with real\-time recommendations\. For example, you might record your users' click\-through rate as they browse your catalog\. You are responsible for generating and recording any online metrics\. 

 *Offline metrics* are the metrics Amazon Personalize generates when you train a solution version\. You can use offline metrics to evaluate the performance of the model before you create a campaign and provide recommendations\. Offline metrics allow you to view the effects of modifying a solution's hyperparameters or compare results from solutions that use the same training data but use different recipes\. For the rest of this section, the term metrics refers to *offline metrics*\.

 To get performance metrics, Amazon Personalize splits the input interactions data into two sets: a training set and a testing set\. The training set consists of 90% of your users and their interactions data\. The testing set consists of the remaining 10% of users and their interactions data\. Amazon Personalize then creates the solution version using the training set\. 

 When training completes, Amazon Personalize gives the solution version the oldest 90% of each user’s data from the testing set as input\. Amazon Personalize then calculates metrics by comparing the recommendations the solution version generates to the actual interactions in the newest 10% of each user’s data from the testing set\. 

To generate a baseline for comparison purposes, we recommend using the [Popularity\-Count](native-recipe-popularity.md) recipe, which recommends the top K most popular items\.

**Important**  
In order for Amazon Personalize to generate solution version metrics, you must have at least 10 datapoints in your input dataset group\.

## Retrieving Metrics<a name="working-with-training-metrics-metrics"></a>

You retrieve the metrics for a specific solution version by calling the [GetSolutionMetrics](API_GetSolutionMetrics.md) operation\.

**Retrieve metrics using the AWS Python SDK**

1. Create a solution version\. For more information, see [Creating a solution](training-deploying-solutions.md)\.

1. Use the following code to retrieve metrics\.

   ```
   import boto3
   
   personalize = boto3.client('personalize')
   
   response = personalize.get_solution_metrics(
       solutionVersionArn = 'solution version arn')
   
   print(response['metrics'])
   ```

The following is an example the output from a solution version created using the [ User\-Personalization recipeUser\-Personalization  Information about the User\-Personalization recipe   The User\-Personalization \(aws\-user\-personalization\) recipe is optimized for all personalized recommendation scenarios\. It predicts the items that a user will interact with based on Interactions, Items, and Users datasets\. When recommending items, it uses automatic item exploration\. With automatic exploration, Amazon Personalize automatically tests different item recommendations, learns from how users interact with these recommended items, and boosts recommendations for items that drive better engagement and conversion\. This improves item discovery and engagement when you have a fast\-changing catalog, or when new items, such as news articles or promotions, are more relevant to users when fresh\.  You can balance how much to explore \(where items with less interactions data or relevance are recommended more frequently\) against how much to exploit \(where recommendations are based on what we know or relevance\)\. Amazon Personalize automatically adjusts future recommendations based on implicit user feedback\.   Automatic updates   With User\-Personalization, Amazon Personalize automatically updates the latest model \(solution version\) every two hours behind the scenes to include new data without creating a new solution version\. With each update, Amazon Personalize updates the solution version with the latest item information and adjusts the exploration according to implicit feedback from users\. This allows Amazon Personalize to gauge item quality based on new interactions for already explored items and continually update item exploration\.   There is no cost for automatic updates\.  **Update requirements**  Amazon Personalize automatically updates only the latest solution version trained with `trainingMode` set to `FULL` and only if you provide new item or interactions data since the last automatic update\. If you have trained a new solution version, Amazon Personalize will not automatically update older solution versions that you have deployed in a campaign\. Updates also do not occur if you have deleted your dataset\.    Amazon Personalize automatically updates only solution versions you created on or after November 17, 2020\.      Working with impressions data   Unlike other recipes, which solely use positive interactions \(clicking, watching, or purchasing\), the User\-Personalization recipe can also use impressions data\. Impressions are lists of items that were visible to a user when they interacted with \(clicked, watched, purchased, and so on\) a particular item\.  Using this information, a solution created with the User\-Personalization recipe can calculate the suitability of new items based on how frequently an item has been ignored, and change recommendations accordingly\. For more information see [Impressions data](interactions-datasets.md#interactions-impressions-data)\.      Properties and hyperparameters  The User\-Personalization recipe has the following properties:    **Name** – `aws-user-personalization`    **Recipe Amazon Resource Name \(ARN\)** – `arn:aws:personalize:::recipe/aws-user-personalization`    **Algorithm ARN** – `arn:aws:personalize:::algorithm/aws-user-personalization`   For more information, see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\. The following table describes the hyperparameters for the User\-Personalization recipe\. A *hyperparameter* is an algorithm parameter that you can adjust to improve model performance\. Algorithm hyperparameters control how the model performs\. Featurization hyperparameters control how to filter the data to use in training\. The process of choosing the best value for a hyperparameter is called hyperparameter optimization \(HPO\)\. For more information, see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\.  The table also provides the following information for each hyperparameter:   **Range**: \[lower bound, upper bound\]   **Value type**: Integer, Continuous \(float\), Categorical \(Boolean, list, string\)   **HPO tunable**: Can the parameter participate in HPO?   


| Name | Description | 
| --- | --- | 
| Algorithm hyperparameters | 
| hidden\_dimension |  The number of hidden variables used in the model\. *Hidden variables* recreate users' purchase history and item statistics to generate ranking scores\. Specify a greater number of hidden dimensions when your Interactions dataset includes more complicated patterns\. Using more hidden dimensions requires a larger dataset and more time to process\. To decide on the optimal value, use HPO\. To use HPO, set `performHPO` to `true` when you call [CreateSolution](API_CreateSolution.md) and [CreateSolutionVersion](API_CreateSolutionVersion.md) operations\. Default value: 149 Range: \[32, 256\] Value type: Integer HPO tunable: Yes  | 
| bptt |  Determines whether to use the back\-propagation through time technique\. *Back\-propagation through time* is a technique that updates weights in recurrent neural network\-based algorithms\. Use `bptt` for long\-term credits to connect delayed rewards to early events\. For example, a delayed reward can be a purchase made after several clicks\. An early event can be an initial click\. Even within the same event types, such as a click, it’s a good idea to consider long\-term effects and maximize the total rewards\. To consider long\-term effects, use larger `bptt` values\. Using a larger `bptt` value requires larger datasets and more time to process\. Default value: 32 Range: \[2, 32\] Value type: Integer HPO tunable: Yes  | 
| recency\_mask |  Determines whether the model should consider the latest popularity trends in the Interactions dataset\. Latest popularity trends might include sudden changes in the underlying patterns of interaction events\. To train a model that places more weight on recent events, set `recency_mask` to `true`\. To train a model that equally weighs all past interactions, set `recency_mask` to `false`\. To get good recommendations using an equal weight, you might need a larger training dataset\. Default value: `True` Range: `True` or `False` Value type: Boolean HPO tunable: Yes  | 
| Featurization hyperparameters | 
| min\_user\_history\_length\_percentile |  The minimum percentile of user history lengths to include in model training\. *History length* is the total amount of data about a user\. Use `min_user_history_length_percentile` to exclude a percentage of users with short history lengths\. Users with a short history often show patterns based on item popularity instead of the user's personal needs or wants\. Removing them can train models with more focus on underlying patterns in your data\. Choose an appropriate value after you review user history lengths, using a histogram or similar tool\. We recommend setting a value that retains the majority of users, but removes the edge cases\.  For example, setting `min_user_history_length_percentile to 0.05` and `max_user_history_length_percentile to 0.95` includes all users except those with history lengths at the bottom or top 5%\. Default value: 0\.0 Range: \[0\.0, 1\.0\] Value type: Float HPO tunable: No  | 
| max\_user\_history\_length\_percentile |  The maximum percentile of user history lengths to include in model training\. *History length* is the total amount of data about a user\. Use `max_user_history_length_percentile` to exclude a percentage of users with long history lengths because data for these users tend to contain noise\. For example, a robot might have a long list of automated interactions\. Removing these users limits noise in training\. Choose an appropriate value after you review user history lengths using a histogram or similar tool\. We recommend setting a value that retains the majority of users but removes the edge cases\. For example, setting `min_user_history_length_percentile to 0.05` and `max_user_history_length_percentile to 0.95` includes all users except those with history lengths at the bottom or top 5%\. Default value: 0\.99 Range: \[0\.0, 1\.0\] Value type: Float HPO tunable: No  | 
| Item exploration campaign configuration hyperparameters | 
| exploration\_weight |  Determines how frequently recommendations include items with less interactions data or relevance\. The closer the value is to 1\.0, the more exploration\. At zero, no exploration occurs and recommendations are based on current data \(relevance\)\. For more information see [CampaignConfig](API_CampaignConfig.md)\. Default value: 0\.3 Range: \[0\.0, 1\.0\] Value type: Float HPO tunable: No  | 
| exploration\_item\_age\_cut\_off |  Determines items to be explored based on time frame since latest interaction\. Provide the maximum item age, in days since the latest interaction, to define the scope of item exploration\. The larger the value, the more items are considered during exploration\. For more information see [CampaignConfig](API_CampaignConfig.md)\. Default value: 30\.0 Range: Positive floats Value type: Float HPO tunable: No  |      Training with the User\-Personalization recipe \(console\)   To use the User\-Personalization recipe to generate recommendations in the console, first train a new solution version using the recipe\. Then deploy a campaign using the solution version and use the campaign to get recommendations\.   Training a new solution version with the User\-Personalization recipe \(console\)   Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\.   Create a dataset group with a new schema and upload your dataset with impressions data\. Optionally include [CREATION\_TIMESTAMP]() data in your Items dataset so Amazon Personalize can more accurately calculate the age of an item and identify cold items\.  For more information on importing data, see [Preparing and importing data](data-prep.md)\.   On the **Dataset groups** page, choose the new dataset group that contains the dataset or datasets with impressions data\.    In the navigation pane, choose **Solutions and recipes** and choose **Create solution**\.   On the **Create solution** page, for the **Solution name**, enter the name of your new solution\.   For **Recipe**, choose **aws\-user\-personalization**\. The **Solution configuration** section appears providing several configuration options\.    In **Solution configuration**, if your data has EVENT\_TYPE or EVENT\_VALUE\_THRESHOLD columns, use the following fields to choose the interactions data that Amazon Personalize uses when training the model\.     **Event type:** If your data has multiple event types in an EVENT\_TYPE column and you want to train with just a single type of event, optionally enter an event type, such as *click*\. Amazon Personalize will use only events with this type when training a model\. You can enter only one type\. If you don't provide an event type, Amazon Personalize trains the model with all interactions data regardless of type\.     **Event value threshold:** If your Interactions dataset has an EVENT\_VALUE\_THRESHOLD column and you want to train events based on value, optionally enter a value\. Amazon Personalize will use only events with a value greater than or equal to this value to train the model\. If you don't provide a value, Amazon Personalize trains the model with all interactions data regardless of value\.     For more information see [Choosing the interactions data used for training](event-values-types.md)\.     Optionally configure hyperparameters for your solution\. For a list of User\-Personalization recipe properties and hyperparameters, see [Properties and hyperparameters](#bandit-hyperparameters)\.    Choose **Next**\. You can review your settings on the **Create solution version** page\.   Choose **Finish** to create the solution version\. On the solution details page, you can track training progress in the **Solution versions** section\. When training is complete, the status is **Active**\.   Creating a campaign and getting recommendations \(console\)   When your solution version status is **Active** you are ready to create your campaign and get recommendations as follows:   On either the solution details page or the **Campaigns** page, choose **Create new campaign**\.    On the **Create new campaign** page, for **Campaign details**, provide the following information:    **Campaign name:** Enter the name of the campaign\. The text you enter here appears on the Campaign dashboard and details page\.   **Solution:** Choose the solution that you just created\.   **Solution version ID:** Choose the ID of the solution version that you just created\.   **Minimum provisioned transactions per second:** Set the minimum provisioned transactions per second that Amazon Personalize supports\. For more information, see the [CreateCampaign](API_CreateCampaign.md) operation\.     For **Campaign configuration**, provide the following information:     **Exploration weight:** Configure how much to explore, where recommendations include items with less interactions data or relevance more frequently the more exploration you specify\. The closer the value is to 1, the more exploration\. At zero, no exploration occurs and recommendations are based on current data \(relevance\)\.   **Exploration item age cut off**: Enter the maximum item age, in days since the latest interaction, to define the scope of item exploration\. To increase the number of items Amazon Personalize considers during exploration, enter a greater value\.   For example, if you enter 10, only items with interactions data from the 10 days since the latest interaction in the dataset are considered during exploration\.   Recommendations might include items without interactions data from outside this time frame\. This is because these items are relevant to the user's interests, and exploration wasn't required to identify them\.      Choose **Create campaign**\.   On the campaign details page, when the campaign status is **Active**, you can use the campaign to get recommendations and record impressions\. For more information, see [Step 4: Get recommendations](getting-started-console.md#getting-started-console-get-recommendations) in "Getting Started\."   Amazon Personalize automatically updates your latest solution version every two hours to include new data\. Your campaign automatically uses the updated solution version\. For more information see [Automatic updates](#automatic-updates)\.  To manually update the campaign, you first create and train a new solution version using the console or the [CreateSolutionVersion](API_CreateSolutionVersion.md) operation, with `trainingMode` set to `update`\. You then manually update the campaign on the **Campaign** page of the console or by using the [UpdateCampaign](API_UpdateCampaign.md) operation\.    Amazon Personalize doesn't automatically update solution versions you created before November 17, 2020\.        Training with the User\-Personalization recipe \(Python SDK\)  When you have created a dataset group and uploaded your dataset\(s\) with impressions data, you can train a solution with the User\-Personalization recipe\. Optionally include [CREATION\_TIMESTAMP](items-datasets.md#creation-timestamp-data) data in your dataset so Amazon Personalize can more accurately calculate the age of an item and identify cold start items\. For more information on creating dataset groups and uploading training data see [Datasets and schemas](how-it-works-dataset-schema.md)\. To train a solution with the User\-Personalization recipe using the AWS SDK  Create a new solution using the `create_solution` method\. Replace `solution name` with your solution name and `dataset group arn` with the Amazon Resource Name \(ARN\) of your dataset group\. 

   ```
   import boto3
   
   personalize = boto3.client('personalize')
   
   print('Creating solution')
   create_solution_response = personalize.create_solution(name='solution name', 
                               recipeArn= arn:aws:personalize:::recipe/aws-user-personalization, 
                               datasetGroupArn = 'dataset group arn',
                               )
   solution_arn = create_solution_response['solutionArn']
   print('solution_arn: ', solution_arn)
   ``` For a list of aws\-user\-personalization recipe properties and hyperparameters, see [Properties and hyperparameters](#bandit-hyperparameters)\.   Create a new *solution version* with the updated training data and set `trainingMode` to `FULL` using the following code snippet\. Replace the `solution arn` with the ARN of your solution\. 

   ```
   import boto3
           
   personalize = boto3.client('personalize')
           
   create_solution_version_response = personalize.create_solution_version(solutionArn = 'solution arn', 
                                                                  trainingMode='FULL')
   
   new_solution_version_arn = create_solution_version_response['solutionVersionArn']
   print('solution_version_arn:', new_solution_version_arn)
   ```   When Amazon Personalize is finished creating your solution version, create your campaign with the following parameters:   Provide a new `campaign name` and the `solution version arn` generated in step 2\.    Modify the `explorationWeight` item exploration configuration hyperparameter to configure how much to explore\. Items with less interactions data or relevance are recommended more frequently the closer the value is to 1\.0\. The default value is 0\.3\.   Modify the `explorationItemAgeCutOff` item exploration configuration hyperparameter parameter to provide the maximum duration, in days relative to the latest interaction, for which items should be explored\. The larger the value, the more items are considered during exploration\.   Use the following Python snippet to create a new campaign with an emphasis on exploration with exploration cut\-off at 30 days\. Creating a campaign usually takes a few minutes but can take over an hour\. 

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
   ```  With User\-Personalization, Amazon Personalize automatically updates your solution version every two hours to include new data\. Your campaign automatically uses the updated solution version\. For more information see [Automatic updates](#automatic-updates)\.  To manually update the campaign, you first create and train a new solution version using the console or the [CreateSolutionVersion](API_CreateSolutionVersion.md) operation, with `trainingMode` set to `update`\. You then manually update the campaign on the **Campaign** page of the console or by using the [UpdateCampaign](API_UpdateCampaign.md) operation\.   Amazon Personalize doesn't automatically update solution versions you created before November 17, 2020\.        Getting recommendations and recording impressions \(Python SDK\)  When your campaign is created, you can use it to get recommendations for a user and record impressions\. For information on getting batch recommendations using the AWS SDK see [Creating a batch inference job \(AWS CLI and AWS SDKs\)](recommendations-batch.md#batch-sdk-cli)\.  To get recommendations and record impressions  Call the `get_recommendations` method\. Change the `campaign arn` to the ARN of your new campaign and `user id` to the userId of the user\. 

   ```
   import boto3
               
   rec_response = personalize_runtime.get_recommendations(campaignArn = 'campaign arn', userId = 'user id')
   print(rec_response['recommendationId'])
   ```   Create a new event tracker for sending PutEvents requests\. Replace `event tracker name` with the name of your event tracker and `dataset group arn` with the ARN of your dataset group\. 

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
   ```    Use the `recommendationId` from step 1 and the `event tracking id` from step 2 to create a new `PutEvents` request\. This request logs the new impression data from the user’s session\. Change the `user id` to the ID of the user\.  

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
   ```     Sample Jupyter notebookSample notebook  For a sample Jupyter notebook that shows how to use the User\-Personalization recipe, see [User Personalization with Exploration](https://github.com/aws-samples/amazon-personalize-samples/blob/master/next_steps/core_use_cases/user_personalization/user-personalization-with-exploration.ipynb)\.  ](native-recipe-new-item-USER_PERSONALIZATION.md) recipe with an additional optimization objective\.

```
{
    "solutionVersionArn": "arn:aws:personalize:us-west-2:acct-id:solution/MovieSolution/<version-id>",
    "metrics": {
        "coverage": 0.27,
        "mean_reciprocal_rank_at_25": 0.0379,
        "normalized_discounted_cumulative_gain_at_5": 0.0405,
        "normalized_discounted_cumulative_gain_at_10": 0.0513,
        "normalized_discounted_cumulative_gain_at_25": 0.0828,
        "precision_at_5": 0.0136,
        "precision_at_10": 0.0102,
        "precision_at_25": 0.0091,
        "average_rewards_at_k": 121.214
        
    }
}
```

The above metrics are described below using the following terms:
+ *Relevant recommendation* refers to a recommendation that matches a value in the testing data for the particular user\.
+ *Rank* refers to the position of a recommended item in the list of recommendations\. Position 1 \(the top of the list\) is presumed to be the most relevant to the user\.
+ *Query* refers to the internal equivalent of a [GetRecommendations](API_RS_GetRecommendations.md) call\.

For each metric, higher numbers are better\.

**coverage**  
 An evaluation metric that tells you the proportion of unique items that Amazon Personalize might recommend using your model out of the total number of unique items in Interactions and Items datasets\. To make sure Amazon Personalize recommends more of your items, use a model with a higher coverage score\. Recipes that feature item exploration, such as User\-Personalization, have higher coverage than those that don’t, such as popularity\-count\. 

**mean reciprocal rank at 25**  
An evaluation metric that assesses the relevance of a model’s highest ranked recommendation\. Amazon Personalize calculates this metric using the average accuracy of the model when ranking the most relevant recommendation out of the top 25 recommendations over all requests for recommendations\.   
This metric is useful if you're interested in the single highest ranked recommendation\.

**normalized discounted cumulative gain \(NCDG\) at K \(5/10/25\)**  
An evaluation metric that tells you about the relevance of your model’s highly ranked recommendations, where K is a sample size of 5, 10, or 25 recommendations\. Amazon Personalize calculates this by assigning weight to recommendations based on their position in a ranked list, where each recommendation is discounted \(given a lower weight\) by a factor dependent on its position\. The normalized discounted cumulative gain at K assumes that recommendations that are lower on a list are less relevant than recommendations higher on the list\.  
Amazon Personalize uses a weighting factor of `1/log(1 + position)`, where the top of the list is position `1`\.  
This metric rewards relevant items that appear near the top of the list, because the top of a list usually draws more attention\.

**precision at K**  
An evaluation metric that tells you how relevant your model’s recommendations are based on a sample size of K \(5, 10, or 25\) recommendations\. Amazon Personalize calculates this metric based on the number of relevant recommendations out of the top K recommendations, divided by K, where K is 5, 10, or 25\.  
This metric rewards precise recommendation of the relevant items\.

**average\_rewards\_at\_k**  
 When you create a solution version \(train a model\) for a solution with an optimization objective, Amazon Personalize generates an `average_rewards_at_k` metric\. The score for `average_rewards_at_k` tells you how well the solution version performs in achieving your objective\. Amazon Personalize calculates this metric by dividing the total rewards generated by interactions \(for example, total revenue from clicks\) by the total possible rewards from recommendations\. The higher the score, the more gains on average per user you can expect from recommendations\. For more information see [Optimizing a solution for an additional objective](optimizing-solution-for-objective.md)\. 

## Example<a name="working-with-training-metrics-example"></a>

The following is a simple example where, to generate metrics, a solution version produces a list of recommendations for a specific user\. The second and fifth recommendations match records in the testing data for this user\. These are the relevant recommendations\. If `K` is set at `5`, the following metrics are generated for the user\.

**reciprocal\_rank**  
Calculation: 1/2  
Result: 0\.5000

**normalized\_discounted\_cumulative\_gain\_at\_5**  
Calculation: \(1/log\(1 \+ 2\) \+ 1/log\(1 \+ 5\)\) / \(1/log\(1 \+ 1\) \+ 1/log\(1 \+ 2\)\)  
Result: 0\.6241

**precision\_at\_5**  
Calculation: 2/5  
Result: 0\.4000

Now that you have evaluated your solution version, create a campaign by deploying the optimum solution version\. For more information, see [Creating a campaign](campaigns.md)\.