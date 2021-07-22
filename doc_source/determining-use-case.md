# Determining your use case<a name="determining-use-case"></a>

Before you use Amazon Personalize, determine your use case to identify what recipe to use to train the model and what data to import\. *Recipes* are Amazon Personalize algorithms that are prepared for specific use cases\. After you import data, you tell Amazon Personalize what recipe to use\. 

**Topics**
+ [Amazon Personalize use cases](#use-cases)
+ [What data to import](#what-data-to-import)

## Amazon Personalize use cases<a name="use-cases"></a>

Before you get started providing personalized experiences for your users, choose your use case from the following and note its corresponding recipe type\.
+ Recommending items for users \(USER\_PERSONALIZATION recipes\)

  To provide recommendations for your users, train your model with a USER\_PERSONALIZATION recipe\. Recommendations can either be *personalized* recommendations, such as recommending movies for a user based on interactions, items, and user data, or *popular* items based on interactions data\.
+ Ranking items for a user \(PERSONALIZED\_RANKING recipes\) 

  To personalize the order of curated lists or search results for your users, train your model with a PERSONALIZED\_RANKING recipe\. PERSONALIZED\_RANKING recipes create a personalized list by re\-ranking a collection of input items based on predicted interest level for a given user\. Personalized lists improve the customer experience and increase customer loyalty and engagement\. 
+  Recommending similar items \(RELATED\_ITEMS recipes\)

  To recommend similar items, such as items frequently bought together or movies that other users have also watched, you should use a RELATED\_ITEMS recipe\. Recommending similar items can help your customers discover items and can increase user conversion rate\. 

## What data to import<a name="what-data-to-import"></a>

 If you are providing personalized recommendations for your users, import data that helps Amazon Personalize recommend items that are new or have fewer user interactions\. This data includes creation timestamp data and unstructured text metadata about your items, as well as impressions data and contextual metadata from your customers' interactions with items\. 

 For personalized recommendations or ranked items, import categorical data, such as a user's gender or an item's genre, to help Amazon Personalize identify underlying patterns that reveal the most relevant items for your users\. For all use cases, you can use categorical metadata to filter recommendations from Amazon Personalize based on a user or item's attributes\. 

 For general information about how Amazon Personalize reads and stores your data, see [Datasets and schemas](how-it-works-dataset-schema.md)\. For more information on the types of data you can import, see [Interactions dataset](interactions-datasets.md), [Items dataset](items-datasets.md), and [Users dataset](users-datasets.md)\. 