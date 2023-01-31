# Trending\-Now recipe<a name="native-recipe-trending-now"></a>

 The Trending\-Now recipe \(aws\-trending\-now\) generates recommendations for items that are rapidly becoming more popular with your users\. You might use the Trending\-Now recipe if items gaining in popularity are more relevant to your customers\. For example, your customers might highly value what other users are interacting with\. Common uses include recommending viral social media content, breaking news articles, or recent sports videos\. 

Trending\-Now identifies the top trending items by calculating the increase in interactions that each item has over configurable intervals of time\. The items with highest rate of increase are considered trending items\. The time is based on timestamp data in your Interactions dataset\. You can specify the time interval by providing a `Trend discovery frequency` when you create your solution\. 

 For example, if you specify `30 min` for `Trend discovery frequency`, for every 30 minutes of data, Amazon Personalize identifies the items with the greatest rate of increase in interactions since the last evaluation\. Available frequencies include 30 minutes, 1 hour, 3 hour, and 1 day\. Choose a frequency that aligns with the distribution of your interactions data\. Missing data over the interval you choose can reduce recommendation accuracy\. 

 With Trending\-Now, you call the [GetRecommendations](API_RS_GetRecommendations.md) operation or get recommendations on the **Test campaign** page of the Amazon Personalize console\. Amazon Personalize returns the top trending items\. You pass a `userId` in your request only if you apply a filter that requires it\. With the GetRecommendations API, you can configure the number of trending items returned with the `numResults` parameter\. You can't get batch recommendations with the Trending\-Now recipe\. 

 To use Trending\-Now, you must create an Interactions dataset with at least 1000 unique historical and event interactions combined \(after filtering by eventType and eventValueThreshold, if provided\)\. 

## Properties and hyperparameters<a name="trending-now-hyperparameters"></a>

The Trending\-Now recipe has the following properties:
+  **Name** – `aws-trending-now`
+  **Recipe Amazon Resource Name \(ARN\)** – `arn:aws:personalize:::recipe/aws-trending-now`
+  **Algorithm ARN** – `arn:aws:personalize:::algorithm/aws-trending-now-custom`

For more information, see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\.

The following table describes the hyperparameters for the Trending\-Now recipe\. A *hyperparameter* is an algorithm parameter that you can adjust to improve model performance\. Algorithm hyperparameters control how the model performs\. The process of choosing the best value for a hyperparameter is called hyperparameter optimization \(HPO\)\. For more information, see [Hyperparameters and HPO](customizing-solution-config-hpo.md)\. 

The table also provides the following information for each hyperparameter:
+ **Range**: \[lower bound, upper bound\]
+ **Value type**: Integer, Continuous \(float\), Categorical \(Boolean, list, string\)
+ **HPO tunable**: Can the parameter participate in HPO?


| Name | Description | 
| --- | --- | 
| Feature transformation hyperparameters | 
| Trend discovery frequency |  Specify how often Amazon Personalize evaluates your interactions data and identifies trending items\. For example, if you specify `30 min` for `Trend discovery frequency`, every 30 minutes Amazon Personalize identifies items with the greatest rate of increase in interactions over 30\-minute intervals\.  Available frequencies include 30 minutes, 1 hour, 3 hours, and 1 day\. Choose a frequency that aligns with the distribution of your interactions data\. Missing data over the interval you choose can reduce recommendation accuracy\.  Default value: 3h Possible values: 30 min, 1h, 3h, and 1 day\. Value type: String HPO tunable: No  | 