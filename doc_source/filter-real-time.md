# Filtering real\-time recommendations<a name="filter-real-time"></a>

You can filter real\-time recommendations with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or the AWS SDKs\.

**Topics**
+ [Filtering real\-time recommendations \(console\)](#filter-rt-console)
+ [Filtering real\-time recommendations \(AWS CLI\)](#filter-rt-cli)
+ [Filtering real\-time recommendations \(AWS SDKs\)](#filter-rt-sdk)

## Filtering real\-time recommendations \(console\)<a name="filter-rt-console"></a>

To filter real\-time recommendations using the console, create a filter and then apply it to a recommendation request\. 

**Note**  
To filter recommendations using a filter with parameters and a campaign that you deployed before November 10, 2020, you must redeploy the campaign by using the [ UpdateCampaign ](API_UpdateCampaign.md) operation or create a new campaign\.

### Creating a filter \(console\)<a name="creating-filter-console"></a>

 To create a filter in the console, choose the dataset group that contains the campaign you want to filter results for and then provide a filter name and a filter expression\. 

**To create a filter \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\. 

1. Choose the dataset group that contains the campaign that you want to filter results for\.

1. In the navigation page, choose **Filters** and then choose **Create new filter**\. The **Create filter** page displays\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/create-filter-page.png)

1. For **Filter name**, enter a name for your filter\. You will choose the filter by this name when you apply it to a recommendation request\.

1. For **Expression**, choose either **Build expression** or **Add expression manually** and build or insert your expression:
   + To use the expression builder, choose **Build expression**\. The expression builder provides structure, fields, and guidelines for building correctly formatted filter expressions\. For more information, see [Using the filter expression builder](#using-filter-expression-builder)\.
   +  To input your own expression, choose **Add expression manually**\. For more information, see [Filter expression elements](filter-expressions.md#filter-expression-elements)\. 

1. Choose **Finish**\. The filter's overview page shows the filter’s Amazon Resource Name \(ARN\), status, and full filter expression\. To delete the filter, choose **Delete**\. For information about finding and deleting filters after you have left the overview page, see [Deleting a filter \(console\)](#delete-filter-console)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/filter-details-page.png)

### Applying a filter \(console\)<a name="apply-filter-console"></a>

 To apply a filter, on the **Test campaign results** panel for the campaign, choose the filter and enter any filter parameter values\. Then get recommendations for a user\. 

**Important**  
For filter expressions that use an `INCLUDE` element to include items, you must provide values for all parameters that are defined in the expression\. For filters with expressions that use an `EXCLUDE` element to exclude items, you can omit the `filter-values`\. In this case, Amazon Personalize doesn't use that portion of the expression to filter recommendations\.

**To apply a filter \(console\)**

1. In the navigation pane, choose **Campaigns**\.

1. On the **Campaigns** page, choose the target campaign\.

1. For comparison, start by getting recommendations for a user without applying a filter\. Under **Test campaign results**, enter the ID of a user that you want to get recommendations for, and choose **Get recommendations**\. A table containing the user’s top recommendations appears\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/Recommendations_no-filter.PNG)

1. From the **Filter name** menu, choose the filter that you created\. If your filter has any placeholder parameters, the associated fields for each parameter appear\.

1. If you're using a filter with placeholder parameters, for each parameter, enter the value to set the filter criteria\. To use multiple values for one parameter, separate each value with a comma\.

1. Using the same `User ID` as in the earlier step, choose **Get recommendations**\. The recommendations table appears\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/Recommendations_filter.png)

   If the user already bought a recommended item, the filter removes it from the recommendation list\. In this example, items 2657, 2985 were replaced by the most suitable items that the user had not purchased \(items 2641 and 1573\)\.

### Using the filter expression builder<a name="using-filter-expression-builder"></a>

The **Expression builder** on the **Create filter** page provides structure, fields, and guidelines for building correctly formatted filter 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/expression-builder-empty.png)

To build a filter expression:
+ Use the **Action**, **Property**, **Operator**, and **Value** fields to create an expression\. 

  For the **Value**, enter a fixed value or, to set filter criteria when you get recommendations, enter *$* \+ a parameter name\. For example, `$GENRES`\. When you get recommendations, you'll supply the value or values to filter by\. In this example, you would provide a genre or list of genres when you get recommendations\.

  Separate multiple non\-parameter values with a comma\. You cannot add comma\-separated parameters to a filter\.
**Note**  
After you choose a **Property** \(in `dataset.property` format\), the **Property** value for any succeeding rows chained by `AND` or `OR` conditions must use the same `dataset`\.
+  Use the **\+** and **X** buttons to add or delete a row from your expression\. You can't delete the first row\. 
+  For new rows, use the `AND`, `IF`, or `OR` operators on the **AND** menu to create a chain of conditions\. 

  For `IF` conditions:
  + Each expression can contain only one `IF` item\. If you remove an IF condition, the Expression builder removes any `AND` conditions following it\.
  + You can use `IF` conditions only for expressions that filter by the `CurrentUser`\.
+  Choose the **Add expression** button to add an additional filter expression for more precise filtering, including filtering using Items and Interactions datasets\. Each expression is first evaluated independently and the result is a union of the two results\. 
**Note**  
To create a filter that uses both Item and Interaction datasets, you *must* use multiple expressions\.

#### Expression builder example<a name="expression-builder-example"></a>

The following example shows how to build a filter that excludes items with a genre that you specify when you get recommendations \(note the $GENRES placeholder parameter\), and with a `DOWNLOAD_COUNT` of more than `200`, but only if the current user's age is greater than `17`\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/personalize/latest/dg/images/create-filter-expression-builder.png)

### Deleting a filter \(console\)<a name="delete-filter-console"></a>

Deleting a filter removes the filter from the list of filters for a dataset group\.

**Important**  
You can't delete a filter while a batch inference job is in progress\.

**To delete a filter \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\. 

1. From the **Dataset groups** list, choose the dataset group that contains the filter that you want to delete\. 

1. In the navigation pane, choose **Filters**\.

1. From the list of filters, choose the filter that you want to delete and choose **View Details**\. The filter details page appears\.

1. Choose **Delete** and confirm the deletion in the confirmation dialog box\. 

## Filtering real\-time recommendations \(AWS CLI\)<a name="filter-rt-cli"></a>

To filter recommendations using the AWS CLI, you create a filter and then apply it by specifying the filter ARN in a [ GetRecommendations ](API_RS_GetRecommendations.md) or [ GetPersonalizedRanking ](API_RS_GetPersonalizedRanking.md) request\.

**Important**  
To filter recommendations using a filter with parameters and a campaign you deployed before November 10, 2020, you must re\-deploy the campaign by using the [ UpdateCampaign ](API_UpdateCampaign.md) call or create a new campaign\.

### Creating a filter \(AWS CLI\)<a name="creating-filter-cli"></a>

Use the following `create-filter` operation to create a filter and specify the filter expression\. 

Replace the `Filter name` with the name of the filter, and the `Dataset group ARN` with the Amazon Resource Name \(ARN\) of the dataset group\. Replace the sample `filter-expression` with your own filter expression\. 

```
aws personalize create-filter \
  --name Filter name \
  --dataset-group-arn dataset group arn \
  --filter-expression "EXCLUDE ItemID WHERE Items.CATEGORY IN (\"$CATEGORY\")"
```

 If successful, the filter ARN is displayed\. Record it for later use\. To verify that the filter is active, use the [ DescribeFilter ](API_DescribeFilter.md) operation before you use the filter\. 

 For more information about the API, see [ CreateFilter ](API_CreateFilter.md)\. For more information about filter expressions, including examples, see [Creating filter expressions](filter-expressions.md#creating-filter-expressions)\. 

### Applying a filter \(AWS CLI\)<a name="applying-filter-cli"></a>

When you use the `get-recommendations` or `get-personalized-ranking` operations, apply a filter by passing the `filter-arn` and any filter values as parameters\. 

The following is an example of the `get-recommendations` operation\. Replace `Campaign ARN` with the Amazon Resource Name \(ARN\) of your campaign, `User ID` with the ID of the user that you are getting recommendations for, and `Filter ARN` with the ARN of your filter\. 

If your expression has any parameters, include the `filter-values` object\. For each parameter in your filter expression, provide the parameter name \(case sensitive\) and the values\. For example, if your filter expression has a `$GENRE` parameter, provide *"GENRE"* as the key, and a genre or genres, such as `"Comedy"`, as the value\. Separate multiple values with a comma\. For example, `"\"comedy\",\"drama\",\"horror"\"`\. 

**Important**  
For filter expressions that use an `INCLUDE` element to include items, you must provide values for all parameters that are defined in the expression\. For filters with expressions that use an `EXCLUDE` element to exclude items, you can omit the `filter-values`\. In this case, Amazon Personalize doesn't use that portion of the expression to filter recommendations\.

```
aws personalize-runtime get-recommendations \
  --campaign-arn Campaign ARN \
  --user-id User ID \
  --filter-arn Filter ARN \
  --filter-values '{
      "Parameter name": "\"value\"",
      "Parameter name": "\"value1\",\"value2\",\"value3\""
    }'
```

### Deleting a filter \(AWS CLI\)<a name="delete-filter-cli"></a>

 Use the following `delete-filter` operation to delete a filter\. Replace `filter ARN` with the ARN of the filter\. 

```
aws personalize delete-filter --filter-arn Filter ARN
```

## Filtering real\-time recommendations \(AWS SDKs\)<a name="filter-rt-sdk"></a>

To filter recommendations using the AWS SDKs, you create a filter and then apply it by specifying the filter ARN in a [ GetRecommendations ](API_RS_GetRecommendations.md) or [ GetPersonalizedRanking ](API_RS_GetPersonalizedRanking.md) request\.

**Important**  
To filter recommendations using a filter with parameters and a campaign you deployed before November 10, 2020, you must re\-deploy the campaign by using the [ UpdateCampaign ](API_UpdateCampaign.md) call or create a new campaign\.

### Creating a filter \(AWS SDKs\)<a name="creating-filter-sdk"></a>

 Create a new filter with the [ CreateFilter ](API_CreateFilter.md) operation\. 

------
#### [ SDK for Python \(Boto3\) ]

Use the following `create_filter` method to create a filter with the AWS SDK for Python \(Boto3\)\. Replace `Filter Name` with the name of the filter, and `Dataset Group ARN` with the Amazon Resource Name \(ARN\) of the dataset group\. Replace the example `filterExpression` with your own filter expression\.

```
import boto3
 
personalize = boto3.client('personalize')
 
response = personalize.create_filter(
    name = 'Filter Name',
    datasetGroupArn = 'Dataset Group ARN',
    filterExpression = 'EXCLUDE ItemID WHERE Items.CATEGORY IN ($CATEGORY)'
) 
filter_arn = response["filterArn"]
print("Filter ARN: " + filter_arn)
```

Record the filter ARN for later use\. To verify that the filter is active, use the [ DescribeFilter ](API_DescribeFilter.md) operation before using the filter\. For more information about the API, see [ CreateFilter ](API_CreateFilter.md)\. For more information about filter expressions, including examples, see [Creating filter expressions](filter-expressions.md#creating-filter-expressions)\.

------
#### [ SDK for Java 2\.x ]

The following `createFilter` method shows how to create a filter with the AWS SDK for Java 2\.x\. The method returns the ARN \(Amazon Resource Number\) of the new filter\. Pass the following as parameters: an Amazon Personalize service client, a name for the filter, the dataset group where you want to create the filter, and the filter expression\. For more information about filter expressions, including examples, see [Creating filter expressions](filter-expressions.md#creating-filter-expressions)\. 

```
public static String createFilter(PersonalizeClient personalizeClient,
                                 String filterName,
                                 String datasetGroupArn,
                                 String filterExpression) {
    try {
        CreateFilterRequest request = CreateFilterRequest.builder()
                .name(filterName)
                .datasetGroupArn(datasetGroupArn)
                .filterExpression(filterExpression)
                .build();

        return personalizeClient.createFilter(request).filterArn();
    }
    catch(PersonalizeException e) {
        System.err.println(e.awsErrorDetails().errorMessage());
        System.exit(1);
    }
    return "";
}
```

------

### Applying a filter \(AWS SDKs\)<a name="applying-filter-sdk"></a>

When you use the `get_recommendations` or `get_personalized_ranking` methods, apply a filter by passing a `filterArn` and any filter values as parameters\.

 The following code shows how to filter recommendations with the AWS SDK for Python \(Boto3\) or the AWS SDK for Java 2\.x\. 

**Important**  
For filter expressions that use an `INCLUDE` element to include items, you must provide values for all parameters that are defined in the expression\. For filters with expressions that use an `EXCLUDE` element to exclude items, you can omit the `filter-values`\. In this case, Amazon Personalize doesn't use that portion of the expression to filter recommendations\. 

------
#### [ SDK for Python \(Boto3\) ]

Use the following `get_recommendations` method to filter recommendations with the SDK for Python \(Boto3\)\. Replace `Campaign ARN` with the Amazon Resource Name \(ARN\) of your campaign, `User ID` with the ID of the user that you are getting recommendations for, and `Filter ARN` with the ARN of your filter\. 

For `filterValues`, for each optional parameter in your filter expression, provide the parameter name \(case sensitive\) and the value or values\. For example, if your filter expression has a `$GENRES` parameter, provide *"GENRES"* as the key, and a genre or genres, such as `"\"Comedy"\"`, as the value\. For multiple values, separate each value with a comma\. For example, `"\"comedy\",\"drama\",\"horror\""`\. 

```
import boto3

personalize_runtime = boto3.client("personalize-runtime")

response = personalize_runtime.get_recommendations(
    campaignArn = "Campaign ARN",
    userId = "User ID",
    filterArn = "Filter ARN",
    filterValues = {
      "Parameter name": "\"value1\"",
      "Parameter name": "\"value1\",\"value2\",\"value3\""
      ....
    }
)
```

------
#### [ SDK for Java 2\.x ]

Use the following `getFilteredRecs` method to apply a filter to an Amazon Personalize recommendation request\. Pass the following as parameters: an Amazon Personalize runtime service client, the campaign's ARN \(Amazon Resource Name\), the user's userID, the filter's ARN, and any filter parameter names \(case sensitive\) and their values\. For example, if your filter expression has a `$GENRES` parameter, provide *"GENRES"* as the parameter name\. 

The following example uses two parameters, one with two values and one with one value\. Depending on your filter expression, modify the code to add or remove parameterName and parameterValue fields\.

```
public static void getFilteredRecs(PersonalizeRuntimeClient personalizeRuntimeClient,
                                   String campaignArn,
                                   String userId,
                                   String filterArn,
                                   String parameter1Name,
                                   String parameter1Value1,
                                   String parameter1Value2,
                                   String parameter2Name,
                                   String parameter2Value){

    try {

        Map<String, String> filterValues = new HashMap<>();

        filterValues.put(parameter1Name, String.format("\"%1$s\",\"%2$s\"",
                parameter1Value1, parameter1Value2));
        filterValues.put(parameter2Name, String.format("\"%1$s\"",
                parameter2Value));

        GetRecommendationsRequest recommendationsRequest = GetRecommendationsRequest.builder()
                .campaignArn(campaignArn)
                .numResults(20)
                .userId(userId)
                .filterArn(filterArn)
                .filterValues(filterValues)
                .build();

        GetRecommendationsResponse recommendationsResponse = personalizeRuntimeClient.getRecommendations(recommendationsRequest);
        List<PredictedItem> items = recommendationsResponse.itemList();

        for (PredictedItem item: items) {
            System.out.println("Item Id is : "+item.itemId());
            System.out.println("Item score is : "+item.score());
        }
    } catch (PersonalizeRuntimeException e) {
        System.err.println(e.awsErrorDetails().errorMessage());
        System.exit(1);
    }
}
```

------

### Deleting a filter \(AWS Python SDK\)<a name="delete-filter-sdk"></a>

 Use the following `delete_filter` method to delete a filter\. Replace `filter ARN` with the ARN of the filter\. 

```
import boto3
personalize = boto3.client("personalize")

response = personalize.delete_filter(
  filterArn = "filter ARN"
)
```