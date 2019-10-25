# Formatting Your Input Data<a name="data-prep-formatting"></a>

The files that you use to import data into Amazon Personalize must map to the schema that you are using\.

Amazon Personalize imports data only from files that are in the comma\-separated values \(CSV\) format\. Amazon Personalize requires the first row of your CSV file to contain column headers\. The column headers in your CSV file need to map to the schema to create the dataset\. Don't enclose headers in quotation marks \("\)\.

For example, the following CSV data sample maps to the Interactions schema shown in [Datasets and Schemas](how-it-works-dataset-schema.md)\. This data represents historical user activity from a website that sells movie tickets\. You can use the data to train a model that gives a user movie recommendations based on the activities of other users\.

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

Amazon Personalize requires the `USER_ID`, `ITEM_ID`, and `TIMESTAMP` fields\. `USER_ID` is the identifier for a user of your application\. `ITEM_ID` is the identifier for a movie\. `EVENT_TYPE` and `EVENT_VALUE` are the identifiers for user activities\. In the sample data, a `click` might represent a movie purchase event and `15` might be the purchase price of the movie\. `TIMESTAMP` represents the Unix time that the movie purchase took place\.

## Categorical Data<a name="data-prep-formatting-categorical"></a>

To include multiple categories for a single item when you use categorical string data, separate the values using the vertical bar, '\|', character\. For example, to match the Items schema from the previous section using two categories, a data row would resemble the following:

```
ITEM_ID,GENRE
item_123,horror|comedy
```

After you format your data, upload it to an Amazon S3 bucket so you can import it into Amazon Personalize\. For more information, see [Uploading to an S3 Bucket](data-prep-upload-s3.md)\.