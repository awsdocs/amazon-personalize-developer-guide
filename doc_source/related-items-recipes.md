# RELATED\_ITEMS recipes<a name="related-items-recipes"></a>

 The RELATED\_ITEMS recipes return items similar to an item that you specify when you get recommendations\. The RELATED\_ITEMS recipes are as follows: 
+  [Similar\-Items recipe](native-recipe-similar-items.md) 
+  [SIMS recipe](native-recipe-sims.md) 

## [Similar\-Items](native-recipe-similar-items.md)<a name="similar-items-overview"></a>

The Similar\-Items recipe generates recommendations for items that are similar to an item you specify\. Similarity is calculated based on both interactions data and item metadata\. Use Similar\-Items when you have a catalog that includes many new items with item metadata but little to no interactions data\. 

 To use Similar\-Items, you must create an Interactions dataset and an Items dataset\. If you don't have item metadata and want to recommend similar items, use the [SIMS recipe](native-recipe-sims.md) recipe\. If Amazon Personalize can't find the item ID that you specify in your recommendation request, the recipe returns popular items as recommendations\. 

## [SIMS](native-recipe-sims.md)<a name="sims-overview"></a>

The item\-to\-item similarities \(SIMS\) recipe generates items similar to a given item based on the co\-occurrence of the item in user history in your Interactions dataset\. If sufficient user behavior data for an item isn't available, or if the specified item ID isn't found, the recipe returns popular items as recommendations\. Use the SIMS recipe to improve item discovery\. Training is faster with the SIMS recipe compared to other recipes\. 