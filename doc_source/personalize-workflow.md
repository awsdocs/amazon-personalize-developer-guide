# Amazon Personalize workflow<a name="personalize-workflow"></a>

 With Amazon Personalize, you determine your use case, import your data, train and deploy a model, and then request recommendations from Amazon Personalize\. Repeat the data import and training processes to sustain and improve the relevance of your recommendations as your catalogue grows\. You can complete the Amazon Personalize workflow with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or the AWS SDKs\. 

1. **Determine your use case**

    Before you use Amazon Personalize, determine your use case to identify what recipe to use to train your model, and what data to import into Amazon Personalize\. *Recipes* are Amazon Personalize algorithms that are prepared for different use cases\. To get started providing personalized experiences for your users, choose your use case from the following and note its corresponding recipe type\. 
   + Recommending items for users \(USER\_PERSONALIZATION recipes\)
   + Ranking items for a given user \(PERSONALIZED\_RANKING recipes\)
   + Recommending similar items \(RELATED\_ITEMS recipes\)

   For more information see [Determining your use case](determining-use-case.md)\.

1. **Import data**

   You import item, user, and interaction records into Amazon Personalize datasets\. You can choose to import records in bulk, or incrementally, or both\. With incremental imports, you can add one or more historical records or import data from real\-time user activity\. 

   The data that you import depends on your use case\. For information about the types of data that you can import, see [Datasets and schemas](how-it-works-dataset-schema.md) and the sections on each dataset type \([Interactions dataset](interactions-datasets.md), [Items dataset](items-datasets.md), [Users dataset](users-datasets.md)\)\. 

   For more information about importing data see [Preparing and importing data](data-prep.md)\.

1. **Train a model**

   After you've imported your data, Amazon Personalize uses it to train a model\. In Amazon Personalize, you start training by creating a *solution*, where you specify your use case by choosing an Amazon Personalize recipe\. Then you create a *solution version*, which is the trained model that Amazon Personalize uses to generate recommendations\. For more information see [Creating a solution](training-deploying-solutions.md)\. 

1. **Deploy a model \(for real\-time recommendations\)**

   After Amazon Personalize finishes creating your solution version \(trained model\), you deploy it in a campaign\. A campaign creates and manages a recommendation API that you use in your application to request real\-time recommendations from your custom model\. For more information about deploying a model see [Creating a campaign](campaigns.md)\. For batch recommendations, you don't need to create a campaign\.

1. **Get recommendations**

   Get recommendations in real\-time or as part of a batch workflow with purely historical data\. Get real\-time recommendations when you want to update recommendations as customers use your application\. Get batch recommendations when you do not require real\-time updates\. For more information, see [Getting recommendations](getting-recommendations.md)\. 

1. **Refresh your data and repeat**

    Keep your item and user data current, record new interaction data in real\-time, and re\-train your model on a regular basis\. This allows your model to learn from your user’s most recent activity and sustains and improves the relevance of recommendations\. For more information see [Maintaining recommendation relevance](maintaining-relevance.md)\.