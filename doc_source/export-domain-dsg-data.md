# Exporting a dataset<a name="export-domain-dsg-data"></a>

After you've imported data into a Amazon Personalize dataset, you can export the data to an Amazon S3 bucket\. After exporting a dataset, you can verify and inspect the data that Amazon Personalize uses to generate recommendations\. You also can view the user interaction events that you previously recorded in real time and perform offline analysis on your data\. 

You can choose to export only the data that you imported in bulk \(imported using an Amazon Personalize dataset import job\), only the data that you imported individually \(records that you imported using the console or the `PutEvents`, `PutUsers`, or `PutItems` operations\), or both\. 

To export a dataset, you create a dataset export job\. A *dataset export job* is a record export tool that outputs the records in a dataset to one or more CSV files in an Amazon S3 bucket\. The output CSV file includes a header row with column names that match the fields in the dataset's schema\. Amazon Personalize combines duplicate records \(records that match exactly for all fields\) into one record\. 

 You create a dataset export job using the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\. You create a dataset export job for a dataset in a Domain dataset group the same way you would for a dataset in a Custom dataset group\. For step by step instructions see [Exporting a dataset](export-data.md)\. 