# Step 4: Evaluating a solution version with metrics<a name="working-with-training-metrics"></a>

 You can evaluate the performance of your solution version through offline and online metrics\. *Online metrics* are the empirical results you observe in your users' interactions with real\-time recommendations\. For example, you might record your users' click\-through rate as they browse your catalog\. You are responsible for generating and recording any online metrics\. 

 *Offline metrics* are the metrics Amazon Personalize generates when you train a solution version\. You can use offline metrics to evaluate the performance of the model before you create a campaign and provide recommendations\. Offline metrics allow you to view the effects of modifying a solution's hyperparameters or compare results from solutions that use the same training data but use different recipes\. For the rest of this section, the term metrics refers to *offline metrics*\.

 To get performance metrics, Amazon Personalize splits the input interactions data into two sets: a training set and a testing set\. The training set consists of 90% of each user's interactions data\. The testing set consists of the remaining 10% of each user's interactions data\. Amazon Personalize then creates the solution version using the training set\. 

 When training completes, Amazon Personalize gives the solution version the oldest 90% of each user’s data from the testing set as input\. Amazon Personalize then calculates metrics by comparing the recommendations the solution version generates to the actual interactions in the newest 10% of each user’s data from the testing set\. 

To generate a baseline for comparison purposes, we recommend using the [Popularity\-Count](native-recipe-popularity.md) recipe, which recommends the top K most popular items\.

**Important**  
In order for Amazon Personalize to generate solution version metrics, you must have at least 10 datapoints in your input dataset group\.

## Retrieving Metrics<a name="working-with-training-metrics-metrics"></a>

You retrieve the metrics for a specific solution version by calling the [ GetSolutionMetrics ](API_GetSolutionMetrics.md) operation\.

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

The following is an example the output from a solution version created using the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) recipe with an additional optimization objective\.

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
        "average_rewards_at_k": 0.653
    }
}
```

The above metrics are described below using the following terms:
+ *Relevant recommendation* refers to a recommendation that matches a value in the testing data for the particular user\.
+ *Rank* refers to the position of a recommended item in the list of recommendations\. Position 1 \(the top of the list\) is presumed to be the most relevant to the user\.
+ *Query* refers to the internal equivalent of a [ GetRecommendations ](API_RS_GetRecommendations.md) call\.

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
When you create a solution version \(train a model\) for a solution with an optimization objective, Amazon Personalize generates an `average_rewards_at_k` metric\. The score for `average_rewards_at_k` tells you how well the solution version performs in achieving your objective\. To calculate this metric, Amazon Personalize calculates the rewards for each user as follows:  
`rewards_per_user = total rewards from the user's interactions with their top 25 reward generating recommendations / total rewards from the user's interactions with recommendations`  
The final `average_rewards_at_k` is the average of all `rewards_per_user` normalized to be a decimal value less than or equal to 1 and greater than 0\. The closer the value is to 1, the more gains on average per user you can expect from recommendations\.  
For example, if your objective is to maximize revenue from clicks, Amazon Personalize calculates each user score by dividing total revenue generated by the items the user clicked from their top 25 most expensive recommendations by the revenue from all of the recommended items the user clicked\. Amazon Personalize then returns a normalized average of all user scores\. The closer the `average_rewards_at_k` is to 1, the more revenue on average you can expect to gain per user from recommendations\.  
 For more information see [Optimizing a solution for an additional objective](optimizing-solution-for-objective.md)\. 

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