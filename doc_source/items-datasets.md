# Items Dataset<a name="items-datasets"></a>

 An *Items dataset* stores metadata about your items\. This might include information such as price, genre, or availability\. 

 When you create an Items dataset, you must also create a schema for the dataset\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\. For an example of a schema for an Items dataset see [Items Schema Example](schema-examples-items.md)\.  For information on schema requirements see [Dataset and Schema Requirements](how-it-works-dataset-schema.md#dataset-requirements)\. 

 This section provides information about required item data and the kinds of item data, including Creation Timestamp data, you can upload for training\. It also includes an [Items Schema Example](schema-examples-items.md)\. For information about importing item data into an Items dataset, see [Preparing and Importing Data](data-prep.md)\. 

 Once you create an Items dataset and import item data, you can then filter recommendations to include or exclude items based on specific item conditions\. For more information see [Filtering Recommendations](filter.md)\. 

**Topics**
+ [Required Item Data](item-dataset-requirements.md)
+ [Creation Timestamp Data](creation-timestamp-data.md)
+ [Items Schema Example](schema-examples-items.md)