# RELATED\_ITEMS recipes<a name="related-items-recipes"></a>

**Note**  
 All RELATED\_ITEMS recipes use interactions data\. Choose the Similar\-Items recipe if you have also have item metadata and want Amazon Personalize to use it to find similar items\. Or choose the SIMS recipe if you want to configure more hyperparameters for the model, such as the popularity discount factor\. 

 The RELATED\_ITEMS recipes return items similar to an item that you specify when you get recommendations\. The RELATED\_ITEMS recipes are as follows: 
+  [Similar\-Items recipe](native-recipe-similar-items.md) 
+  [SIMS recipe](native-recipe-sims.md) 

## [Similar\-Items](native-recipe-similar-items.md)<a name="similar-items-overview"></a>

The Similar\-Items recipe generates recommendations for items that are similar to an item you specify\. It calculates similarity based on both interactions data and, if you provide it, item metadata\. If Amazon Personalize can't find the item ID that you specify in your recommendation request, the recipe returns popular items as recommendations\.

## [SIMS](native-recipe-sims.md)<a name="sims-overview"></a>

The item\-to\-item similarities \(SIMS\) recipe generates items similar to a given item based on the co\-occurrence of the item in user history in your Interactions dataset\. If sufficient user behavior data for an item isn't available, or if the specified item ID isn't found, the recipe returns popular items as recommendations\.