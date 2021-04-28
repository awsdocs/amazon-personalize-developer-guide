# Formatting your input data<a name="data-prep-formatting"></a>

The files that you use to import data into Amazon Personalize must map to the schema that you are using\.

Amazon Personalize imports data only from files that are in the comma\-separated values \(CSV\) format\. Amazon Personalize requires the first row of your CSV file to contain column headers\. The column headers in your CSV file need to map to the schema to create the dataset\. Don't enclose headers in quotation marks \("\)\. `TIMESTAMP` and `CREATION_TIMESTAMP` data must be in *UNIX epoch* time format\. For more information see [Timestamp data](#timestamp-data)\. 

**Important**  
If your data includes any non\-ASCII encoded characters, your CSV file must be encoded in UTF\-8 format\.

The following interactions data represents historical user activity from a website that sells movie tickets\. You can use the data to train a model that gives a user movie recommendations based on the activities of other users\.

```
USER_ID,ITEM_ID,EVENT_TYPE,EVENT_VALUE,TIMESTAMP
196,242,click,15,881250949
186,302,click,13,891717742
22,377,click,10,878887116
244,51,click,20,880606923
166,346,click,10,886397596
298,474,click,40,884182806
115,265,click,20,881171488
253,465,click,50,891628467
305,451,click,30,886324817
```

The associated Interactions schema is repeated below\.

```
{
  "type": "record",
  "name": "Interactions",
  "namespace": "com.amazonaws.personalize.schema",
  "fields": [
    {
      "name": "USER_ID",
      "type": "string"
    },
    {
      "name": "ITEM_ID",
      "type": "string"
    },
    { "name": "EVENT_TYPE",
      "type": "string"
    },
    {
      "name": "EVENT_VALUE",
      "type": "float"
    },
    {
      "name": "TIMESTAMP",
      "type": "long"
    }
  ],
  "version": "1.0"
}
```

Amazon Personalize requires the `USER_ID`, `ITEM_ID`, and `TIMESTAMP` fields\. `USER_ID` is the identifier for a user of your application\. `ITEM_ID` is the identifier for a movie\. `EVENT_TYPE` and `EVENT_VALUE` are the identifiers for user activities\. In the sample data, a `click` might represent a movie purchase event and `15` might be the purchase price of the movie\. `TIMESTAMP` represents the Unix epoch time that the movie purchase took place\.

## Timestamp data<a name="timestamp-data"></a>

 Timestamp data, such as `TIMESTAMP` \(for Interactions datasets\) or `CREATION_TIMESTAMP` \(for Items datasets\) data, must be in Unix epoch time format in seconds\. For example, the Epoch timestamp in seconds for date July 31, 2020 is 1596238243\. To convert dates to Unix epoch timestamps use an [Epoch converter \- Unix timestamp converter](https://www.epochconverter.com)\. 

## Formatting explicit impressions<a name="data-prep-including-explicit-impressions"></a>

If you use the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) recipe, you can record and upload impressions data\. Impressions are lists of items that were visible to a user when they interacted with \(for example, clicked or watched\) a particular item\. To upload impressions data in a bulk data import, you manually record each item ID, separating the values with a vertical bar, '\|', character as part of your historical interactions data\. For more information on impressions data, see [Impressions data](interactions-datasets.md#interactions-impressions-data)\. 

The following is a short excerpt from an Interactions dataset that includes explicit impressions in the `IMPRESSION` column\.


| EVENT\_TYPE | IMPRESSION | ITEM\_ID | TIMESTAMP | USER\_ID | 
| --- | --- | --- | --- | --- | 
| click |  73\|70\|17\|95\|96  | 73 |  1586731606  | USER\_1 | 
| click |  35\|82\|78\|57\|20\|63\|1\|90\|76\|75\|49\|71\|26\|24\|25\|6  | 35 |  1586735164  | USER\_2 | 
| \.\.\. | \.\.\. | \.\.\. | \.\.\. | \.\.\. | 

The application showed user `USER_1` items `73`, `70`, `17`, `95`, and `96` and the user ultimately chose item `73`\. When you create a new solution version based on this data, items `70`, `17`, `95`, and `96` will be less frequently recommended to user `USER_1`\.

## Categorical data<a name="data-prep-formatting-categorical"></a>

To include multiple categories for a single item when you use categorical string data, separate the values using the vertical bar, '\|', character\. For example, to match the Items schema from the previous section using two categories, a data row would resemble the following:

```
ITEM_ID,GENRE
item_123,horror|comedy
```

After you format your data, upload it to an Amazon S3 bucket so you can import it into Amazon Personalize\. For more information, see [Uploading to an Amazon S3 bucket](data-prep-upload-s3.md)\.