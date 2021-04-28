# Importing interactions incrementally<a name="importing-interactions"></a>

To import interaction *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)* incrementally, you create an *[event tracker](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event-tracker)* and then import one or more events into your Interactions dataset\. You can incrementally import historical interaction events using the Amazon Personalize console, or import historical or real\-time events using the AWS Command Line Interface \(AWS CLI\), or the AWS SDKs\.

This section includes information about importing events using the Amazon Personalize console\. We recommend using the Amazon Personalize console to incrementally import *only* historical events\. For information about using the AWS CLI or the AWS SDKs to record events in real\-time, see [Recording events](recording-events.md)\. 

For information about how Amazon Personalize updates filters for new records and how new records influence recommendations, see [Importing records incrementally](incremental-data-updates.md)\. 

**Topics**
+ [Creating an event tracker \(console\)](#event-tracker-console)
+ [Importing events incrementally \(console\)](#importing-interactions-console)

## Creating an event tracker \(console\)<a name="event-tracker-console"></a>

**Note**  
 If you've created an event tracker, you can skip to [Importing events incrementally \(console\)](#importing-interactions-console)\. 

Before you can incrementally import an event to an Interactions dataset, you must create an *[event tracker](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event-tracker)* for the dataset group\. 

**To create an event tracker \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1.  On the **Dataset groups** page, choose the dataset group with the Interactions dataset that you want to import events to\.

1. On the **Dashboard** for the dataset group, in **Install event ingestion SDK**, choose **Start**\. 

1. On the **Configure tracker** page, in **Tracker configurations**, for **Tracker name**, provide a name for the event tracker, and choose **Next**\.

1. The **Install the SDK** page shows the **Tracking ID** for the new event tracker and instructions for using AWS Amplify or AWS Lambda to stream event data\.

   You can ignore this information because you're using the Amazon Personalize console to upload event data\. If you want to stream event data using AWS Amplify or AWS Lambda in the future, you can view this information by choosing the event tracker on the **Event trackers** page\. 

1. Choose **Finish**\. You can now incrementally import events using the console \(see [Importing events incrementally \(console\)](#importing-interactions-console) or record events in real time using the `PutEvents` operation \(see [Recording events](recording-events.md)\)\. 

## Importing events incrementally \(console\)<a name="importing-interactions-console"></a>

 After you create an event tracker, you can incrementally import events to an Interactions dataset\. This procedure assumes you have already created an Interactions dataset\. For information about creating datasets, see [Step 2: Creating a dataset and a schema](data-prep-creating-datasets.md)\.

**To import events incrementally \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign in to your account\.

1. On the **Dataset groups** page, choose the dataset group with the Interactions dataset that you want to import events to\. 

1. In the navigation pane, choose **datasets**\. 

1. On the **Datasets** page, choose the Interactions dataset\. 

1. At the top right of the dataset details page, choose **Modify dataset**, and choose **Create record**\. 

1. In **Create user\-item interaction record\(s\)** page, for **Record input**, enter the event details in JSON format\. The event's field names and values must match the schema that you used when you created the Interactions dataset\. Amazon Personalize provides a JSON template with field names and data types from this schema\. You can import up to 10 events at a time\.

1. Choose **Create record\(s\)**\. In **Response**, the result of the import is listed and a success or failure message is displayed\. 