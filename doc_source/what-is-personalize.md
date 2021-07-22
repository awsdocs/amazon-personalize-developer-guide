# What is Amazon Personalize?<a name="what-is-personalize"></a>

Amazon Personalize is a machine learning service that makes it easy for developers to add individualized recommendations to customers who use their applications\. It reflects the vast experience that Amazon has in building personalization systems\.

You can use Amazon Personalize in a variety of scenarios, such as giving users recommendations based on their preferences and behavior, personalized re\-ranking of results, and personalizing content for emails and notifications\.

Amazon Personalize does not require extensive machine learning experience\. You can build, train, and deploy a solution version \(a trained Amazon Personalize recommendation model\) with the AWS console, the AWS Command Line Interface \(AWS CLI\), or programmatically by using the AWS SDK\. As the developer, you only need to do the following:

1. Format input data and upload the data into an Amazon S3 bucket, or send real\-time event data from user interactions with items\.

1. Select a training recipe \(algorithm\) to use on the data\.

1. Train a solution version using the recipe\.

1. Deploy the solution version\.

Amazon Personalize can capture live events from your users to achieve real\-time personalization\. Amazon Personalize can blend real\-time user activity data with existing user profile and item information \(historical data\) to recommend the most relevant items for the user\. You can also use Amazon Personalize to collect interactions data for new properties, such as a new website, and after enough data has been collected, Amazon Personalize can start to make recommendations\.

To give recommendations to your users, call one of the recommendation APIs, and then create personalized experiences for them\.

Amazon Personalize can improve its recommendations over time as new user activity data is collected\. For example, a new movie rental event by a user can result in more relevant movie recommendations for the user in the future\.

Amazon Personalize can provide recommendations based on a user's browsing context\. For example, Amazon Personalize can provide different recommendations when a user is browsing on a mobile device than when that same user is browsing on a desktop\.

With Amazon Personalize you can train a solution for different use cases\. For example, user personalization, items related to an item, and re\-ranking of items\. You choose a recipe and input data based on your use case\. A recipe performs featurization of your data, and applies a choice of learning algorithms, along with default hyperparameters, and hyperparameter optimization job configuration\.

Recipes in Amazon Personalize allow you to create custom personalization models without needing machine learning expertise\. To help you decide which recipe to use, Amazon Personalize provides extensive metrics on the performance of a trained solution version\.

## Are you a first\-time Amazon Personalize user?<a name="first-time-user"></a>

If you're a first\-time user of Amazon Personalize, we recommend you read the following sections in order:

1. **[How it works](how-it-works.md)** – This section introduces the Amazon Personalize workflow and walks you through the steps to create personalized experiences for your users\. This section also includes common Amazon Personalize terms and their definitions\.

1. **[Setting up Amazon Personalize](setup.md)** – In this section you set up your AWS account, set up the required permissions to use Amazon Personalize, and set up the AWS CLI and the AWS SDKs to use and manage Amazon Personalize\.

1. **[Determining your use case](determining-use-case.md)** – This section introduces various Amazon Personalize components that you work with to create an end\-to\-end experience\.

1. **[Getting started](getting-started.md)** – In this section you get started using Amazon Personalize with a simple movie dataset\. You learn how to create an Amazon Personalize campaign that returns movie recommendations for a user with the Amazon Personalize console, the AWS CLI, and AWS SDKs\. 

1. **[Preparing and importing data](data-prep.md)** – This section describes how to prepare and import your training data into Amazon Personalize\.

1. **[Recording events](recording-events.md)** – This section provides information about improving user recommendations by recording user events\.

1. **[Creating a solution](training-deploying-solutions.md)** – This section provides information about creating a solution version by training a model\.

1. **[Creating a campaign](campaigns.md)** – This section provides information about deploying a solution version as a campaign\.

1. **[Getting recommendations](getting-recommendations.md)** – This section shows how to get recommendations from a campaign\.

1. **[Maintaining recommendation relevance](maintaining-relevance.md)** – This section shows how to maintain the relevance of Amazon Personalize recommendations as your catalog grows and your customers use your application\.

## Are you an experienced Amazon Personalize user?<a name="experienced-user"></a>

If you're an experienced Amazon Personalize user, you can find in\-depth tutorials and code samples in the [amazon\-personalize\-samples GitHub repository](https://github.com/aws-samples/amazon-personalize-samples)\.