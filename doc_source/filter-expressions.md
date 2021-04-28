# Filter expressions<a name="filter-expressions"></a>

To configure filters, you must use a properly formatted *filter expression*\. Filter expressions are composed of dataset and property identifiers in `dataset`\.`property` format, along with logical operators, keywords, and values\. For values, you can specify fixed values or add placeholder parameters, which allow you to set the filter criteria when you get recommendations\.

For a complete list of filter expression elements, see [Filter expression elements](#filter-expression-elements)\. For examples of filter expressions, see [Filter expression examples](#filter-expression-examples)\. 

**Note**  
Amazon Personalize ignores case only when matching event types\.

**Topics**
+ [Creating filter expressions](#creating-filter-expressions)
+ [Filter expression examples](#filter-expression-examples)

## Creating filter expressions<a name="creating-filter-expressions"></a>

The general structure of a filter expression is as follows: 

```
EXCLUDE/INCLUDE ItemID WHERE dataset type.property IN/NOT IN (value/parameter)
```

You can either manually create filter expressions or get help with expression syntax and structure by using the [Expression builder](filter-real-time.md#using-filter-expression-builder) in the console\. You can use filter expressions to filter items based on data from the following datasets:
+  **Interactions**: Use filter expressions to include or exclude items that a user has interacted with \(for example, user events such as click or stream\)\. When filtering, the number of interactions Amazon Personalize considers for a user depends on the `max_user_history_length_percentile` and `min_user_history_length_percentile` hyperparameters you defined before training\.  

  For example, if you used *\.99* for the `max_user_history_length_percentile`, and 99% of your users have at most *4* interactions, Amazon Personalize will only filter based on the user's most recent *4* interactions\. If a user has less than the number interactions at the `min_user_history_length_percentile`, Amazon Personalize doesn't consider the user's interactions when filtering\. 

  To filter based on up to 200 historical and streamed interactions for a user, set the `max_user_history_length_percentile` to *1\.0* and retrain the model\. For more information on hyperparameters, see [Step 1: Choosing a recipe](working-with-predefined-recipes.md) and navigate to the *Properties and Hyperparameters* section for your recipe\. 
+  **Items**: Use filter expressions to include or exclude items based on specific item conditions\. 
+  **Users**: Use filter expressions to include or exclude items based on specific properties of the user you are getting recommendations for \(the `CurrentUser`\)\. If you have created a Users dataset, you can add an `IF` condition to your expression to check conditions for the `CurrentUser` regardless of the dataset that is being used in the expression\. 

 When creating a filter expression, note the following limitations: 
+ You can't chain Interaction and Item datasets into one expression\. To create a filter that filters by Interaction and then Item datasets \(or the opposite\), you must chain two or more expressions together\. For more information, see [Combining multiple expressions](#multiple-expression-example)\. 
+  You can't create filter expressions that filter using values with a boolean type in your schema\. To filter based on boolean values, use a schema with a field of type *String* and use the values `"True"` and `"False"` in your data\. Or you can use type *int* or *long* and values `0` and `1`\. 

### Filter expression elements<a name="filter-expression-elements"></a>

Use the following elements to create filter expressions:

**INCLUDE or EXCLUDE**  
Use `INCLUDE` to limit recommendations to only items that meet the filter criteria *OR* use `EXCLUDE` to remove all items that meet the filter criteria\.

**ItemID**  
Use `ItemID` after the `INCLUDE` or `EXCLUDE` element\.

**WHERE**  
Use `WHERE` to check conditions for items\. You must use the `WHERE` element after the `ItemID`\. 

**AND/OR**  
To chain multiple conditions together within the same filter expression, use `AND` or `OR`\. Conditions chained together using `AND` or `OR` can only affect properties of the dataset used in the first condition\.

**Dataset\.property**  
Provide the dataset and the metadata property that you want to filter recommendations by in `dataset`\.`property` format\. For example, to filter based on the genres property in your Items dataset, you would use Items\.genres in your filter expression\. 

**IF condition**  
Use an `IF` condition *only* to check conditions for the `CurrentUser` and only *once* at the end of an expression\. However, you can extend an `IF` condition using `AND`\. 

**CurrentUser\.property**  
 To filter recommendations based on the user you are getting recommendations for, in *only* an IF condition, use `CurrentUser` and provide the user property\. For example, `CurrentUser.AGE`\.  

**IN/NOT IN**  
Use `IN` or `NOT IN` as comparison operators to filter based on matching \(or not matching\) one or more string values\. Amazon Personalize filters only on exact strings\.

**Comparison operators**  
Use =, <, <=, >, >= operators to test numerical data for equality\.

**Asterisk \(\*\) character**  
Use `*` to include or exclude interactions of all types\. Use `*` *only* for filter expressions that use the `EVENT_TYPE` property of an `Interactions` dataset\.

**Pipe separator**  
Use the pipe separator \(`|`\) to chain multiple expressions together\. For more information, see [Combining multiple expressions](#multiple-expression-example)\.

**Parameters**  
For expressions that use `=` and `IN` operators, use the dollar sign \($\) and a parameter name to add a placeholder parameter as a value\. For example, `$GENRES`\. For this example, when you get recommendations, you supply the genre or genres to filter by\. For information on the number of parameters you can use, see [Service quotas](limits.md#limits-table)\.  
You define a parameter name when you add it to an expression\. The parameter name does not have to match the property name\. We recommend that you use a parameter name that is similar to the property name and easy to remember\. You use the parameter name \(case sensitive\) when you apply the filter to recommendations requests\.

## Filter expression examples<a name="filter-expression-examples"></a>

 Use the following examples to learn how to build your own filter expressions\. They are organized by dataset type\. 

 **Interactions** 

The following expression excludes items based on an event type \(such as click\) or event types that you specify when you get recommendations using the `$EVENT_TYPE` parameter\.

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

The following expression excludes items based on a category or categories that you specify when you get recommendations using the `$CATEGORY` parameter\.

```
EXCLUDE ItemID WHERE Items.CATEGORY IN ($CATEGORY)
```

 The following expression excludes items in the `shoe` category that *do not* have a description of `boot`\. 

```
EXCLUDE ItemID WHERE Items.CATEGORY IN ("shoe") AND Items.DESCRIPTION NOT IN ("boot")
```

The following expression includes only items with a genre or genres that you specify when you get recommendations using the `$GENRE` parameter\.

```
INCLUDE ItemID WHERE Items.GENRE IN ($GENRE)
```

 **Users** 

The following expression excludes items with a genre or genres that you specify when you get recommendations using the `$GENRE` parameter, but only if the current user's age is equal to the value that you specify when you get recommendations using the `$AGE` parameter\. 

```
EXCLUDE ItemID WHERE Items.GENRE IN ($GENRE) IF CurrentUser.AGE = $AGE
```

The following expression includes only items in the `watch` category, with a description of `luxury`, if the current user's age is over `18`\.

```
INCLUDE ItemID WHERE Items.CATEGORY IN ("watch") AND Items.DESCRIPTION IN ("luxury") IF CurrentUser.AGE > 18
```

### Combining multiple expressions<a name="multiple-expression-example"></a>

To filter by Items and Interactions datasets with one filter, combine multiple expressions together using a pipe separator \(`|`\)\. Each expression is first evaluated independently and the result is either the union or the intersection of the two results\.

**Matching expressions example**

 If both expressions use `EXCLUDE` or both expressions use `INCLUDE`, the result is the union of the two results as follows \(A and B are different expressions\): 
+ `Exclude A | Exclude B` is equal to `Exclude result from A or result from B`
+ `Include A | Include B` is equal to `Include result from A or result from B`

The following example shows how to combine two expressions that use `INCLUDE`\. The first expression includes only items with a category or categories that you specify when you get recommendations using the `$CATEGORY` parameter\. The second expression includes items the user has marked as a `favorite`\. Recommendations will include only items with the category you specify and items that the user has marked as a favorite\.

```
INCLUDE ItemID WHERE Items.CATEGORY IN ($CATEGORY) | INCLUDE ItemID WHERE Interactions.EVENT_TYPE IN ("favorite")
```

**INCLUDE and EXCLUDE example**

 If one or more expression uses `INCLUDE` and one more expression uses `EXCLUDE`, the result is the subtraction of the `EXCLUDE` expression result from the `INCLUDE` expression result as follows \(A, B, C, and D are different expressions\)\.
+ `Include A | Exclude B` is equal to `Include result from A - result from B`
+  `Include A | Include B | Exclude C | Exclude D` is equal to `Include (A or B) - (C or D)` 

The following example shows how to combine an `INCLUDE` expression and a `EXCLUDE` expression\. The first expression includes only items with a genre or genres that you specify when you get recommendations using the `$GENRE` parameter\. The second expression excludes items that the user has clicked or streamed\. Recommendations will include only items with a genre that you specify that have not have been clicked or streamed\.

```
INCLUDE ItemID WHERE Items.GENRE IN ($GENRE) | EXCLUDE ItemID WHERE Interactions.EVENT_TYPE IN ("click", "stream")
```