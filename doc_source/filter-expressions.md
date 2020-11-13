# Filter Expressions<a name="filter-expressions"></a>

To configure filters, you must use a properly formatted *filter expression*\. Filter expressions are composed of dataset and property identifiers in `dataset`\.`property` format, along with logical operators, keywords, and values\. For values, you can specify fixed values or add placeholder parameters, which allow you to set the filter criteria when you get recommendations\.

For a complete list of filter expression elements, see [Filter Expression Elements](#filter-expression-elements)\. For examples of filter expressions, see [Filter Expression Examples](#filter-expression-examples)\. 

**Note**  
Amazon Personalize ignores case only when matching event types\.

## Creating Filter Expressions<a name="creating-filter-expressions"></a>

You can either manually create filter expressions or get help with expression syntax and structure by using the [Expression builder](filter-real-time.md#using-filter-expression-builder) in the console\. You can use filter expressions to filter items based on data from the following datasets:
+  **Interactions**: Use filter expressions to include or exclude items that a user has interacted with \(for example, user events such as click or stream\)\. Amazon Personalize filters recommendations based on the most recent 200 historical and 100 new interaction events recorded in a user's Interactions dataset\.
+  **Items**: Use filter expressions to include or exclude items based on specific item conditions\. 
+  **Users**: Use filter expressions to include or exclude items based on specific `CurrentUser` conditions\. If you have created a Users dataset, you can add `CurrentUser` to any expression regardless of the dataset that is being used in the expression\.

**Note**  
You can't chain Interaction and Item datasets into one expression\. To create a filter that filters by Interaction and then Item datasets \(or the opposite\), you must chain two or more expressions together\. For more information, see [Multiple Expressions Example](#multiple-expression-example)\. 

### Filter Expression Elements<a name="filter-expression-elements"></a>

Use the following elements to create filter expressions:
+ INCLUDE: Use `INCLUDE` to make sure certain items are in recommendations\.
+ EXCLUDE: Use `EXCLUDE` to remove certain items from recommendations\.
+ ItemID: Use `ItemID` after the `INCLUDE` or `EXCLUDE` element\.
+ WHERE: Use `WHERE` to check conditions for items\. You must use the `WHERE` element after the `ItemID`\. 
+ AND/OR: Use `AND` or `OR` to chain multiple conditions together within the same filter expression\. Conditions chained together using `AND` or `OR` can only affect properties of the dataset used in the first condition\.
+ Dataset\.property: Provide the dataset and the metadata property that you want to filter recommendations by in `dataset`\.`property` format\. For example, to filter based on the genres property in your Items dataset, you would use Items\.genres in your filter expression\. 
+ IF: Use `IF` *only* to check conditions for the `CurrentUser` and only *once* at the end of an expression\. However, you can extend an `IF` condition using `AND`\. 
+ IN/NOT IN: Use `IN` or `NOT IN` as comparison operators to filter based on matching \(or not matching\) one or more string values\. Amazon Personalize filters only on exact strings\.
+  Use =, <, <=, >, >= operators to test numerical data for equality\.
+ Use `*` to include or exclude interactions of all types\. Use `*` *only* for filter expressions that use the `EVENT_TYPE` property of an `Interactions` dataset\.
+ Use the pipe separator \(`|`\) to chain multiple expressions together\. For more information, see [Multiple Expressions Example](#multiple-expression-example)\.
+ For expressions that use `=` and `IN` operators, use the dollar sign \($\) and a parameter name to add a placeholder parameter as a value\. For example, `$GENRES`\. For this example, when you get recommendations, you supply the genre or genres to filter by\. For information on the number of parameters you can use, see [Service Quotas](limits.md#limits-table)\.
**Note**  
You define a parameter name when you add it to an expression\. The parameter name does not have to match the property name\. We recommend that you use a parameter name that is similar to the property name and easy to remember\. You use the parameter name \(case sensitive\) when you apply the filter to recommendations requests\.

### Filter Expression Examples<a name="filter-expression-examples"></a>

 Use the following examples to learn how to build your own filter expressions\. They are organized by dataset type\. 

 **Interactions** 

The following expression excludes items based on an event type \(such as click\) that you specify when you get recommendations using the `$EVENT_TYPE` parameter\.

```
EXCLUDE ItemID WHERE Interactions.EVENT_TYPE IN ($EVENT_TYPE)
```

 The following expression excludes items that a user clicked or streamed\.

```
EXCLUDE ItemID WHERE Interactions.EVENT_TYPE IN ("click", "stream")
```

 The following expression includes items that the user has interacted with in any way\.

```
INCLUDE ItemID WHERE Interactions.EVENT_TYPE IN ("*")
```

 **Items** 

The following expression excludes items based on a category that you specify when you get recommendations using the `$CATEGORY` parameter\.

```
EXCLUDE ItemID WHERE Items.CATEGORY IN ($CATEGORY)
```

 The following expression excludes items in the `shoe` category that *do not* have a description of `boot`\. 

```
EXCLUDE ItemID WHERE Items.CATEGORY IN ("shoe") AND Items.DESCRIPTION NOT IN ("boot")
```

 The following expression includes items with a download count equal to a value you specify when you get recommendations using the `$NUMBER_OF_DOWNLOADS` parameter\.

```
INCLUDE ItemID WHERE Items.NUMBER_OF_DOWNLOADS = $NUMBER_OF_DOWNLOADS
```

 **Users** 

The following expression excludes items with a genre that you specify when you get recommendations using the `$GENRE` parameter, but only if the current user's age is equal to the value that you specify when you get recommendations using the `$AGE` parameter\. 

```
EXCLUDE ItemID WHERE Items.GENRE IN ($GENRE) IF CurrentUser.AGE = $AGE
```

The following expression includes items in the `watch` category, with a description of `luxury`, if the current user's age is over `18`\. 

```
INCLUDE ItemID WHERE Items.CATEGORY IN ("watch") AND Items.DESCRIPTION IN ("luxury") IF CurrentUser.AGE > 18
```

#### Multiple Expressions Example<a name="multiple-expression-example"></a>

To filter by multiple datasets, chain multiple expressions together using a pipe separator \(`|`\)\. Each expression is first evaluated independently and the result is a union of the two results\.

The following example includes two expressions\. The first includes items in a genre that you specify when you get recommendations using the `$GENRE` parameter\. The second excludes items that the user has clicked or streamed\. Recommendations will include items with a genre that you specify when you get recommendations, but only items that have not have been clicked or streamed\.

```
INCLUDE ItemID WHERE Items.GENRE IN ($GENRE) | EXCLUDE ItemID WHERE Interactions.EVENT_TYPE IN ("click", "stream")
```