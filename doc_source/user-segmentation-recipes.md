# USER\_SEGMENTATION recipes<a name="user-segmentation-recipes"></a>

USER\_SEGMENTATION recipes generate segments of users based on item input data\. Each user segment is sorted in descending order based on the probability that each user will interact with items in your inventory\. Use a USER\_SEGMENTATION recipe to create segments of users who will most likely interact with your catalog based on their item or item attribute preferences\. For example, you might want to create a targeted marketing campaign for users that would most likely watch a particular movie or purchase a particular products by brand\. 

## [Item\-Affinity](item-affinity-recipe.md)<a name="item-affinity-overview"></a>

The Item\-Affinity \(aws\-item\-affinity\) recipe is a USER\_SEGMENTATION recipe that creates a user segment for each item that you specify\.

To train a model, the Item\-Affinity recipe uses the Interactions and Items datasets in your dataset group\. To create user segments, you train a solution version with the Item\-Affinity recipe, and then create a [batch segment job](creating-batch-seg-job.md)\.

## [Item\-Attribute\-Affinity](item-attribute-affinity-recipe.md)<a name="item-attribute-affinity-overview"></a>

The Item\-Attribute\-Affinity \(aws\-item\-attribute\-affinity\) recipe is a USER\_SEGMENTATION recipe that creates a user segment for each item attribute that you specify\.

To train a model, the Item\-Attribute\-Affinity recipe uses the Interactions dataset and Item dataset from a dataset group\. To create user segments, you train a solution version with the Item\-Attribute\-Affinity recipe, and then create a [batch segment job](creating-batch-seg-job.md)\.