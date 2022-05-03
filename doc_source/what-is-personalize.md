# What is Amazon Personalize?<a name="what-is-personalize"></a>

Amazon Personalize is a fully managed machine learning service that makes it easy for developers to deliver personalized experiences to their users\. It reflects the knowledge and experience that Amazon has in building personalization systems\.

You can use Amazon Personalize in a variety of scenarios, such as generating recommendations for users based on their preferences and behavior, personalized re\-ranking of results, personalizing content for emails, or creating targeted marketing campaigns based on user segments\. Amazon Personalize does not require machine learning experience\. You can get started quickly with use case optimized resources for your business domain, or you can create your own configurable custom resources\. 

 In Amazon Personalize, you start by creating a dataset group, which is a container for Amazon Personalize components\. Your dataset group can be one of the following: 
+  A *Domain dataset group*, where you create preconfigured resources for different business domains and use cases, such as getting recommendations for similar videos \(VIDEO\_ON\_DEMAND domain\) or best selling items \(ECOMMERCE domain\)\. You choose your business domain, import your data, and create recommenders\. You use recommenders in your application to get recommendations\.  

  Use a Domain dataset group if you have a video on demand or e\-commerce application and want Amazon Personalize to find the best configurations for your use cases\. If you start with a Domain dataset group, you can also add custom resources such as solutions with solution versions trained with recipes for custom use cases\. 
+ A *Custom dataset group*, where you create configurable resources for custom use cases and batch recommendation workflows\. You choose a recipe, train a solution version \(model\), and deploy the solution version with a campaign\. You use a campaign in your application to get recommendations\. 

  Use a Custom dataset group if you don't have a video on demand or e\-commerce application or want to configure and manage only custom resources, or want to get recommendations in a batch workflow\. If you start with a Custom dataset group, you can't associate it with a domain later\. Instead, create a new Domain dataset group\. 

You can create and manage Domain dataset groups and Custom dataset groups with the AWS console, the AWS Command Line Interface \(AWS CLI\), or programmatically with the AWS SDKs\. 

With Domain dataset groups and Custom dataset groups, Amazon Personalize can capture real\-time events from your users to deliver real\-time personalization\. Amazon Personalize can blend real\-time user activity data with existing user profile and item information \(historical data\) to recommend the most relevant items for the user\. You can also use Amazon Personalize to collect interactions data for new properties, such as a new website, and after enough data has been collected, Amazon Personalize can start to make recommendations\.

**Topics**
+ [Pricing for Amazon Personalize](#whatis-pricing)
+ [Recommended sections for first\-time Amazon Personalize users](#first-time-user)
+ [Learn more](#experienced-user)

## Pricing for Amazon Personalize<a name="whatis-pricing"></a>

 With Amazon Personalize, you pay only for what you use\. There are no minimum fees and no upfront commitments\. The costs of Amazon Personalize depend on data processing and storage, training, and number of recommendation requests\.

The [AWS Free Tier](https://aws.amazon.com/free/) allows you a monthly up to 20GB of storage per available AWS region, up to 100 hours of training time per eligible AWS region, and up to 50 TPS\-hours of real\-time recommendations/month\. The Amazon Personalize free tier is valid for the first two months of usage\.

For a complete list of charges and prices, see [Amazon Personalize pricing](https://aws.amazon.com/personalize/pricing/)\.

## Recommended sections for first\-time Amazon Personalize users<a name="first-time-user"></a>

If you're a first\-time user of Amazon Personalize, we recommend you read the following sections in order:

1. **[How it works](how-it-works.md)** – This section introduces the Amazon Personalize workflow and walks you through the steps to create personalized experiences for your users\. This section also includes common Amazon Personalize terms and their definitions\. Start with this section to make sure you have good understanding of Amazon Personalize workflows and terms before you start getting recommendations\. 

1. **[Setting up Amazon Personalize](setup.md)** – In this section you set up your AWS account, set up the required permissions to use Amazon Personalize, and set up the AWS CLI and the AWS SDKs to use and manage Amazon Personalize\.

1. **[Getting started](getting-started.md)** – In this section you get started using Amazon Personalize with a simple movie dataset\. Complete these tutorials to get hands on experience with Amazon Personalize\. You can choose to either get started with a Domain dataset group or a Custom dataset group: 
   +  To get started creating a Domain dataset group, complete the [Getting started prerequisites](gs-prerequisites.md) and then start the tutorials in [Getting started with a Domain dataset group](getting-started-domain.md)\. 
   +  To get started with a Custom dataset group, complete the [Getting started prerequisites](gs-prerequisites.md) and then start the tutorials in [Getting started with a Domain dataset group](getting-started-domain.md)\. 

1. Depending on your application, complete either of the following sections:
   + **[Domain dataset groups](domain-dataset-groups.md)** – If you have a streaming video or e\-commerce application, follow the procedures in this section to create a Domain dataset group and recommenders for your use cases\. These procedures build upon what you learned when completing the Domain dataset group getting started exercises and provide more in depth information about recommenders and domain use cases\. 
   + **[Custom dataset groups](custom-dataset-groups.md)** – If you don't have a streaming video or e\-commerce application, follow the procedures in this section to create a Custom dataset group\. These procedures build upon what you learned when completing the Custom dataset group getting started exercises and provide more in depth information about custom recipes and models\. 

1. **[Recording events](recording-events.md)** – This section covers how to record user interaction events in real\-time\. After you have set up your Amazon Personalize resources, complete this section to learn how to keep your Interactions dataset up to date with your users' behavior by recording interaction *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)* with an event tracker and the [PutEvents](API_UBS_PutEvents.md) operation\. 

1. **[Filtering recommendations and user segments](filter.md)** – This section covers how to filter recommendations\. Complete this section to learn how to construct filter expressions to filter recommendations based on custom criteria\. For example, you might not want to recommend products that a user has already purchased, or recommend movies that a user has already watched\. 

## Learn more<a name="experienced-user"></a>

The following resources provide additional information about Amazon Personalize:
+ For a series of videos on how to use Amazon Personalize, see the [Amazon Personalize Deep Dive Video Series](https://www.youtube.com/watch?v=3gJmhoLaLIo) found on YouTube\.
+ For in\-depth tutorials and code samples, see the [amazon\-personalize\-samples GitHub repository](https://github.com/aws-samples/amazon-personalize-samples)\.