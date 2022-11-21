# Interactions data<a name="interactions-datasets"></a>

 In Amazon Personalize, an *interaction* is an *event* that you record and then import as training data\. You can record multiple event types, such as *click*, *watch* or *like*\. For example, if a user *clicks* a particular item and then *likes* the item, and you want Amazon Personalize to use these events as training data, for each event you would record the user's ID, the item's ID, the timestamp \(in Unix time epoch format\), and the event type \(*click* and *like*\)\. You would then add both interaction events to an Interactions dataset\. Once you have recorded enough events, you can train a model and use Amazon Personalize to generate recommendations for users\. For minimum requirements see [Service quotas](limits.md#limits-table)\. 

 Amazon Personalize stores interactions data in an *Interactions dataset*\. For all use cases \(Domain dataset groups\) and recipes \(Custom dataset groups\), your interactions data must have the following: 
+ At minimum 1000 interactions records from users interacting with items in your catalog\. These interactions can be from bulk imports, or streamed events, or both\.
+ At minimum 25 unique user IDs with at least 2 interactions for each\.

 To create a recommender or a custom solution, you must at minimum create an Interactions dataset\. This section provides information about the following types of interactions data you can import into Amazon Personalize\. 

**Topics**
+ [Event type and event value data](#event-type-and-event-value-data)
+ [Contextual metadata](#interactions-contextual-metadata)
+ [Impressions data](#interactions-impressions-data)

## Event type and event value data<a name="event-type-and-event-value-data"></a>

 Interactions datasets can store event type data, such as *click* and *watch* event types, and event value data for each of your events\. 
+  If you create a Domain dataset group for the VIDEO\_ON\_DEMAND or ECOMMERCE domain, all use cases require your data to include an EVENT\_TYPE field\. Different use cases require different event types\. For more information see [Choosing recommender use cases](domain-use-cases.md)\. 

   With a Domain dataset group, Amazon Personalize does not use event value data\. 
+  If you create a Custom dataset group, Amazon Personalize uses event type and event value data to filter events before training\. You can import event type data, or event type *and* event value data\. Import this data to choose the interactions data Amazon Personalize uses in training as follows: 
  + **Choose events based on event type** – To choose records based on type, record a type for each of your events in an EVENT\_TYPE column\. When you configure a solution you'll specify the type and Amazon Personalize will use only records with this type in training\. 

     For example, if your data includes *purchase*, *click*, and *watch* event types, and you want Amazon Personalize to train the model with only *watch* events, you would include each event's type in an EVENT\_TYPE column\. Then, when you create a solution, specify *watch* as the `event type` that Amazon Personalize uses in training\. 

     If your Interactions dataset has multiple event types in an EVENT\_TYPE column, and you do not provide an event type when you configure your solution, Amazon Personalize uses all interactions data for training with equal weight regardless of type\. 
  + **Choose records based on type and value ** – To choose records based on type and value, record an event type and event value for each event\. The value you choose for each event depends on what data you want to exclude and what event types you are recording\. For example, you might match the user activity, such as the percentage of video the user watched for *watch* event types\. 

     When you configure a solution, you set a specific value as a threshold to exclude records from training\. For example, if your EVENT\_VALUE data for events with an EVENT\_TYPE of *watch* is the percentage of a video that a user watched, if you set the event value threshold to 0\.5, and the event type to *watch*, Amazon Personalize trains the model using only *watch* interaction events with an EVENT\_VALUE greater than or equal to 0\.5\. 

## Contextual metadata<a name="interactions-contextual-metadata"></a>

 With certain recipes and recommender use cases, Amazon Personalize can use contextual metadata when identifying underlying patterns that reveal the most relevant items for your users\. Contextual metadata is interactions data you collect on the user's environment at the time of an event, such as their location or device type\. 

Including contextual metadata allows you to provide a more personalized experience for existing users\. For example, if customers shop differently when accessing your catalog from a phone compared to a computer, include contextual metadata about the user's device\. Recommendations will then be more relevant based on how they are browsing\.

 Additionally, contextual metadata helps decrease the cold\-start phase for new or unidentified users\. The cold\-start phase refers to the period when your recommendation engine provides less relevant recommendations due to the lack of historical information regarding that user\. 

For Domain dataset groups, the following recommender use cases can use contextual metadata:
+ [Recommended for you](ECOMMERCE-use-cases.md#recommended-for-you-use-case) \(ECOMMERCE domain\)
+ [Top picks for you](VIDEO_ON_DEMAND-use-cases.md#top-picks-use-case) \(VIDEO\_ON\_DEMAND domain\)

 For Custom dataset groups and custom solutions, recipes that use contextual metadata include the following:
+  [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) 
+  [Personalized\-Ranking](native-recipe-search.md) 

 For more information on contextual information, see the following AWS Machine Learning Blog post: [ Increasing the relevance of your Amazon Personalize recommendations by leveraging contextual information](http://aws.amazon.com/blogs/machine-learning/increasing-the-relevance-of-your-amazon-personalize-recommendations-by-leveraging-contextual-information/)\. 

## Impressions data<a name="interactions-impressions-data"></a>

 If you create a Domain dataset group for the VIDEO\_ON\_DEMAND or ECOMMERCE domain, or use the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) recipe, Amazon Personalize can model impressions data that you upload to an Interactions dataset\. Impressions are lists of items that were visible to a user when they interacted with \(for example, clicked or watched\) a particular item\. 

 Amazon Personalize uses impressions data to determine what items to include in exploration\. *Exploration* is where recommendations include new items with less interactions data or relevance\. The more frequently an item occurs in impressions data, the less likely it is that Amazon Personalize includes the item in exploration\. Impression values can have at most 1000 characters \(including the vertical bar character\)\. 

For information about the benefits of exploration see [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md)\. Amazon Personalize can model two types of impressions: [Implicit impressions](#implicit-impressions-info) and [Explicit impressions](#explicit-impressions-info)\. 

### Implicit impressions<a name="implicit-impressions-info"></a>

*Implicit impressions* are the recommendations, retrieved from Amazon Personalize, that you show the user\. You can integrate them into your recommendation workflow by including the `RecommendationId` \(returned by the [GetRecommendations](API_RS_GetRecommendations.md) and [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) operations\) as input for future [PutEvents](API_UBS_PutEvents.md) requests\. Amazon Personalize derives the implicit impressions based on your recommendation data\. 

 For example, you might have an application that provides recommendations for streaming video\. Your recommendation workflow using implicit impressions might be as follows:

1. You request video recommendations for one of your users using the Amazon Personalize [GetRecommendations](API_RS_GetRecommendations.md) API operation\.

1. Amazon Personalize generates recommendations for the user using your model \(solution version\) and returns them with a `recommendationId` in the API response\.

1. You show the video recommendations to your user in your application\.

1. When your user interacts with \(for example, clicks\) a video, record the choice in a call to the [PutEvents](API_UBS_PutEvents.md) API and include the `recommendationId` as a parameter\. For a code sample see [Recording impressions data](recording-events.md#putevents-including-impressions-data)\.

1. Amazon Personalize uses the `recommendationId` to derive the impression data from the previous video recommendations, and then uses the impression data to guide exploration, where future recommendations include new videos with less interactions data or relevance\. 

   For more information on recording events with implicit impression data, see [Recording impressions data](recording-events.md#putevents-including-impressions-data)\.

### Explicit impressions<a name="explicit-impressions-info"></a>

*Explicit impressions* are impressions that you manually record and send to Amazon Personalize\. Use explicit impressions to manipulate results from Amazon Personalize\. The order of the items has no impact\. 

 For example, you might have a shopping application that provides recommendations for shoes\. If you only recommend shoes that are currently in stock, you can specify these items using explicit impressions\. Your recommendation workflow using explicit impressions might be as follows:

1. You request recommendations for one of your users using the Amazon Personalize [GetRecommendations](API_RS_GetRecommendations.md) API\.

1. Amazon Personalize generates recommendations for the user using your model \(solution version\) and returns them in the API response\.

1. You show the user only the recommended shoes that are in stock\.

1. For real\-time incremental data import, when your user interacts with \(for example, clicks\) a pair of shoes, you record the choice in a call to the [PutEvents](API_UBS_PutEvents.md) API and list the recommended items that are in stock in the `impression` parameter\. For a code sample see [Recording impressions data](recording-events.md#putevents-including-impressions-data)\.

   For importing impressions in historical interactions data, you can list explicit impressions in your csv file and separate each item with a '\|' character\. The vertical bar character counts towards the 1000 character limit\. For an example see [Formatting explicit impressions](data-prep-formatting.md#data-prep-including-explicit-impressions)\.

1. Amazon Personalize uses the impression data to guide exploration, where future recommendations include new shoes with less interactions data or relevance\. 