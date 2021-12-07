# Amazon Personalize workflows<a name="personalize-workflow"></a>

 With Amazon Personalize, there are two workflow options: 
+ You can create a Domain dataset group with resources optimized for different use cases\. If you start with a Domain dataset group, you can still add custom resources such as solutions and solution versions trained for custom use cases\.
+  Or you can create a Custom dataset group with custom resources\. 

## Workflow for Domain dataset groups<a name="domain-workflow"></a>

To create domain\-based resources, you create a Domain dataset group for your business domain, import your data, create recommenders for each of your uses cases and get recommendations\. Repeat the data import process to maintain the relevance of your recommendations as your catalogue grows\. You can complete the domain workflow with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or the AWS SDKs\.

1. **Create a Domain dataset group**

    When you create a Domain dataset group, choose your business domain from the following types\. 
   + VIDEO\_ON\_DEMAND
   + ECOMMERCE

1. **Import data**

   You import item, user, and interaction records into *datasets* \(Amazon Personalize containers for data\)\. You can choose to import records in bulk, or incrementally, or both\. With incremental imports, you can add one or more historical records or import data from real\-time user activity\. 

   The data that you import depends on your domain\. Each domain has a default schema that you can map your data to\. For information about the types of data that you can import, see [Datasets and schemas](how-it-works-dataset-schema.md) and the sections on each domain \([VIDEO\_ON\_DEMAND datasets and schemas](VIDEO-ON-DEMAND-datasets-and-schemas.md) and [ECOMMERCE datasets and schemas](ECOMMERCE-datasets-and-schemas.md)\)\. 

1. **Create recommenders**

   After you've imported your data, Amazon Personalize uses it to train one or more models\. For Domain dataset groups, you start training by creating a *recommender* for each of your use cases\. Amazon Personalize trains the models backing each recommender with the best configurations for the selected use case\. For more information see [Creating recommenders](creating-recommenders.md)\. 

1. **Get recommendations**

   After Amazon Personalize creates your recommenders, you can use them to get recommendations with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or the AWS SDKs\. For more information see [Getting recommendations from a recommender](domain-dsg-recommendations.md)\.

1. **Refresh your data and repeat**

    Keep your item and user data current and record new interaction data in real\-time\. Amazon Personalize manages the life cycle of your recommenders and retrains models as your datasets change\. This allows your model to learn from your user’s most recent activity and sustains and improves the relevance of recommendations\. For more information see [Maintaining recommendation relevance \(Domain dataset group\)](maintaining-relevance-domain.md)\.

## Workflow for Custom dataset groups<a name="custom-workflow"></a>

With the custom workflow you determine your use case, import your data, train and deploy a model, and then get recommendations\. Repeat the data import and training processes to sustain and improve the relevance of your recommendations as your catalogue grows\. You can complete the Amazon Personalize workflow with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or the AWS SDKs\.

1. **Determine your use case**

    Choose your use case from the following and note its corresponding recipe type\. \(Recipes are Amazon Personalize algorithms prepared for different use cases\.\)
   + Recommending items for users \(USER\_PERSONALIZATION recipes\)
   + Ranking items for a given user \(PERSONALIZED\_RANKING recipes\)
   + Recommending similar items \(RELATED\_ITEMS recipes\)

   For more information see [Determining your use case](determining-use-case.md)\.

1. **Import data**

   You import item, user, and interaction records into *datasets* \(Amazon Personalize containers for data\)\. You can choose to import records in bulk, or incrementally, or both\. With incremental imports, you can add one or more historical records or import data from real\-time user activity\. 

   The data that you import depends on your use case\. For information about the types of data that you can import, see [Datasets and schemas](how-it-works-dataset-schema.md) and the sections on each dataset type \([Interactions data](interactions-datasets.md), [Item data](items-datasets.md), [User data](users-datasets.md)\)\. 

   For more information about importing data see [Preparing and importing data](data-prep.md)\.

1. **Train a model**

   After you've imported your data, Amazon Personalize uses it to train a model\. In Amazon Personalize, you start training by creating a *solution*, where you specify your use case by choosing an Amazon Personalize recipe\. Then you create a *solution version*, which is the trained model that Amazon Personalize uses to generate recommendations\. For more information see [Creating a solution](training-deploying-solutions.md)\. 

1. **Deploy a model \(for real\-time recommendations\)**

   After Amazon Personalize finishes creating your solution version \(trained model\), you deploy it in a campaign\. A campaign creates and manages a recommendation API that you use in your application to request real\-time recommendations from your custom model\. For more information about deploying a model see [Creating a campaign](campaigns.md)\. For batch recommendations, you don't need to create a campaign\.

1. **Get recommendations and user segments**

   Get recommendations in real time or as part of a batch workflow\. Get real\-time recommendations when you want to update recommendations as customers use your application\. Get batch recommendations and user segments when you don't require real\-time updates\. For more information, see [Getting recommendations \(Custom dataset group\)](getting-recommendations.md)\. 

1. **Refresh your data and repeat**

    Keep your item and user data current, record new interaction data in real\-time, and re\-train your model on a regular basis\. This allows your model to learn from your user’s most recent activity and sustains and improves the relevance of recommendations\. For more information see [Maintaining recommendation relevance](maintaining-relevance.md)\.