# Custom dataset groups<a name="custom-dataset-groups"></a>

 With Custom dataset groups, you build custom resources for configurable use cases\. You train, and deploy configurable solutions and solution versions \(a trained Amazon Personalize recommendation model\) based on your business needs\. The Custom dataset groups workflow is as follows:

1. Format your historical input data based on custom schema requirements in [Custom datasets and schemas](custom-datasets-and-schemas.md) and upload the data into an Amazon S3 bucket\.

1.  Import your data into an Amazon Personalize dataset and record real\-time event data from user interactions with items\. 

1. Choose a recipe \(algorithm\) to use on the data and when you have collected enough data, train a solution version with the recipe\.

    The minimum data requirements to train a model are as follows: 
   +  1000 records of combined interaction data \(after filtering by `eventType` and `eventValueThreshold`, if provided\)\.
   +  25 unique users with at least 2 interactions each\. 

    Different recipes have additional data requirements\. For more information see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\. 

1. Get recommendations or user segments with the following methods: 
   + Deploy the solution version with a campaign and get recommendations\.
   + Create a batch inference job to get batch recommendations\.
   + Create a user segment job to get user segments\.