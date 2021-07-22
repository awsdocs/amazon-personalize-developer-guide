# Maintaining recommendation relevance<a name="maintaining-relevance"></a>

 Maintain the relevance of recommendations to increase user engagement, click\-through rate, and conversion rate for your application as your catalogue grows\. To maintain and improve the relevance of Amazon Personalize recommendations for your users, keep your data and solution versions up to date\. This allows Amazon Personalize to learn from your user’s most recent behavior and include your newest items in recommendations\. 

**Topics**
+ [Keeping datasets current](#updating-data)
+ [Keeping solution versions up to date](#retraining-model)

## Keeping datasets current<a name="updating-data"></a>

 As your catalog grows, update your historical data with bulk or incremental data import operations\. We recommend that you first import your records in bulk, and then incrementally add items and users as your catalog grows\. For more information about importing historical data see [Preparing and importing data](data-prep.md)\. 

For real\-time recommendations, keep your Interactions dataset up to date with your users' behavior by recording interaction *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)* with an event tracker and the [PutEvents](API_UBS_PutEvents.md) operation\. Amazon Personalize updates recommendations based on your user's most recent activity as they interact with your application\. For more information on recording real\-time events, see [Recording events](recording-events.md)\. 

If you have already created a solution version \(trained a model\), new records influence recommendations as follows:
+  For *new events*, Amazon Personalize immediately uses historical and real\-time interaction events between a user and existing items \(items you included in the data you used to train the latest model\) when generating recommendations for the same user\. Historical events that you import using the Amazon Personalize console and events that you record in real\-time influence recommendations in the same way\. For more information, see [How real\-time events influence recommendations](recording-events.md#recorded-events-influence-recommendations)\. 
+ For *new items*, if you trained the solution version with the User\-Personalization recipe, Amazon Personalize automatically updates the model every two hours\. After each update, the new items can be included in recommendations with exploration\. For information about exploration see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

   For any other recipe, you must retrain the model for the new items to be included in recommendations\. 
+  For *new users*, recommendations will initially be only for popular items\. Starting with the first event, user recommendations will be more relevant as you record events\. For more information, see [Recording events](recording-events.md)\. 

## Keeping solution versions up to date<a name="retraining-model"></a>

 Create a new solution version \(retrain the model\) to include new items in recommendations and update the model with your user’s most recent behavior\. Once you retrain the model, you must update the campaign to deploy it\. For more information see [Updating a campaign](update-campaigns.md)\. 

 Retraining frequency depends on your business requirements and the recipe that you use\. For most workloads, we recommend training a new model weekly with training mode set to `FULL`\. This creates a completely new solution version based on the entirety of the training data from the datasets in your dataset group\. 

 If you are adding items daily and don't use the User\-Personalization recipe, create a new solution version daily with training mode set to `UPDATE` to include the new items in recommendations\. With the User\-Personalization recipe, Amazon Personalize automatically updates your latest solution version \(trained with `trainingMode` set to `FULL`\) every two hours to include new items in recommendations with exploration\. Your campaign automatically uses the updated solution version\. You should still regularly train a new solution version weekly so that the model can learn from your user's behavior\. For more information, see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

For information on creating a new solution version, see [Step 3: Creating a solution version](creating-a-solution-version.md)\. 