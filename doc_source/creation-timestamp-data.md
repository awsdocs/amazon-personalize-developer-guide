# Creation Timestamp Data<a name="creation-timestamp-data"></a>

Amazon Personalize uses creation timestamp data \(in UNIX epoch time format, in seconds\) to calculate the age of an item and adjust recommendations accordingly\.

If creation timestamp data is missing for one or more items, Amazon Personalize infers this information from interaction data, if any, and uses the timestamp of the item’s oldest interaction data as the item's creation date\. If an item has no interaction data, its creation date is set as the timestamp of the latest interaction in the training set and is considered a new item\. 