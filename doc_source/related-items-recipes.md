# RELATED\_ITEMS recipes<a name="related-items-recipes"></a>

 The RELATED\_ITEMS recipes return items similar to an item that you specify when you get recommendations\. The RELATED\_ITEMS recipes are as follows: 
+  [Similar\-Items recipe](native-recipe-similar-items.md) 
+  [SIMS recipe \(legacy\)](native-recipe-sims.md) 

## [Similar\-Items](native-recipe-similar-items.md)<a name="similar-items-overview"></a>

The Similar\-Items recipe generates recommendations for items that are similar to an item you specify\. It calculates similarity based on both interactions data and, if you provide it, item metadata\. If Amazon Personalize can't find the item ID that you specify in your recommendation request, the recipe returns popular items as recommendations\.

## [SIMS \(legacy\)](native-recipe-sims.md)<a name="sims-overview"></a>

**Note**  
 We recommend using the aws\-similar\-items \(Similar\-Items\) recipe over the legacy SIMS recipe\. Similar\-Items is more accurate and it calculates similarity based on interactions data *and* any item metadata\. SIMS uses only interactions data\. For more information, see [Similar\-Items recipe](native-recipe-similar-items.md)\. 

The item\-to\-item similarities \(SIMS\) recipe generates items similar to a given item based on the co\-occurrence of the item in user history in your Interactions dataset\. If sufficient user behavior data for an item isn't available, or if the specified item ID isn't found, the recipe returns popular items as recommendations\.