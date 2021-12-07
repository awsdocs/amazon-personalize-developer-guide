# Amazon Personalize terms<a name="terms"></a>

 This section introduces the terms used in Amazon Personalize\. 

**Topics**
+ [Data import and management](#data-terms)
+ [Training](#training-terms)
+ [Model deployment and recommendations](#deployment-terms)

## Data import and management<a name="data-terms"></a>

The following terms relate to importing, exporting, and formatting data in Amazon Personalize\.

**contextual metadata**  
Interactions data that you collect about a user's browsing context \(such as device used or location\) when an event \(such as a click\) occurs\. Contextual metadata can improve recommendation relevance for new and existing users\.

**dataset**  
A container for data that you upload to Amazon Personalize\. There are three types of Amazon Personalize datasets: Users, Items, and Interactions\.

**dataset group**  
A dataset group serves as container for Amazon Personalize components, including datasets, event trackers, solutions, filters, campaigns, and batch inference jobs\. A dataset group organizes your resources into independent collections, so resources from one dataset group can’t influence resources in any other dataset group\. A dataset group can either be a Domain dataset group or a Custom dataset group\. 

**Domain dataset group**  
A dataset group containing preconfigured resources for different business domains and use cases\. Amazon Personalize manages the life cycle of training models and deployment\. When you create a Domain dataset group, you choose your business domain, import your data, and create recommenders for each of your use cases\. You use your recommender in your application to get recommendations with the GetRecommendations operation\.   
 If you start with a Domain dataset group, you can still add custom resources such as solutions and solution versions trained with recipes for custom use cases\. 

**Custom dataset group**  
A dataset group containing custom resources, including solutions, solution versions, filters, campaigns, and batch inference jobs\. You use a campaign to get recommendations with the GetRecommendations operation\. You manage the life cycle of training models and deployment\. If you start with a Custom dataset group, you can't associate with a domain later\. Instead, create a new Domain dataset group\.

**dataset export job**  
A record export tool that outputs the records in a dataset to one or more CSV files in an Amazon S3 bucket\. The output CSV file includes a header row with column names that match the fields in the dataset's schema\. 

**dataset import job**  
 A bulk import tool that populates your Amazon Personalize dataset with data from a CSV file in your Amazon S3 bucket\. 

**event**  
A user action – such as a click, a purchase, or a video viewing – that you record and upload to an Amazon Personalize Interactions dataset\. You import events in bulk from a CSV file, incrementally with the Amazon Personalize console, and in real\-time\.

**explicit impressions**  
 A list of items that you manually add to an Amazon Personalize Interactions dataset\. Unlike implicit impressions, which Amazon Personalize automatically derives from your recommendation data, you choose what to include in explicit impressions\. 

**implicit impressions**  
 The recommendations that your application shows a user\. Unlike explicit impressions, which you manually add to an Interactions dataset, Amazon Personalize automatically derives implicit impressions from your recommendation data\. 

**impressions data**  
 The list of items that you presented to a user when they interacted with a particular item by clicking it, watching it, purchasing it, and so on\. Amazon Personalize uses impressions data to calculate the relevance of new items for a user based on how frequently users have selected or ignored the same item\. 

**interactions dataset**  
A container for historical and real\-time data that you collect from interactions between users and items \(called *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)*\)\. Interactions data can include *[impressions data](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#impressions-data)* and [contextual metadata](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#contextual-metadata)\.

**items dataset**  
A container for metadata about your items, such as price, genre, or availability\.

**schema**  
A JSON object in [Apache Avro](https://avro.apache.org/docs/current/) format that tells Amazon Personalize about the structure of your data\. Amazon Personalize uses your schema to parse your data\. 

**users dataset**  
A container for metadata about your users, such as age, gender, or loyalty membership\.

## Training<a name="training-terms"></a>

The following terms relate to training a model in Amazon Personalize\.

**item\-to\-item similarities \(SIMS\) recipe**  
 A *[RELATED\_ITEMS](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#related-items)* recipe that uses the data from an Interactions dataset to make recommendations for items that are similar to a specified item\. The SIMS recipe calculates similarity based on the way users interact with items instead of matching item metadata, such as price or color\. 

**item\-affinity**  
A USER\_SEGMENTATION recipe that uses the data from an Interactions dataset and Items dataset to create user segments for each item that you specify based on the likelihood that the users will interact with the item\.

**item\-attribute\-affinity**  
A USER\_SEGMENTATION recipe that uses the data from an Interactions dataset and Items dataset to create a user segment for each item attribute that you specify based on the likelihood that the users will interact with items with the attribute\.

**personalized\-ranking recipe**  
 A *[PERSONALIZED\_RANKING](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#personalized-ranking-recipes)* recipe that ranks a collection of items that you provide based on the predicted interest level for a specific user\. Use the personalized\-ranking recipe to personalize the order of curated lists of items or search results that are personalized for a specific user\. 

**popularity\-count recipe**  
A *[USER\_PERSONALIZATION](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#user-personalization-recipes)* recipe that recommends the items that have had the most interactions with unique users\.

**recommender**  
A Domain dataset group tool that generates recommendations\. You create a recommender for a Domain dataset group and use in your application to get real\-time recommendations with the GetRecommendations API\. When you create a recommender, you specify a use case and Amazon Personalize trains the models backing the recommender with the best configurations for the use case\. 

**recipe**  
An Amazon Personalize algorithm that is preconfigured to predict the items that a user will interact with \(for USER\_PERSONALIZATION recipes\), or calculate items that are similar to specific items that a user has shown interest in \(for RELATED\_ITEMS recipes\), or rank a collection of items that you provide based on the predicted interest for a specific user \(for PERSONALIZED\_RANKING recipes\)\.

**solution**  
The recipe, customized parameters, and trained models \(Solution Versions\) that Amazon Personalize uses to generate recommendations\. 

**solution version**  
A trained model that you create as part of a solution in Amazon Personalize\. You deploy a solution version in a campaign to activate the personalization API that you use to request recommendations\. 

**training mode**  
 The scope of training to be performed when creating a solution version\. There are two different modes: FULL and UPDATE\. FULL mode creates a completely new solution version based on the entirety of the training data from the datasets in your dataset group\. UPDATE incrementally updates the existing solution version to recommend new items that you added since the last training\.   
 With User\-Personalization, Amazon Personalize automatically updates the latest solution version trained with FULL training mode\. See [Automatic updates](native-recipe-new-item-USER_PERSONALIZATION.md#automatic-updates)\. 

**user\-personalization recipe**  
A Hierarchical Recurrent Neural Network \(HRNN\) based *[USER\_PERSONALIZATION](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#user-personalization-recipes)* recipe that predicts the items that a user will interact with\. The user\-personalization recipe can use item exploration and impressions data to generate recommendations for new items\.

## Model deployment and recommendations<a name="deployment-terms"></a>

The following terms relate to deploying and using a model in Amazon Personalize\.

**batch inference job**  
 A tool that imports your batch input data from an Amazon S3 bucket, uses your solution version to generate recommendations, and exports the recommendations to an Amazon S3 bucket\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. Use a batch inference job to get recommendations for large datasets that do not require real\-time updates\. 

**batch segment job**  
 A tool that imports your batch input data from an Amazon S3 bucket, uses your solution version to create user segments, and exports the user segments to an Amazon S3 bucket\. We recommend using a different location for your output data \(either a folder or a different Amazon S3 bucket\)\. Use a batch segment job with a solution backed by a USER\_SEGMENTATION recipe to create segments of users based on the likelihood the user will interact with different items or items with different item attributes\.

**campaign**  
A deployed solution version \(trained model\) with provisioned dedicated transaction capacity for creating real\-time recommendations for your application users\. After you create a campaign, you use the `getRecommendations` or `getPersonalizedRanking` API operations to get recommendations\.

**item exploration**  
 The process that Amazon Personalize uses to test different item recommendations and learn how users respond to new items with no or very little interaction data\. Amazon Personalize uses exploration when you train the model with the user\-personalization recipe\. For real\-time recommendations, you configure exploration at the campaign level\. For batch recommendations, you configure exploration when you create a batch inference job\. 

**recommendations**  
 A list of items that Amazon Personalize predicts a user will interact with\. Depending on the Amazon Personalize recipe used, recommendations can be either a list of items \(USER\_PERSONALIZATION recipes and RELATED\_ITEMS recipes\), or a ranking of a collection of items you provided \(PERSONALIZED\_RANKING recipes\)\. 

**user segments**  
 Lists of user that Amazon Personalize predicts a user will interact with your catalogue\. Depending on the USER\_SEGMENTATION recipe used, you create user segments based on items \(Item\-Affinity recipe\) item metadata \(Item\-Attribute\-Affinity recipe\)\. You create user segments with a batch segment job\. 