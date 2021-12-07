# Managing recommenders<a name="managing-recommenders"></a>

 Amazon Personalize automatically retrains the models backing your recommenders every 7 days\. This is a full retraining that creates entirely new models based on the entirety of the data in your datasets\. For recommenders that create personalized recommendations for users, Amazon Personalize updates the existing models every two hours to include new items in recommendations with exploration\. For instructions on creating a new recommender, see [Creating recommenders](creating-recommenders.md)\. 

 Depending on the recommender's use case, you can update an active recommender's configuration with the Amazon Personalize console or with the [ UpdateRecommender ](API_UpdateRecommender.md) operation\. For example, for Top picks for you and Recommended for you, you can update exploration configuration with the following fields: 
+ Emphasis on exploring less relevant items: Configure how much to explore, where recommendations include items with less interactions data or relevance more frequently the more exploration you specify\. The closer the value is to 1, the more exploration\. At zero, no exploration occurs and recommendations are based on current data \(relevance\)\.
+ Exploration item age cut off: Enter the maximum item age, in days since the latest interaction, to define the scope of item exploration\. To increase the number of items Amazon Personalize considers during exploration, enter a greater value\. 

   For example, if you enter 10, only items with interactions data from the 10 days since the latest interaction in the dataset are considered during exploration\. 
**Note**  
Recommendations might include items without interactions data from outside this time frame\. This is because these items are relevant to the user's interests, and exploration wasn't required to identify them\.

 To update a recommender with the console, do the following\. 

**To update a recommender's configuration \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1.  On the **Dataset groups** page, choose your Domain dataset group\. 

1. From the navigation pane, choose **Recommenders**\.

1. On the **Recommenders** page, choose the recommender you want to update\.

1. In **Recommender configuration** choose **Edit**\.

1. Make any changes to the recommender's configuration and choose **Update**\.