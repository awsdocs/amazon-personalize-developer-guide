# User data<a name="users-datasets"></a>

 The user data that you can import into Amazon Personalize includes numerical and categorical metadata about your users, such as gender or loyalty membership\. You import metadata about your users into an Amazon Personalize *Users dataset* 

 This topic provides information about the following types of user data: 

**Topics**
+ [Categorical metadata](#user-categorical-data)

## Categorical metadata<a name="user-categorical-data"></a>

With some recipes and both VIDEO\_ON\_DEMAND and ECOMMERCE domains, Amazon Personalize uses categorical metadata, such as a user's gender or membership status, when identifying underlying patterns that reveal the most relevant items for your users\. With all recipes and domains, you can import categorical metadata and use it to filter recommendations based on a user's attributes\. For information about filtering recommendations see [Filtering recommendations and user segments](filter.md)\. 

Categorical values can have at most 1,000 characters\. If you have a user with a categorical value with more than 1,000 characters, your dataset import job will fail\.

 For Custom dataset groups and custom solutions, recipes that use categorical metadata include the following:
+  [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) 
+  [Personalized\-Ranking](native-recipe-search.md) 
+  [Similar\-Items](native-recipe-similar-items.md) 