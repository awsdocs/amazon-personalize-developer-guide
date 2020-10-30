# Interactions Dataset<a name="interactions-datasets"></a>

 An *Interactions dataset* stores historical and real\-time data from interactions between users and items\. To create a recommendation system using Amazon Personalize, you must at minimum create an Interactions dataset\. 

 In Amazon Personalize, an *interaction* is an *event* that you record and then import as training data\. You can record multiple event types, such as *click*, *watch* or *like*\. For example, if a user *clicks* a particular item and then *likes* the item, and you want Amazon Personalize to use these events as training data, for each event you would record the user's ID, the item's ID, the timestamp \(in Unix time epoch format\), and the event type \(*click* and *like*\)\. You would then add both interaction events to an Interactions dataset\. Once you have recorded enough events, you can train a model and use Amazon Personalize to generate recommendations for users\. For minimum requirements see [Service Quotas](limits.md#limits-table)\. 

 When you create an Interactions dataset, you must also create a schema for the dataset\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\.  For an example of a schema for an Interactions dataset see [Interactions Schema Example](schema-examples-interactions.md)\. For information on schema requirements see [Dataset and Schema Requirements](how-it-works-dataset-schema.md#dataset-requirements)\. 

 This section provides information about the kinds of interactions data, including impressions data and contextual metadata, you can upload for training\. It also includes an [Interactions Schema Example](schema-examples-interactions.md)\. For information about importing historical interactions data, see [Preparing and Importing Data](data-prep.md)\. For information about recording events in real\-time using the [PutEvents](API_UBS_PutEvents.md) API, see [Recording Events](recording-events.md)\. 

 Once you create an Interactions dataset and import interaction data, you can then filter recommendations to include or exclude items that a user has interacted with\. For more information see [Filtering Recommendations](filter.md)\. 

**Topics**
+ [Required Interaction Data](interactions-dataset-requirements.md)
+ [Impressions Data](interactions-impressions-metadata.md)
+ [Contextual Metadata](interactions-contextual-metadata.md)
+ [Interactions Schema Example](schema-examples-interactions.md)