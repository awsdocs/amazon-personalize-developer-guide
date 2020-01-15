# Limits in Amazon Personalize<a name="limits"></a>

The following sections contain information about Amazon Personalize guidelines and limits\.

**Topics**
+ [Supported AWS Regions](#regions)
+ [Compliance](#ompliance)
+ [Service Limits](#limits-table)

## Supported AWS Regions<a name="regions"></a>

For a list of AWS Regions that support Amazon Personalize, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/personalize.html) in the *Amazon Web Services General Reference*\.

## Compliance<a name="ompliance"></a>

For more information about Amazon Personalize compliance programs, see [AWS Compliance](https://aws.amazon.com/compliance/), [AWS Compliance Programs](https://aws.amazon.com/compliance/programs/), and [AWS Services in Scope by Compliance Program](https://aws.amazon.com/compliance/services-in-scope)\.

## Service Limits<a name="limits-table"></a>

Your AWS account has the following limits for Amazon Personalize\.


| Resource | Limit | 
| --- | --- | 
| Maximum amount of data for an individual dataset \(Users, Items, or Interactions\) for HRNN, SIMS, Popularity\-Count, and Personalized\-Ranking recipes\. | 100 GB | 
| Maximum amount of data for Interactions dataset for HRNN\-metadata and HRNN\-coldstart recipes\. | 100 GB | 
| Maximum amount of combined data for Users and Items datasets for HRNN\-metadata and HRNN\-coldstart recipes\. | 5 GB | 
| Minimum number of unique combined historical and event interactions \(after filtering by eventType and eventValueThreshold, if provided\) required to train a model \(training a model means you create a solution version\)\. | 1000 | 
| Minimum number of unique users, with at least 2 interactions each, required to train a model \(training a model means you create a solution version\)\. | 25 | 
| Maximum number of interactions that are considered by a model during training\. | 500 million | 
| Maximum number of items that are considered by a model during training\. | 750 thousand | 
| Maximum number of cold start items the HRNN\-Coldstart recipe supports to train a model \(training a model means you create a solution version\)\. | 80,000 | 
| Minimum number of cold start items the HRNN\-Coldstart recipe requires to train a model \(training a model means you create a solution version\)\. | 100 | 
| Maximum transaction rate \(GetRecommendations and GetPersonalizedRanking requests\)\. | 2500/sec | 
| Maximum number of GetRecommendations requests per second per campaign\. | 500/sec | 
| Maximum number of GetPersonalizedRanking requests per second per campaign\. | 500/sec | 
| Maximum number of PutEvents requests\. | 1000/sec | 
| Maximum number of events in a PutEvents call\. | 10 | 
| Maximum size of an event\. | 10 KB | 
| Maximum number of input files in a batch inference job\. | 1000 | 
| Maximum size of batch inference job input\. | 1 GB | 
| Maximum number of records per input file in a batch inference job\. | 50 million | 

Your AWS account has the following limits per region for Amazon Personalize\.


| Resource | Limit | 
| --- | --- | 
| Total number of active schemas\. | 500 | 
| Total number of active datasets\. | 500 | 
| Total number of active dataset groups\. | 500 | 
| Total number of active event trackers\. | 500 | 
| Total number of active solutions\. | 500 | 
| Total number of active campaigns\. | 5 | 