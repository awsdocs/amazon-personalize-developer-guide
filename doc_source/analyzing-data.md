# Analyzing data in datasets<a name="analyzing-data"></a>

After you import data into Amazon Personalize datasets, you can use the Amazon Personalize console to analyze the data\. You can learn about your data through data insights and column and row statistics\. And you can learn what actions you can take to improve your data\. These actions can help you meet Amazon Personalize resource requirements, such as model training requirements, or they can lead to improved recommendations\. 

 After you make any recommended changes, you can import your data again and see if you resolved any issues or improved dataset statistics\. For information on updating data, see [Updating data](updating-datasets.md)\. 

 If you don't see any insights, your data aligns with Amazon Personalize data expectations\. You can analyze data in a Domain dataset group or Custom dataset group\. When generating insights and calculating statistics, Amazon Personalize considers all data in the dataset, including bulk data and records imported individually\. 

**Topics**
+ [Required permissions for analyzing data](#analyze-data-minimum-permissions)
+ [Data insights](#data-insights)
+ [Viewing dataset insights and statistics](#run-analysis-console)

## Required permissions for analyzing data<a name="analyze-data-minimum-permissions"></a>

If you grant your users only the permissions required to perform a task in Amazon Personalize, your AWS Identity and Access Management \(IAM\) policy must include the following additional data insight actions\. If you give users full access to Amazon Personalize, no changes are required\.
+ personalize:CreateDataInsightsJob
+ personalize:ListDataInsightsJob
+ personalize:DescribeDataInsightsJob
+ personalize:GetDataInsight

## Data insights<a name="data-insights"></a>

 The following are the possible data insights and recommended actions\. 


| Insight | Action | 
| --- | --- | 
| The Interactions dataset has only X interactions\. Model training requires a minimum of 1,000 interactions\.  | Import Y additional unique interactions records before training a model\. | 
| The Interactions dataset has only X unique users with two or more interactions\. Model training requires at least 25 such users\.  |  Import at least 2 interactions records each for Y additional users\.  | 
| X% of items in the Items dataset have no interactions in the Interactions dataset, so they might not be recommended\. |  Make sure you import all of your interactions data and check for mismatching IDs between your items and interactions datasets\. If you use the User\-Personalization recipe, modify the exploration configuration to recommend more items without interactions data\.  | 
| X% of users in the Users dataset have no interactions in the Interactions dataset\. These users will receive recommendations for popular items\. |  Make sure you import all of your interactions data and check for mismatching IDs between your Users and Interactions datasets\. Import any additional interactions so more users have interactions data\.  | 
| The <Users or Items or Interactions> dataset has X% rows with a missing value\. This can negatively affect recommendations\. We recommend that all required and optional fields be at least 70% percent complete\. |  Import additional complete records, or import data again without incomplete rows, or import data again with missing values replaced with substitute data, such as the average for numeric columns or the most common value for categorical columns\.  | 
| The following column\(s\) in the <datasetType> dataset are less than 70% complete: <ColumnName, ColumnName\.\.\.>\. This can negatively affect recommendations\. We recommend that all required and optional columns be at least 70% complete\. |  Import additional complete records, or import data again without incomplete rows, or import data again with missing values replaced with substitute data, such as the average for numeric columns or the most common value for categorical columns\.  | 
| The following \(numerical\) column\(s\) have outliers, which can negatively impact recommendations: <ColumnName, ColumnName\.\.\.>\.  |  Check the data in these column\(s\) for inaccuracies, and check your data collection and data processing for issues\. Resolve any inaccuracies and import data again\.  | 
| The following column\(s\) have more than 30 possible categories, which can negatively impact recommendations: <ColumnName, ColumnName\.\.\.>\.  |  Check your categorical data for issues, such as duplicated categories caused by variations in spelling\. Resolve any inaccuracies and import data again\.  | 
|  The following textual metadata column\(s\) are less than 85% percent complete and will not be used in model training: <ColumnName, ColumnName\.\.\.>\.  |  Import additional rows or import the rows again with text data for these column\(s\)\.  | 
|  The Interactions dataset has more than 10 unique event types, which will cause model training to fail\.  |  Check your event type column for inaccuracies such as duplicated event types caused by variations in spelling\. Remove unnecessary event types and import data again\.  | 
|  The Interactions dataset has the same timestamp for all records\. If you use a USER\_SEGMENTATION recipe and all records have the same timestamp, model training will fail\.  |  Check your data for timestamp issues and replace duplicated timestamps with unique timestamps\.  | 

## Viewing dataset insights and statistics<a name="run-analysis-console"></a>

To view insights and statistics on your data in Amazon Personalize datasets, navigate to your datasets in the Amazon Personalize console and choose run analysis\. 

**To view insights and statistics**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1.  On the **Dataset groups** page, choose your dataset group\.

1. From the navigation pane, choose **Datasets** and then choose **Data analysis**\.

1.  At the top right, choose **Run analysis**\. Amazon Personalize starts analyzing your data\. This can take up to 15 minutes\. If successful, the results appear on this page\. 

1. In **Insights**, use the following to filter the insights that appear\.
   + To find insights that include specific language, enter your criteria in **Find insight**\. As you enter text, the list updates to include only insights with the exact string in the insight or recommended action\.
   +  To filter the insights by dataset type, change **All datasets** to the specific dataset type\. The list updates to include only insights related to this dataset\. 

1. To view dataset statistics for a dataset, do the following\.
   + To view general details and statistics about a dataset, such as the number of rows, unique users and unique items in an Interactions dataset, expand the section for the dataset\. 
   + To view detailed statistics for a column, expand the dataset section, choose **Column level statistics** and choose the radio button for the column\.

1.  Correct any issues in your data, import it again, and run another analysis to verify\. For more information on importing data again, see [Updating data](updating-datasets.md)\. 