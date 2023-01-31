# Step 4: Evaluating a solution version with metrics<a name="working-with-training-metrics"></a>

 You can evaluate the performance of your solution version through offline and online metrics\. *Online metrics* are the empirical results you observe in your users' interactions with real\-time recommendations\. For example, you might record your users' click\-through rate as they browse your catalog\. You are responsible for generating and recording any online metrics\. 

 *Offline metrics* are the metrics Amazon Personalize generates when you train a solution version\. You can use offline metrics to evaluate the performance of the model before you create a campaign and provide recommendations\. Offline metrics allow you to view the effects of modifying a solution's hyperparameters or compare results from models trained with the same data\. For the rest of this section, the term metrics refers to *offline metrics*\.

 To get performance metrics, Amazon Personalize splits the input interactions data into a training set and a testing set\. The split depends on the type of recipe you choose:
+  For USER\_SEGMENTATION recipes, the training set consists of 80% of each user's interactions data and the testing set consists of 20% of each user's interactions data\. 
+  For all other recipe types, the training set consists of 90% of your users and their interactions data\. The testing set consists of the remaining 10% of users and their interactions data\. 

 Amazon Personalize then creates the solution version using the training set\. After training completes, Amazon Personalize gives the new solution version the oldest 90% of each user’s data from the testing set as input\. Amazon Personalize then calculates metrics by comparing the recommendations the solution version generates to the actual interactions in the newest 10% of each user’s data from the testing set\. 

To generate a baseline for comparison purposes, we recommend using the [Popularity\-Count](native-recipe-popularity.md) recipe, which recommends the top K most popular items\.

**Topics**
+ [Retrieving solution version metrics](#working-with-training-metrics-metrics)
+ [Metric definitions](#metric-definitions)
+ [Example](#working-with-training-metrics-example)
+ [Additional resources](#additional-metrics-resources)

## Retrieving solution version metrics<a name="working-with-training-metrics-metrics"></a>

After you create a solution version, you can use metrics to evaluate its performance\. You can retrieve metrics for a solution version with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), and AWS SDKs\. 

**Topics**
+ [Retrieving solution version metrics \(console\)](#retrieving-solution-version-metrics-console)
+ [Retrieving solution version metrics \(AWS CLI\)](#retrieve-metrics-cli)
+ [Retrieving solution version metrics \(AWS SDKs\)](#retrieve-metrics-sdks)

### Retrieving solution version metrics \(console\)<a name="retrieving-solution-version-metrics-console"></a>

To view recommender metrics in the console, you navigate to the details page for your solution version\.

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. On the **Dataset groups** page, choose your Custom dataset group\. 

1. From the navigation pane, choose **Custom resources** and then choose **Solutions and recipes**\.

1. Choose your solution\.

1. In **Solution versions**, choose your solution version to view its details page\. The metrics are listed on the **Solution version metrics** tab in the bottom pane\. For definitions of metrics, see [Metric definitions](#metric-definitions)\.

   Now that you have evaluated your solution version, you can create a campaign by deploying the solution version with the best metrics for your use case\. For more information about deploying a solution, see [Creating a campaign](campaigns.md)\.

### Retrieving solution version metrics \(AWS CLI\)<a name="retrieve-metrics-cli"></a>

You retrieve the metrics for a specific solution version by calling the [GetSolutionMetrics](API_GetSolutionMetrics.md) operation\. The following code shows how to retrieve metrics with the AWS CLI\. 

```
personalize get-solution-metrics --solution-version-arn solution version ARN
```

The following is an example the output from a solution version created using the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) recipe with an additional optimization objective\.

```
{
    "solutionVersionArn": "arn:aws:personalize:us-west-2:acct-id:solution/SolutionName/<version-id>",
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

For explanations of each metric, see [Metric definitions](#metric-definitions)\. Now that you have evaluated your solution version, you can create a campaign by deploying the solution version with the best metrics for your use case\. For more information about deploying a solution, see [Creating a campaign](campaigns.md)\.

### Retrieving solution version metrics \(AWS SDKs\)<a name="retrieve-metrics-sdks"></a>

You retrieve the metrics for a specific solution version by calling the [GetSolutionMetrics](API_GetSolutionMetrics.md) operation\. Use the following code to retrieve metrics\. 

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3

personalize = boto3.client('personalize')

response = personalize.get_solution_metrics(
    solutionVersionArn = 'solution version arn')

print(response['metrics'])
```

------
#### [ SDK for Java 2\.x ]

```
public static void getSolutionVersionMetrics(PersonalizeClient personalizeClient, String solutionVersionArn) {

    try {
        GetSolutionMetricsRequest request = GetSolutionMetricsRequest.builder()
                .solutionVersionArn(solutionVersionArn)
                .build();
        Map<String, Double> metrics = personalizeClient.getSolutionMetrics(request).metrics();
        metrics.forEach((key, value) -> System.out.println(key + " " + value));
    } catch (PersonalizeException e ) {
        System.err.println(e.awsErrorDetails().errorMessage());
        System.exit(1);
    }
}
```

------

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

For explanations of each metric, see [Metric definitions](#metric-definitions)\. Now that you have evaluated your solution version, you can create a campaign by deploying the solution version with the best metrics for your use case\. For more information about deploying a solution, see [Creating a campaign](campaigns.md)\.

## Metric definitions<a name="metric-definitions"></a>

The metrics Amazon Personalize generates for solution versions are described below using the following terms:
+ *Relevant recommendation* is a recommendation for an item that the user actually interacted with\. These items are from the newest 10% of each user’s interactions data from the testing set\. 
+ *Rank* refers to the position of a recommended item in the list of recommendations\. Position 1 \(the top of the list\) is presumed to be the most relevant to the user\.

For each metric, higher numbers \(closer to 1\) are better\. To dive deeper, see the resources listed in [Additional resources](#additional-metrics-resources)\. 

**coverage**  
The value for *coverage* tells you the proportion of unique items that Amazon Personalize might recommend out of the total number of unique items in Interactions and Items datasets\. A higher coverage score means Amazon Personalize recommends more of your items, rather than the same few items repeatedly for different users\. Recipes that feature item exploration, such as User\-Personalization, have higher coverage than those that don’t, such as popularity\-count\. 

**mean reciprocal rank at 25**  
This metric tells you about a model's ability to generate a relevant recommendation at the top ranked position\. You might choose a model with a high *mean reciprocal rank at 25* if you are generating relevant search results for a user, and don't expect the user to choose an item lower on the list\. For example, users frequently choose the first cooking recipe in search results\.   
 Amazon Personalize calculates this metric using the average reciprocal rank score for requests for recommendations\. Each reciprocal rank score is calculated as follows: `1 / the rank of the highest item interacted with by the user`, where the total possible rankings is 25\. Other lower ranked items the user interacts with are ignored\. If the user chose the first item, the score is 1\. If they don't choose any items, the score is 0\.   
 For example, you might show three different users 25 recommendations each:   
+ If User 1 clicks the item at rank *4* and the item at rank *10*, their reciprocal rank score is *1/4*\.
+ If User 2 clicks an item at rank *2*, an item at rank *4*, and an item at rank *12*, their reciprocal rank score is 1/2\.
+ If User 3 clicks on a single item at rank *6*, their reciprocal rank score is 1/6\.
 The mean reciprocal rank over all requests for recommendations \(in this case 3\) is calculated as `(1/4 + 1/2 + 1/6) / 3 = .3056`\.   


**normalized discounted cumulative gain \(NCDG\) at K \(5/10/25\)**  
This metric tells you about how well your model ranks recommendations, where K is a sample size of 5, 10, or 25 recommendations\. This metric is useful if you are most interested in the ranking of recommendations beyond just the highest ranked item \(for this, see `mean reciprocal rank at 25`\)\. For example, the score for `NCDG at 10` would be useful if you have an application that shows up to 10 movies in a carousel at a time\.   
Amazon Personalize calculates the NCDG by assigning weight to recommendations based on their ranking position for each user in the testing set\. Each recommendation is discounted \(given a lower weight\) by a factor dependent on its position\. The final metric is the average of all users in the testing set\. The normalized discounted cumulative gain at K assumes that recommendations that are lower on a list are less relevant than recommendations higher on the list\.  
Amazon Personalize uses a weighting factor of `1/log(1 + position)`, where the top of the list is position `1`\.

**precision at K**  
This metric tells you how relevant your model’s recommendations are based on a sample size of K \(5, 10, or 25\) recommendations\.   
 Amazon Personalize calculates this metric based on the number of relevant recommendations out of the top K recommendations for each user in the testing set, divided by K, where K is 5, 10, or 25\. The final metric is the average across all users in the testing set\.  
For example, if you recommend 10 items to a user, and the user interacts with 3 of them, the precision at K is 3 correctly predicted items divided by the total 10 recommended items: `3 / 10 = .30`\.   
This metric rewards precise recommendation of relevant items\. The closer the score is to one, the more precise the model\. 

**average\_rewards\_at\_k**  
When you create a solution version \(train a model\) for a solution with an optimization objective, Amazon Personalize generates an `average_rewards_at_k` metric\. The score for `average_rewards_at_k` tells you how well the solution version performs in achieving your objective\. To calculate this metric, Amazon Personalize calculates the rewards for each user as follows:  
`rewards_per_user = total rewards from the user's interactions with their top 25 reward generating recommendations / total rewards from the user's interactions with recommendations`  
The final `average_rewards_at_k` is the average of all `rewards_per_user` normalized to be a decimal value less than or equal to 1 and greater than 0\. The closer the value is to 1, the more gains on average per user you can expect from recommendations\.  
For example, if your objective is to maximize revenue from clicks, Amazon Personalize calculates each user score by dividing total revenue generated by the items the user clicked from their top 25 most expensive recommendations by the revenue from all of the recommended items the user clicked\. Amazon Personalize then returns a normalized average of all user scores\. The closer the `average_rewards_at_k` is to 1, the more revenue on average you can expect to gain per user from recommendations\.  
 For more information see [Optimizing a solution for an additional objective](optimizing-solution-for-objective.md)\. 

**trend prediction accuracy**  
If you trained the solution version with the [Trending\-Now](native-recipe-trending-now.md) recipe, the rate of increase in popularity of items recommended by the model\. The higher the trend prediction accuracy \(the closer to 1\), the better the model is at correctly identifying trending items\.   
 To calculate popularity acceleration, Amazon Personalize divides the rate of increase in popularity across all recommended items by the total popularity increase of the top 25 trending items\. These items come from the actual interactions in the testing set\.   
 Depending on your data distribution and what you choose for Trend discovery frequency, the value for trend prediction accuracy can be 0\.0\.  

**hit \(hit at K\)**  
If you trained the solution version with a USER\_SEGMENTATION recipe, the average number of users in the predicted top relevant K results that match the actual users\. Actual users are the users who actually interacted with the items in the test set\. K is the top 1% of the most relevant users\. The higher the value the more accurate the predictions\. 

**recall \(recall at K\)**  
If you trained the solution version with a USER\_SEGMENTATION recipe, the average percentage of predicted users in the predicted top relevant K results that match the actual users\. Actual users are the users who actually interacted with the items in the test set\. K is the top 1% of the most relevant users\. The higher the value, the more accurate the predictions\.

## Example<a name="working-with-training-metrics-example"></a>

The following is a simple example for a solution version that produces a list of recommendations for a specific user\. The second and fifth recommendations match records in the testing data for this user\. These are the relevant recommendations\. If `K` is set at `5`, the following metrics are generated for the user\.

**reciprocal\_rank**  
Calculation: 1/2  
Result: 0\.5000

**normalized\_discounted\_cumulative\_gain\_at\_5**  
Calculation: \(1/log\(1 \+ 2\) \+ 1/log\(1 \+ 5\)\) / \(1/log\(1 \+ 1\) \+ 1/log\(1 \+ 2\)\)  
Result: 0\.6241

**precision\_at\_5**  
Calculation: 2/5  
Result: 0\.4000

## Additional resources<a name="additional-metrics-resources"></a>

For information on evaluating a solution version with A/B testing, see [ Using A/B testing to measure the efficacy of recommendations generated by Amazon Personalize](http://aws.amazon.com/blogs/machine-learning/using-a-b-testing-to-measure-the-efficacy-of-recommendations-generated-by-amazon-personalize/)\. To dive deeper in different types of metrics for recommender systems, see the following external resources:
+ [MRR vs MAP vs NDCG: Rank\-Aware Evaluation Metrics And When To Use Them](https://medium.com/swlh/rank-aware-recsys-evaluation-metrics-5191bba16832/)
+  [Discounted Cumulative Gain](https://machinelearningmedium.com/2017/07/24/discounted-cumulative-gain/) 
+  [Recall and Precision at k for Recommender Systems](https://medium.com/@bond.kirill.alexandrovich/precision-and-recall-in-recommender-systems-and-some-metrics-stuff-ca2ad385c5f8) 
+  [Ranking Evaluation Metrics for Recommender Systems](https://towardsdatascience.com/ranking-evaluation-metrics-for-recommender-systems-263d0a66ef54) 