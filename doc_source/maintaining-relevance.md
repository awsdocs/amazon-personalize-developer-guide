# Maintaining recommendation relevance<a name="maintaining-relevance"></a>

 Maintain the relevance of recommendations to increase user engagement, click\-through rate, and conversion rate for your application as your catalogue grows\. To maintain and improve the relevance of Amazon Personalize recommendations for your users, keep your data and solution versions up to date\. This allows Amazon Personalize to learn from your user’s most recent behavior and include your newest items in recommendations\. 

 You can automate and schedule re\-training and data import tasks with **Maintaining Personalized Experiences with Machine Learning**, an AWS Solutions Implementation that automates the Amazon Personalize workflow, including data import, solution version training, and batch workflows\. For more information see [Maintaining Personalized Experiences with Machine Learning](https://aws.amazon.com/solutions/implementations/maintaining-personalized-experiences-with-ml/)\. 

**Topics**
+ [Keeping datasets current](#updating-data)
+ [Keeping solution versions up to date](#retraining-model)

## Keeping datasets current<a name="updating-data"></a>

 As your catalog grows, update your historical data with bulk or individual data import operations\. We recommend that you first import your records in bulk, and then add individual items and users as your catalog grows\. For more information about importing historical data see [Preparing and importing data](data-prep.md)\. 

 For real\-time recommendations with the User\-Personalization and Personalized\-Ranking recipes, keep your Interactions dataset up to date with your users' behavior by recording interaction *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)* with an event tracker and the [PutEvents](API_UBS_PutEvents.md) operation\. Amazon Personalize updates recommendations based on your user's most recent activity as they interact with your catalog\. For more information on recording real\-time events, see [Recording events](recording-events.md)\. 

If you have already created a solution version \(trained a model\), new records influence recommendations as follows:
+  For *individual interactions*, Amazon Personalize immediately uses real\-time interaction events between a user and existing items \(items you included in the data you used to train the latest model\) when generating recommendations for the same user\. For more information, see [How real\-time events influence recommendations](recording-events.md#recorded-events-influence-recommendations)\. 

   For *bulk interactions*, you must create a new solution version for bulk interactions data to influence recommendations\. 
+ For *individual and bulk item data*, if you trained the solution version with User\-Personalization and deployed it in a campaign, Amazon Personalize automatically updates the model every two hours\. After each update, the new items can be included in recommendations with exploration\. For information about exploration see [Automatic updates](native-recipe-new-item-USER_PERSONALIZATION.md#automatic-updates)\. 

   For any other recipe, you must retrain the model for the new items to be included in recommendations\. 
+ For *new users* without interactions data, recommendations are initially for only popular items\. If you have metadata about the user in a Users dataset and you choose a recipe that uses metadata, such as User\-Personalization or Personalized\-Ranking, these popular items will be more relevant for the user\. 

  To get more relevant recommendations for a new user, you can import bulk interactions for the user and create a new solution version\. Or you can record events for the user as they interact with your catalog\. Their recommendations will be more relevant as you record more events\. For more information, see [Recording events](recording-events.md)\. 

## Keeping solution versions up to date<a name="retraining-model"></a>

 Create a new solution version \(retrain the model\) to include new items in recommendations and update the model with your user’s most recent behavior\. Once you retrain the model, you must update the campaign to deploy it\. For more information see [Updating a campaign](update-campaigns.md)\. 

 Retraining frequency depends on your business requirements and the recipe that you use\. For most workloads, we recommend training a new model weekly with training mode set to `FULL`\. This creates a completely new solution version based on the entirety of the training data from the datasets in your dataset group\. If you add new items frequently and you don't use User\-Personalization, you may need to do a full retraining more often to include those new items in recommendations\. 

 With User\-Personalization, Amazon Personalize automatically updates your latest fully trained solution version \(trained with `trainingMode` set to `FULL`\) every two hours to include new items in recommendations with exploration\. The solution version must be deployed in a campaign for updates to occur\. Your campaign automatically uses the updated solution version\. This is not a full retraining; you should still train a new solution version weekly with `trainingMode` set to `FULL` so the model can learn from your users' behavior\. 

 If every two hours is not frequent enough, you can manually create a solution version with `trainingMode` set to `UPDATE` to include those new items in recommendations\. Just remember that Amazon Personalize automatically updates only your latest fully trained solution version, so the manually updated solution version won't be automatically updated in the future\. For more information, see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

For information on creating a new solution version, see [Step 3: Creating a solution version](creating-a-solution-version.md)\. 