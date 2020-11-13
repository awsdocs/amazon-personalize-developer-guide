# Required Interaction Data<a name="interactions-dataset-requirements"></a>

 The training data you provide for each interaction must match your schema\. Depending on your schema, interaction metadata can include empty/null values\. At minimum, you must provide the following for each interaction: 
+ User ID
+ Item ID
+ Timestamp \(in Unix epoch time format\)

The maximum total number of optional metadata fields you can add to an Interactions dataset, combined with total number of *distinct* event types in your data, is 10\. The metadata fields include EVENT\_TYPE, EVENT\_VALUE, IMPRESSION, and RECOMMENDATION\_ID metadata fields along with any custom metadata fields you add to your schema\. 

For more information on minimum requirements and maximum data limits for an Interactions dataset, see [Service Quotas](limits.md#limits-table)\.