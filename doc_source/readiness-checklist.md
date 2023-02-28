# Readiness checklist<a name="readiness-checklist"></a>

 After you review how Amazon Personalize works and complete the getting started exercise, you can start getting ready to use Amazon Personalize with your own data\. This checklist provides lists of Amazon Personalize features, requirements, and data guidance\. It can help you plan or you can use it as a reference as you create resources in Amazon Personalize\.

**Topics**
+ [Have you matched your use cases to Amazon Personalize resources?](#readiness-usecases)
+ [Do you have enough interactions data?](#readiness-interact-data)
+ [Do you have a real\-time event streaming architecture in place?](#readiness-events)
+ [Is your data optimized for Amazon Personalize?](#readiness-data-prep)
+ [Do you collect optional data that can improve recommendations?](#readiness-optional-data)
+ [Do you have a plan to test your recommendations?](#readiness-ab-testing)
+ [Do you have additional business goals?](#readiness-business-goals)

## Have you matched your use cases to Amazon Personalize resources?<a name="readiness-usecases"></a>

 Amazon Personalize recommendations can address the following use cases: 
+ Generating personalized recommendations for a user
+ Recommending similar or related items
+ Recommending trending or popular items
+ Re\-ordering by relevance \(only with custom resources\)
+ Generating user segments \(only with custom resources\)

There are two workflows in Amazon Personalize with resources for these use cases:
+ If you have an ecommerce or video on demand app, follow the [Domain dataset group workflow](domain-dataset-groups.md) to create recommenders that are pre\-configured for your domain\.
+ To use more customizable resources, complete the [Custom dataset group workflow](custom-dataset-groups.md)\. If you start with a Domain dataset group, you can still add custom resources\. 

## Do you have enough interactions data?<a name="readiness-interact-data"></a>

For all use cases and recipes, you must have at minimum 1,000 interactions for 25 unique users with at least two interactions each\. For quality recommendations, we recommend that you have at minimum 50,000 interactions from at least 1,000 users with 2 or more interactions each\. 

  If you aren't sure if you have enough data, you can import and analyze it with the Amazon Personalize console\. For more information, see [Analyzing data in datasets](analyzing-data.md)\. 

## Do you have a real\-time event streaming architecture in place?<a name="readiness-events"></a>

 If you don't have enough interactions data, you can use Amazon Personalize to collect additional real\-time event data\. With some recipes and use cases, Amazon Personalize can learn from your user’s most recent activity and update recommendations as they use your application\.

 For information about recording events, including how events impact recommendations, a list of third\-party event tracking services, and sample implementations, see [Recording events](recording-events.md)\. 

## Is your data optimized for Amazon Personalize?<a name="readiness-data-prep"></a>

We recommend you check for the following in your data: 
+ Check for missing values\. We recommend that a minimum of 70% of your records have data for every attribute\. We recommend columns that allow null values be at least 70% complete\. 
+ Fix any inaccuracies or issues in your data, such as inconsistent naming conventions, duplicate categories for an item, mismatched IDs across datasets, or duplicate IDs\. These issues can negatively impact recommendations or lead to unexpected behavior\. For example, you might have both “N/A” and “Not Applicable” in your data, but filter out recommendations based on only “N/A”\. Items marked "Not Applicable" would not be removed by the filter\.

   If an item or user can have multiple categories, such as a movie with multiple genres, combine the categorical values into one attribute and separate each value with the \| operator\. For example, a movie’s GENRES data might be Action \| Adventure \| Thriller\. 
+ Avoid having more than 30 possible categories for a column \(unless the column contains data for only filtering purposes\)\.

For a complete list of data recommendations, and instructions on how you can use Amazon Personalize to identify issues, see [Analyzing data in datasets](analyzing-data.md)\.

## Do you collect optional data that can improve recommendations?<a name="readiness-optional-data"></a>

The following data can help improve your recommendation relevance\.
+  Event type \(required for all Domain dataset group use cases\) 
+  Event value 
+  Contextual metadata 
+ Item and user metadata

 For more information on the types of data Amazon Personalize can use, see [Types of data you can import into Amazon Personalize](data.md)\.

## Do you have a plan to test your recommendations?<a name="readiness-ab-testing"></a>

You can use A/B testing to compare the results of different groups of users interacting with recommendations from different models\. A/B testing can help you compare different recommendation strategies and see if recommendations are helping you achieve your business goals\. 

 To perform A/B testing with Amazon Personalize recommendations, you can use CloudWatch Evidently\. For more information, see [Perform launches and A/B experiments with CloudWatch Evidently](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Evidently.html) in the *Amazon CloudWatch User Guide*\. 

## Do you have additional business goals?<a name="readiness-business-goals"></a>

In some cases, you might have goals in addition to generating relevant recommendations for your users\. For example, you might want to maximize revenue, or promote certain types of items from a certain category\. The following Amazon Personalize features can help:
+ Promotions: You can use promotions to make sure a certain percentage of items satisfy your business requirements\. For more information, see [Promoting items in recommendations \(Custom dataset group\)](promoting-items.md)\.
+ Optimizing for business objective: For some Custom dataset group recipes, you can optimize a solution for a custom objective, such as maximizing streaming minutes or increasing revenue\. For more information, see [Optimizing a solution for an additional objective](optimizing-solution-for-objective.md)\.
+ Filtering recommendations\. Use filters to apply business rules to recommendations\. You can use filters to include or exclude certain types of items from recommendations\. For more information, see [Filtering recommendations and user segments](filter.md)\.