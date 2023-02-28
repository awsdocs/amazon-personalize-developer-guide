# Amazon Personalize endpoints and quotas<a name="limits"></a>

The following sections contain information about Amazon Personalize guidelines, quotas, and endpoints\. For adjustable quotas, you can request a quota increase using the using the [Service Quotas console](https://console.aws.amazon.com/servicequotas/)\. For more information see [Requesting a quota increase](#requesting-limit-increase)\. 

**Topics**
+ [Amazon Personalize endpoints and regions](#regions)
+ [Compliance](#compliance)
+ [Service quotas](#limits-table)
+ [Requesting a quota increase](#requesting-limit-increase)

## Amazon Personalize endpoints and regions<a name="regions"></a>

For a list of Amazon Personalize endpoints by region, see [AWS regions and endpoints](https://docs.aws.amazon.com/general/latest/gr/personalize.html) in the *Amazon Web Services General Reference*\.

## Compliance<a name="compliance"></a>

For information about Amazon Personalize compliance programs, see [AWS compliance](https://aws.amazon.com/compliance/), [AWS compliance programs](https://aws.amazon.com/compliance/programs/), and [AWS services in scope by compliance program](https://aws.amazon.com/compliance/services-in-scope)\.

## Service quotas<a name="limits-table"></a>

Your AWS account has the following quotas for Amazon Personalize\.


| Resource | Quota | 
| --- |--- |
| Interactions | 
| --- |
| Minimum number of unique combined historical and event interactions \(after filtering by eventType and eventValueThreshold, if provided\) required to train a model \(create a solution version\)\. | 1000 | 
| Maximum number of interactions that are considered by a model during training\. | 500 million \(adjustable\) | 
| Maximum number of distinct event types combined with total number of optional metadata columns in Interactions datasets\. | 10 | 
| Maximum number of metadata columns, excluding reserved fields, in Interactions datasets\. | 5 | 
| Maximum number of characters for categorical data and impression values\. | 1000 | 
| Maximum amount of bulk interactions data per dataset import job with FULL import mode\. | 100 GB \(increases to 1TB with any increase to Interactions considered by a model\) | 
| Maximum amount of bulk interactions data per dataset import job with INCREMENTAL import mode\. | 1 GB | 
| Minimum number of interactions records per dataset import job with FULL or INCREMENTAL import mode\. | 1000 | 
| Users | 
| --- |
| Minimum number of unique users, with at least 2 interactions each, required to train a model \(create a solution version\)\. | 25 | 
| Maximum number of metadata fields for a Users dataset\. | 5 | 
| Maximum number of characters for USER\_ID data values\. | 256 | 
| Maximum number of characters for categorical data values\. | 1000 characters | 
| Maximum amount of bulk user data per dataset import job with FULL import mode\. | 100 GB | 
| Maximum amount of bulk user data per dataset import job with INCREMENTAL import mode\. | 1 GB | 
| Items | 
| --- |
| Maximum number of items that are considered by a model during training and generating recommendations\. | 750,000 | 
| Maximum number of metadata fields for an Items dataset\. | 50 | 
| Maximum number of characters for ITEM\_ID data values\. | 256 | 
| Maximum number of characters for categorical data values\. | 1000 characters | 
| Maximum number of characters for textual data values for Chinese and Japanese languages\. | 7,000 characters | 
| Maximum number of characters for textual data values for all other languages\. | 20,000 characters | 
| Maximum amount of bulk items data per dataset import job with BULK import mode\. | 100 GB | 
| Maximum amount of bulk item data per dataset import job with INCREMENTAL import mode\. | 1 GB | 
| Individual record import APIs | 
| --- |
| Maximum rate of PutEvents requests per dataset group\. | 1000/second | 
| Maximum number of events in a PutEvents call\. | 10 | 
| Maximum size of an event\. | 10 KB | 
| Maximum rate of PutItems requests per dataset group\. | 10/second | 
| Maximum number of items in a PutItems call\. | 10 | 
| Maximum rate of PutUsers requests per dataset group\. | 10/second | 
| Maximum number of users in a PutUsers call\. | 10 | 
| Legacy recipes | 
| --- |
| Maximum amount of combined data for Users and Items datasets for HRNN\-metadata and HRNN\-Coldstart recipes\. | 5 GB | 
| Maximum number of cold start items the HRNN\-Coldstart recipe supports to train a model \(create a solution version\)\. | 80000 | 
| Minimum number of cold start items the HRNN\-Coldstart recipe requires to train a model \(create a solution version\)\. | 100 | 
| Filters | 
| --- |
| Total number of filters per dataset group\. | 10 | 
| Maximum number of distinct dataset fields for a filter\. | 5 | 
| Total number of distinct dataset fields across all filters in a dataset group\. | 10 | 
| Maximum number of interactions per user per event type considered by a filter\. | 100 interactions \(adjustable\) | 
| GetRecommendations / GetPersonalizedRanking requests | 
| --- |
| Maximum transaction rate \(GetRecommendations and GetPersonalizedRanking requests\)\. | 2500/sec | 
| Maximum number of GetRecommendations requests per second per campaign\. | 500/sec | 
| Maximum number of GetPersonalizedRanking requests per second per campaign\. | 500/sec\. | 
| Metric attribution quotas | 
| --- |
| Maximum number of metrics for a metric attribution | 10 | 
| Maximum number of unique event attribution sources | 100 | 
| Batch inference jobs | 
| --- |
| Maximum number of input files in a batch inference job\. | 1000 | 
| Maximum size of batch inference job input\. | 1 GB | 
| Maximum number of records per input file in a batch inference job\. | 50 million | 
| Batch segment jobs | 
| --- |
| Maximum number of queries per input file for Item\-Affinity recipe\. | 500 | 
| Maximum number of queries per input file for Item\-Attribute\-Affinity recipe\. | 10 | 

Your AWS account has the following quotas for each region\.


| Resource | Quota | 
| --- | --- | 
| Total number of active schemas\. | 500 | 
| Total number of active dataset groups\. | 5 \(adjustable\) | 
| Total number of pending or in progress dataset import jobs\. | 5 | 
| Total number of pending or in progress batch inference jobs\. | 5 \(adjustable\) | 
| Total number of pending or in progress batch segment jobs\. | 5 | 
| Total number of pending or in progress solution versions\. | 20 \(adjustable\) | 

Each dataset group has the following quotas\.


| Resource | Quota | 
| --- | --- | 
| Total number of active solutions\. | 10 \(adjustable\) | 
| Total number of active campaigns\. | 5 \(adjustable\) | 
| Total number of recommenders\. | 5 | 
| Total number of filters\. | 10 \(adjustable\) | 
| Total number of distinct dataset fields across all filters\. | 10 | 

## Requesting a quota increase<a name="requesting-limit-increase"></a>

 For adjustable quotas, you can request a quota increase using the [Service Quotas console](https://console.aws.amazon.com/servicequotas/)\. The following Amazon Personalize quotas are adjustable: 
+ Maximum number of interactions that are considered by a model during training\.
+ Active campaigns per dataset group 
+ Active dataset groups
+ Active filters per dataset group 
+ Active solutions per dataset group 
+ Amount of data per incremental import
+ Maximum number of interactions per user per event type considered by a filter
+  Total number of pending or in progress batch inference jobs 
+  Total number of pending or in progress solution versions 
+  Maximum rate of `PutEvents` requests 

 To request a quota increase, use the [Service Quotas console](https://console.aws.amazon.com/servicequotas/) and follow the steps in the [Requesting a quota increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) section of the *Service Quotas User Guide*\. 