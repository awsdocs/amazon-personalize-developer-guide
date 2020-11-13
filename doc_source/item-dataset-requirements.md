# Required Item Data<a name="item-dataset-requirements"></a>

 The training data you provide for each item must match your schema\. At minimum, you must provide an Item ID for each item\. Depending on your schema, item metadata can include empty/null values\. 

During model training, Amazon Personalize considers a maximum of 750 thousand items\. If you import more than 750 thousand items, Amazon Personalize decides which items to include in training, with an emphasis on including new items \(items you recently created with no interactions\) and existing items with recent interactions data\.

 For more information on minimum requirements and maximum data limits for an Items dataset, see [Service Quotas](limits.md#limits-table)\.