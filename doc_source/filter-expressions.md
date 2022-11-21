# Filter expressions<a name="filter-expressions"></a>

To configure filters, you must use a properly formatted *filter expression*\. Filter expressions are composed of dataset and field identifiers in `dataset`\.`field` format, along with logical operators, keywords, and values\. For values, you can specify fixed values or add placeholder parameters set the filter criteria when you get recommendations\.

For a complete list of filter expression elements, see [Filter expression elements](#filter-expression-elements)\. For examples of filter expressions, see [Filter expression examples](#filter-expression-examples)\. 

**Note**  
Amazon Personalize ignores case only when matching event types\.

**Topics**
+ [Creating filter expressions](#creating-filter-expressions)
+ [Filter expression examples](#filter-expression-examples)

## Creating filter expressions<a name="creating-filter-expressions"></a>

The general structure of a filter expression is as follows: 

```
EXCLUDE/INCLUDE ItemID/UserID WHERE dataset type.field IN/NOT IN (value/parameter)
```

You can either manually create filter expressions or get help with expression syntax and structure by using the [Expression builder](filter-real-time.md#using-filter-expression-builder) in the console\. You can use filter expressions to filter items or users based on data from the following datasets:
+  **Interactions**: Use filter expressions to include or exclude items that a user has interacted with from recommendations \(for example, user events such as click or stream\)\. Or include or exclude users that have taken certain actions from user segments \(interactions with certain event types\)\. Interactions filters can't be used with the [Item\-Attribute\-Affinity recipe](item-attribute-affinity-recipe.md)\. 

  Amazon Personalize considers up to 100 of the most recent interactions per user per event type\. This is an adjustable quota\. You can request a quota increase using the [Service Quotas console](https://console.aws.amazon.com/servicequotas/)\. 
+  **Items**: Use filter expressions to include or exclude items based on specific item conditions\. You can't use filters to include or exclude items based on unstructured textual item metadata such as product descriptions\.
+  **Users**: For *item recommendations for users*, use filter expressions to include or exclude *items* from recommendations based on properties of the user you are getting recommendations for \(the `CurrentUser`\)\. If you have a Users dataset, you can add an `IF` condition to your expression to check conditions for the `CurrentUser` regardless of the dataset that is being used in the expression\. 

   For *user segments*, use filter expressions to include or exclude *users* from user segments based on properties of users\. 

 When creating a filter expression, note the following limitations: 
+ You can't use filters to include or exclude items based on unstructured textual item metadata such as product descriptions\.
+ You can't chain Interaction and Item datasets into one expression\. To create a filter that filters by Interaction and then Item datasets \(or the opposite\), you must chain two or more expressions together\. For more information, see [Combining multiple expressions](#multiple-expression-example)\. 
+  You can't create filter expressions that filter using values with a boolean type in your schema\. To filter based on boolean values, use a schema with a field of type *String* and use the values `"True"` and `"False"` in your data\. Or you can use type *int* or *long* and values `0` and `1`\. 
+  The maximum number of distinct dataset fields for a filter, either in one expression or across multiple expressions chained together, is **5**\. The maximum number of distinct dataset fields across all filters in a dataset group is **10**\. 

### Filter expression elements<a name="filter-expression-elements"></a>

Use the following elements to create filter expressions:

**INCLUDE or EXCLUDE**  
Use `INCLUDE` to limit recommendations to only items that meet the filter criteria *OR* use `EXCLUDE` to remove all items that meet the filter criteria\.

**ItemID/UserId**  
Use `ItemID` after the `INCLUDE` or `EXCLUDE` element for filtering item recommendations\. Use `UserId` for filtering users from user segments\.

**WHERE**  
Use `WHERE` to check conditions for items or users\. You must use the `WHERE` element after the `ItemID` or `UserID`\. 

**AND/OR**  
To chain multiple conditions together within the same filter expression, use `AND` or `OR`\. Conditions chained together using `AND` or `OR` can only affect fields of the dataset used in the first condition\.

**Dataset\.field**  
Provide the dataset and the metadata field that you want to filter recommendations by in `dataset`\.`field` format\. For example, to filter based on the genres field in your Items dataset, you would use Items\.genres in your filter expression\. You can't use filters to include or exclude items based on textual item metadata\. 

**IF condition**  
Use an `IF` condition *only* to check conditions for the `CurrentUser` and only *once* at the end of an expression\. However, you can extend an `IF` condition using `AND`\. 

**CurrentUser\.field**  
 To filter item recommendations based on the user you are getting recommendations for, in *only* an IF condition, use `CurrentUser` and provide the user field\. For example, `CurrentUser.AGE`\.  

**IN/NOT IN**  
Use `IN` or `NOT IN` as comparison operators to filter based on matching \(or not matching\) one or more string values\. Amazon Personalize filters only on exact strings\.

**Comparison operators**  
Use =, <, <=, >, >= operators to test numerical data, including data passed in a placeholder parameter, for equality\.

**Asterisk \(\*\) character**  
Use `*` to include or exclude interactions of all types\. Use `*` *only* for filter expressions that use the `EVENT_TYPE` field of an `Interactions` dataset\.

**Pipe separator**  
Use the pipe separator \(`|`\) to chain multiple expressions together\. For more information, see [Combining multiple expressions](#multiple-expression-example)\.

**Parameters**  
For expressions that use comparison operators or the `IN` operator, use the dollar sign \($\) and a parameter name to add a placeholder parameter as a value\. For example, `$GENRES`\. For this example, when you get recommendations, you supply the genre or genres to filter by\. For information on the number of parameters you can use, see [Service quotas](limits.md#limits-table)\.  
You define a parameter name when you add it to an expression\. The parameter name does not have to match the field name\. We recommend that you use a parameter name that is similar to the field name and easy to remember\. You use the parameter name \(case sensitive\) when you apply the filter to recommendations requests\. For an example that shows how to apply a filter with placeholder parameters when using the AWS SDKS, see [Applying a filter \(AWS SDKs\)](filter-real-time.md#applying-filter-sdk)\.

## Filter expression examples<a name="filter-expression-examples"></a>

 Use the filter expressions in the following sections to learn how to build your own filter expressions\. 

### Item recommendation filter expressions<a name="item-recommendation-filter-examples"></a>

The following filter expressions show how to filter item recommendations based on interactions, item metadata, and user metadata\. They are organized by data type\.

 **Interaction data** 

The following expression excludes items based on an event type \(such as click\) or event types that you specify when you get recommendations using the `$EVENT_TYPE` parameter\.

```
EXCLUDE ItemID WHERE Interactions.EVENT_TYPE IN ($EVENT_TYPE)
```

 The following expression excludes items that a user clicked or streamed\.

```
EXCLUDE ItemID WHERE Interactions.EVENT_TYPE IN ("click", "stream")
```

The following expression includes only items that the user has clicked\.

```
INCLUDE ItemID WHERE Interactions.EVENT_TYPE IN ("click")
```

 **Item data** 

The following expression excludes items based on a category or categories that you specify when you get recommendations using the `$CATEGORY` parameter\.

```
EXCLUDE ItemID WHERE Items.CATEGORY IN ($CATEGORY)
```

 The following expression excludes items in the `shoe` category that *do not* have a description of `boot`\. 

```
EXCLUDE ItemID WHERE Items.CATEGORY IN ("shoe") AND Items.DESCRIPTION NOT IN ("boot")
```

The following expression includes only items with a price less than or equal to the price that you specify when you get recommendations using the `$PRICE` parameter\.

```
INCLUDE ItemID WHERE Items.PRICE <= $PRICE
```

The following expression includes only items that have been created earlier than a timestamp \(in Unix epoch time\) that you specify when you get recommendations\.

```
INCLUDE ItemID WHERE Items.CREATION_TIMESTAMP < $DATE
```

The following expression includes only items with a genre or genres that you specify when you get recommendations using the `$GENRE` parameter\.

```
INCLUDE ItemID WHERE Items.GENRE IN ($GENRE)
```

 **User data** 

The following expression excludes items with a genre or genres that you specify when you get recommendations using the `$GENRE` parameter, but only if the current user's age is equal to the value that you specify when you get recommendations using the `$AGE` parameter\. 

```
EXCLUDE ItemID WHERE Items.GENRE IN ($GENRE) IF CurrentUser.AGE = $AGE
```

The following expression includes only items in the `watch` category, with a description of `luxury`, if the current user's age is over `18`\.

```
INCLUDE ItemID WHERE Items.CATEGORY IN ("watch") AND Items.DESCRIPTION IN ("luxury") IF CurrentUser.AGE > 18
```

### User segment filter expressions<a name="user-segment-filter-examples"></a>

The following filter expressions show how to filter user segments based on interactions data and user metadata\. They are organized by data type\.

 **User data** 

The following filter expression includes only users with a membership status equal to the value that you specify when you get user segments\.

```
INCLUDE UserID WHERE Users.MEMBERSHIP_STATUS IN ($MEMBERSHIP)
```

The following filter expression excludes users with an `AGE` less than a value you specify when you get user segments\.

```
EXCLUDE UserID WHERE Users.AGE < $AGE
```

 **Interaction data** 

The following filter expression includes only users who have clicked or rated items\.

```
INCLUDE UserID WHERE Interactions.EVENT_TYPE IN ("click, rating")
```

The following filter expression excludes users from user segments who have interactions with an event type you specify when you get user segments\.

```
EXCLUDE UserID WHERE Interactions.EVENT_TYPE IN ($EVENT_TYPE)
```

### Combining multiple expressions<a name="multiple-expression-example"></a>

To combine multiple expressions together you use a pipe separator \(`|`\)\. Use a combination of expressions when you want to Items and Interactions datasets with one filter\. Each expression is first evaluated independently and the result is either the union or the intersection of the two results\.

**Matching expressions example**

 If both expressions use `EXCLUDE` or both expressions use `INCLUDE`, the result is the union of the two results as follows \(A and B are different expressions\): 
+ `Exclude A | Exclude B` is equal to `Exclude result from A or result from B`
+ `Include A | Include B` is equal to `Include result from A or result from B`

The following example shows how to combine two expressions that use `INCLUDE`\. The first expression includes only items with a category or categories that you specify when you get recommendations using the `$CATEGORY` parameter\. The second expression includes items the user has marked as a `favorite`\. Recommendations will include only items with the category you specify along with items that the user has marked as a favorite\.

```
INCLUDE ItemID WHERE Items.CATEGORY IN ($CATEGORY) | INCLUDE ItemID WHERE Interactions.EVENT_TYPE IN ("favorite")
```

**INCLUDE and EXCLUDE example**

 If one or more expression uses `INCLUDE` and one more expression uses `EXCLUDE`, the result is the subtraction of the `EXCLUDE` expression result from the `INCLUDE` expression result as follows \(A, B, C, and D are different expressions\)\.
+ `Include A | Exclude B` is equal to `Include result from A - result from B`
+  `Include A | Include B | Exclude C | Exclude D` is equal to `Include (A or B) - (C or D)` 

Expression order does not matter: If the EXCLUDE expression comes before the INCLUDE expression, the result is the same\.

The following example shows how to combine an `INCLUDE` expression and a `EXCLUDE` expression\. The first expression includes only items with a genre or genres that you specify when you get recommendations using the `$GENRE` parameter\. The second expression excludes items that the user has clicked or streamed\. Recommendations will include only items with a genre that you specify that have not have been clicked or streamed\.

```
INCLUDE ItemID WHERE Items.GENRE IN ($GENRE) | EXCLUDE ItemID WHERE Interactions.EVENT_TYPE IN ("click", "stream")
```