# Item data<a name="items-datasets"></a>

 The item data that you can import into Amazon Personalize includes numerical and categorical metadata such as creation timestamp, price, genre, description, and availability\. You import metadata about your items into an Amazon Personalize *Items dataset*\. Some domains and recipes require an Items dataset\. For more information on recipe requirements see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\. 

 This topic provides information about the following types of item data: 

**Topics**
+ [Creation timestamp data](#creation-timestamp-data)
+ [Categorical metadata](#item-categorical-data)
+ [Unstructured text metadata](#text-data)

## Creation timestamp data<a name="creation-timestamp-data"></a>

Amazon Personalize uses creation timestamp data \(in Unix epoch time format, in seconds\) to calculate the age of an item and adjust recommendations accordingly\.

If creation timestamp data is missing for one or more items, Amazon Personalize infers this information from interaction data, if any, and uses the timestamp of the item’s oldest interaction data as the item's creation timestamp\. If an item has no interaction data, its creation timestamp is set as the timestamp of the latest interaction in the training set and Amazon Personalize considers it a new item\. 

## Categorical metadata<a name="item-categorical-data"></a>

 With certain recipes and domains, Amazon Personalize uses categorical metadata, such as an item's genre or color, when identifying underlying patterns that reveal the most relevant items for your users\. You define your own range of values based on your use case\. Categorical metadata can be in any language\. 

 With all recipes and domains, you can import categorical data and use it to filter recommendations based on an item's attributes\. For information about filtering recommendations, see [Filtering recommendations and user segments](filter.md)\. 

Categorical values can have a maximum of 1000 characters\. If you have an item with a categorical value with more than 1000 characters, your dataset import job will fail\. 

For Domain dataset groups, both VIDEO\_ON\_DEMAND and ECOMMERCE domains use categorical metadata\. For Custom dataset groups and custom solutions, recipes that use categorical metadata include the following:
+  [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) 
+  [Personalized\-Ranking](native-recipe-search.md) 
+  [Similar\-Items](native-recipe-similar-items.md) 

## Unstructured text metadata<a name="text-data"></a>

With certain recipes and domains, Amazon Personalize can extract meaningful information from unstructured text metadata, such as product descriptions, product reviews, or movie synopses\. Amazon Personalize uses unstructured text to identify relevant items for your users, particularly when items are new or have less interactions data\. Include unstructured text data in your Items dataset to increase click\-through rates and conversation rates for new items in your catalog\. 

To use unstructured data, add a field with type `string` to your Items schema and set the field's `textual` attribute to `true`\. Then include the text data in your bulk CSV file and incremental item imports\. For bulk CSV files, wrap the text in double quotes\. Use the `\` character to escape any double quotes or \\ characters in your data\. For an example of an Items schema with a field for unstructured text data, see [Items dataset schema example \(custom\)](item-dataset-requirements.md#schema-examples-items)\. For information about importing data into Amazon Personalize, see [Preparing and importing data](data-prep.md)\.

Unstructured text values can have at most 20,000 characters and text must be in English\. Amazon Personalize truncates values that exceed the character limit to 20,000 characters\.

For Domain dataset groups, both VIDEO\_ON\_DEMAND and ECOMMERCE domains use textual metadata\. For Custom dataset groups and custom solutions, recipes that use textual metadata include the following:
+  [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) 
+  [Personalized\-Ranking](native-recipe-search.md) 
+  [Similar\-Items](native-recipe-similar-items.md) 