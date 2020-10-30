# Users Dataset<a name="users-datasets"></a>

 A *Users dataset* stores metadata about your users\. This might include information such as age, gender, or loyalty membership\. 

 When you create a Users dataset, you must also create a schema for the dataset\. A *schema* tells Amazon Personalize about the structure of your data and allows Amazon Personalize to parse the data\.  For an example of a Users schema, see [Users Schema Example](schema-examples-users.md)\. For information on schema requirements see [Dataset and Schema Requirements](how-it-works-dataset-schema.md#dataset-requirements)\. 

 This section provides information about required user data and the kinds of user data you can upload for training\. It also includes a [Users Schema Example](schema-examples-users.md)\. For information about importing user data into a Users dataset, see [Preparing and Importing Data](data-prep.md)\. 

 Once you create a Users dataset and add user data, you can then filter recommendations to include or exclude items based on specific user conditions\. For more information see [Filtering Recommendations](filter.md)\. 

**Note**  
RELATED\_ITEMS recipes, such as item\-to\-item similarities \(SIMS\), do not use Users datasets\.

**Topics**
+ [Required User Data](user-dataset-requirements.md)
+ [Users Schema Example](schema-examples-users.md)