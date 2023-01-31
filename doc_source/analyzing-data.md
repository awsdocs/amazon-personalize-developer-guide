# Analyzing data in datasets<a name="analyzing-data"></a>

After you import data into Amazon Personalize datasets, you can use the Amazon Personalize console to analyze the data\. You can learn about your data through data insights and column and row statistics\. And you can learn what actions you can take to improve your data\. These actions can help you meet Amazon Personalize resource requirements, such as model training requirements, or they can lead to improved recommendations\. 

 After you make any recommended changes, you can import your data again and see if you resolved any issues or improved dataset statistics\. For information on updating data, see [Updating data](updating-datasets.md)\. 

 If you don't see any insights, your data aligns with Amazon Personalize data expectations\. You can analyze data in a Domain dataset group or Custom dataset group\. When generating insights and calculating statistics, Amazon Personalize considers all data in the dataset, including bulk data and records imported individually\. 

## Viewing dataset insights and statistics<a name="run-analysis-console"></a>

To view insights and statistics on your data in Amazon Personalize datasets, navigate to your datasets in the Amazon Personalize console and choose run analysis\. 

**To view insights and statistics**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1.  On the **Dataset groups** page, choose your dataset group\.

1. From the navigation pane, choose **Datasets** and then choose **Data analysis**\.

1.  At the top right, choose **Run analysis**\. Amazon Personalize starts analyzing your data\. This can take up to 15 minutes\. If successful, the results appear on this page\. 

    Correct any issues in your data, import it again, and run another analysis to verify\. For more information on importing data again, see [Updating data](updating-datasets.md)\. 