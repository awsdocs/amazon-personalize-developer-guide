# What is Amazon Personalize?<a name="what-is-personalize"></a>

Amazon Personalize is a fully managed machine learning service that makes it easy for developers deliver personalized experiences to their users\. It reflects the knowledge and experience that Amazon has in building personalization systems\.

You can use Amazon Personalize in a variety of scenarios, such as generating recommendations for users based on their preferences and behavior, personalized re\-ranking of results, personalizing content for emails, or creating targeted marketing campaigns based on user segments\. Amazon Personalize does not require machine learning experience\. You can get started quickly with use case optimized resources for your business domain, or you can create your own configurable custom resources\. 

 In Amazon Personalize, you start by creating a dataset group, which is a container for Amazon Personalize components\. Your dataset group can be one of the following: 
+  A *Domain dataset group*, where you choose your business domain, import your data, and create recommenders\. You use recommenders in your application to get recommendations\.  

  Use a Domain dataset group if you have a video on demand or e\-commerce application and want Amazon Personalize to find the best configurations for your use cases\. If you start with a Domain dataset group, you can also add custom resources such as solutions with solution versions trained with recipes for custom use cases\. 
+ A *Custom dataset group*, where you choose a recipe, train a solution version \(model\), and deploy the solution version with a campaign\. 

  Use a Custom dataset group if you don't have a video on demand or e\-commerce application or want to configure and manage only custom resources, or want to get recommendations in a batch workflow\. If you start with a Custom dataset group, you can't associate it with a domain later\. Instead, create a new Domain dataset group\. 

You can create and manage Domain dataset groups and Custom dataset groups with the AWS console, the AWS Command Line Interface \(AWS CLI\), or programmatically with the AWS SDKs\. 

With Domain dataset groups and Custom dataset groups, Amazon Personalize can capture real\-time events from your users to deliver real\-time personalization\. Amazon Personalize can blend real\-time user activity data with existing user profile and item information \(historical data\) to recommend the most relevant items for the user\. You can also use Amazon Personalize to collect interactions data for new properties, such as a new website, and after enough data has been collected, Amazon Personalize can start to make recommendations\.

## Using Domain dataset groups<a name="what-are-domain-dataset-groups"></a>

 With Domain dataset groups, Amazon Personalize provides default schemas with data requirements based on your domain, fully manages the life cycle of training and updating recommenders, and handles the provisioning of a recommendation API\. Domain dataset groups make it easy to tell what data you need to import to get recommendations tailored for your use case, and you don't have to worry about finding the best training configuration for your business domain\. 

 If you start with a Domain dataset group, you can still add custom resources such as solutions and solution versions trained with recipes for custom use cases\. 

 For Domain dataset groups, you only need to do the following: 

1. Create a dataset group and choose your business domain\.

1.  Create a schema that includes your domain's required fields\. You must create a schema before you import data\. 

1.  Format any historical input data to match your schema \(list of required fields and types\) and upload the data into an Amazon S3 bucket\. 

1.  Import your data into an Amazon Personalize dataset and record real\-time event data from user interactions with items\. 

1. When you have collected enough data, create recommenders for each of your use cases\.

    The minimum data requirements to train a model are as follows: 
   +  1000 records of combined interaction data \(after filtering by `eventType` and `eventValueThreshold`, if provided\)\.
   +  25 unique users with at least 2 interactions each\. 

1. Use your recommenders in your application to get recommendations with the GetRecommendations operation\.

1. As your catalog grows, maintain recommendation relevance by keeping your data current with bulk or incremental data import operations\. Amazon Personalize manages the life cycle of your recommenders and updates them as your datasets change\. 

## Using Custom dataset groups<a name="what-are-custom-dataset-groups"></a>

With Custom dataset groups, you train a solution and configure them for your use cases\. For example, user personalization, items related to an item, re\-ranking of items, or user segmentation\. You choose a recipe and input data based on your use case\. Amazon Personalize performs featurization of your data, and applies a choice of learning algorithms, along with default hyperparameters, and hyperparameter optimization job configuration\. 

With recipes, you can to create custom personalization models in Amazon Personalize without machine learning expertise\. Some recipes can provide recommendations based on a user's browsing context\. For example, with the User\-Personalization recipe, Amazon Personalize can provide different recommendations when a user is browsing on a mobile device than when that same user is browsing on a desktop\. To help you decide which recipe to use, Amazon Personalize provides metrics to characterize a trained solution version\. 

 For Custom dataset groups, you only need to do the following:

1. Format your historical input data based on custom schema requirements in [Custom datasets and schemas](custom-datasets-and-schemas.md) and upload the data into an Amazon S3 bucket\.

1.  Import your data into an Amazon Personalize dataset and record real\-time event data from user interactions with items\. 

1. Choose a recipe \(algorithm\) to use on the data and when you have collected enough data, train a solution version with the recipe\.

    The minimum data requirements to train a model are as follows: 
   +  1000 records of combined interaction data \(after filtering by `eventType` and `eventValueThreshold`, if provided\)\.
   +  25 unique users with at least 2 interactions each\. 

    Different recipes have additional data requirements\. For more information see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\. 

1. Get recommendations or user segments with the following methods: 
   + Deploy the solution version with a campaign and get recommendations\.
   + Create a batch inference job to get batch recommentations\.
   + Create a user segment job to get user segments\.

## Recommended sections for first\-time Amazon Personalize users<a name="first-time-user"></a>

If you're a first\-time user of Amazon Personalize, we recommend you read the following sections in order:

1. **[How it works](how-it-works.md)** – This section introduces the Amazon Personalize workflow and walks you through the steps to create personalized experiences for your users\. This section also includes common Amazon Personalize terms and their definitions\. Start with this section to learn the terms that will be used throughout this guide\. 

1. **[Setting up Amazon Personalize](setup.md)** – In this section you set up your AWS account, set up the required permissions to use Amazon Personalize, and set up the AWS CLI and the AWS SDKs to use and manage Amazon Personalize\. Complete this section to make sure you have good understanding of Amazon Personalize workflows and terms before you start getting recommendations\.

1. **[Getting started](getting-started.md)** – In this section you get started using Amazon Personalize with a simple movie dataset\. Complete these tutorials to get hands on experience with Amazon Personalize\. You can choose to either get started with a Domain dataset group or a Custom dataset group: 
   +  To get started creating a Domain dataset group, complete the [Getting started prerequisites](gs-prerequisites.md) and then start the tutorials in [Getting started with a Domain dataset group](getting-started-domain.md)\. 
   +  To get started with a Custom dataset group, complete the [Getting started prerequisites](gs-prerequisites.md) and then start the tutorials in [Getting started with a Domain dataset group](getting-started-domain.md)\. 

1. Depending on your application, complete either of the following sections:
   + **[Domain dataset groups](domain-dataset-groups.md)** – If you have a streaming video or e\-commerce application, follow the procedures in this section to create a Domain dataset group and recommenders for your use cases\. These procedures build upon what you learned when completing the Domain dataset group getting started exercises and provide more in depth information about recommenders and domain use cases\. 
   + **[Custom dataset groups](custom-dataset-groups.md)** – If you don't have a streaming video or e\-commerce application, follow the procedures in this section to create a Custom dataset group\. These procedures build upon what you learned when completing the Custom dataset group getting started exercises and provide more in depth information about custom recipes and models\. 

1. **[Recording events](recording-events.md)** – This section covers how to record user interaction events in real\-time\. After you have set up your Amazon Personalize resources, complete this section to learn how to keep your Interactions dataset up to date with your users' behavior by recording interaction *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)* with an event tracker and the [ PutEvents ](API_UBS_PutEvents.md) operation\. 

1. **[Filtering recommendations and user segments](filter.md)** – This section covers how to filter recommendations\. Complete this section to learn how to construct filter expressions to filter recommendations based on custom criteria\. For example, you might not want to recommend products that a user has already purchased, or recommend movies that a user has already watched\. 

## Explore in\-depth tutorials and code samples<a name="experienced-user"></a>

If you're an experienced Amazon Personalize user, you can find in\-depth tutorials and code samples in the [amazon\-personalize\-samples GitHub repository](https://github.com/aws-samples/amazon-personalize-samples)\.