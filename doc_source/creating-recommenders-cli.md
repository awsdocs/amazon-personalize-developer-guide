# Creating recommenders \(AWS CLI\)<a name="creating-recommenders-cli"></a>

 Use the following AWS CLI code to create a recommender for a domain use case\. Run this code for each of your domain use cases\. For `recipeArn`, provide the Amazon Resource Name \(ARN\) for your use case\. The available use cases depend on your domain\. For a list of use cases and their ARNs see [Choosing recommender use cases](domain-use-cases.md)\. 

For `Top picks for your` or `Recommended for you` use cases, Amazon Personalize uses exploration when recommending items\. For more information, see [Configuring exploration](#domain-config-explore-cli)\.

```
aws personalize create-recommender \
--name recommender name \
--dataset-group-arn dataset group ARN \
--recipe-arn recipe ARN
```

## Configuring exploration<a name="domain-config-explore-cli"></a>

For `Top picks for your` or `Recommended for you` use cases, Amazon Personalize uses exploration when recommending items\. Exploration involves testing different item recommendations to learn how users respond to items with very little interaction data\. You can configure exploration with the following: 
+ Emphasis on exploring less relevant items \(for APIs, this is called explorationWeight in the [RecommenderConfig](API_RecommenderConfig.md)\): Configure how much to explore\. Specify a decimal value between 0 to 1\. The default is 0\.3\. The closer the value is to 1, the more exploration\. With more exploration, recommendations include more items with less interactions data or relevance\. At zero, no exploration occurs and recommendations are based on current data \(relevance\)\.
+ Exploration item age cutoff: Specify the maximum item age in days since the latest interaction\. This defines the scope of item exploration\. For example, if you enter 10, Amazon Personalize exploration includes only items with interactions data from the 10 days since the latest interaction in the dataset\.

  To increase the items Amazon Personalize considers during exploration, enter a greater value\. The minimum is 1 day and the default is 30 days\. Recommendations might include items without interactions data from outside this time frame\. This is because these items are relevant to the user and exploration didn't identify them\.

The following code shows how to configure exploration when you create a recommender for the `Top picks for you` use case\. The example uses the default values\.

```
aws personalize create-recommender \
  --name recommender name \
  --dataset-group-arn dataset group ARN \
  --recipe-arn arn:aws:personalize:::recipe/aws-vod-top-picks \
  --recommender-config "{\"itemExplorationConfig\":{\"explorationWeight\":\"0.3\",\"explorationItemAgeCutOff\":\"30\"}}"
```