# Interactions dataset requirements \(custom\)<a name="interactions-dataset-requirements"></a>

An *Interactions dataset* stores historical and real\-time data from interactions between users and items in your catalog\. For information on the types of interactions data Amazon Personalize can use, see [Interactions data](interactions-datasets.md)\.

 The data you provide for each interaction must match your schema\. Depending on your schema, interaction metadata can include empty/null values\. At minimum, you must provide the following for each interaction: 
+ User ID
+ Item ID
+ Timestamp \(in Unix epoch time format\)

 The maximum total number of optional metadata fields you can add to an Interactions dataset, combined with total number of *distinct* event types in your data, is 10\. The metadata fields included in this count are EVENT\_TYPE, EVENT\_VALUE fields along with any custom metadata fields you add to your schema\. The maximum number of metadata fields excluding reserved fields, such as IMPRESSION, is 5\. Categorical values can have at most 1000 characters\. If you have an interaction with a categorical value with more than 1000, your dataset import job will fail\. 

For more information on minimum requirements and maximum data limits for an Interactions dataset, see [Service quotas](limits.md#limits-table)\. 

## Interactions schema example \(custom\)<a name="schema-examples-interactions"></a>

The following example shows a schema for an Interactions dataset\. The `USER_ID`, `ITEM_ID`, and `TIMESTAMP` fields are required\. The `EVENT_TYPE`, `EVENT_VALUE`, and `IMPRESSION` fields are optional reserved keywords recognized by Amazon Personalize\. `LOCATION` and `DEVICE` are optional contextual metadata fields\. For information on schema requirements see [Custom dataset and schema requirements](custom-datasets-and-schemas.md#dataset-requirements)\. 

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
      {
          "name": "EVENT_TYPE",
          "type": "string"
      },
      {
          "name": "EVENT_VALUE",
          "type": [
             "float",
             "null"
          ]
      },
      {
          "name": "LOCATION",
          "type": "string",
          "categorical": true
      },
      {
          "name": "DEVICE",
          "type": [
              "string",
              "null"
          ],
          "categorical": true
      },
      {
          "name": "TIMESTAMP",
          "type": "long"
      },
      {
          "name": "IMPRESSION",
          "type": "string"
      }
  ],
  "version": "1.0"
}
```

For this schema, the first few lines of historical data in a CSV file might look like the following\. Note that some values for EVENT\_VALUE are null\.

```
USER_ID,ITEM_ID,EVENT_TYPE,EVENT_VALUE,LOCATION,DEVICE,TIMESTAMP,IMPRESSION
35,73,click,,Ohio,Tablet,1586731606,73|70|17|95|96|92|55|45|16|97|56|54|33|94|36|10|5|43|19|13|51|90|65|59|38
54,35,watch,0.75,Indiana,Cellphone,1586735164,35|82|78|57|20|63|1|90|76|75|49|71|26|24|25|6|37|85|40|98|32|13|11|54|48
9,33,click,,Oregon,Cellphone,1586735158,68|33|62|6|15|57|45|24|78|89|90|40|26|91|66|31|47|17|99|29|27|41|77|75|14
23,10,watch,0.25,California,Tablet,1586735697,92|89|36|10|39|77|4|27|79|18|83|16|28|68|78|40|50|3|99|7|87|49|12|57|53
27,11,watch,0.55,Indiana,Tablet,1586735763,11|7|39|95|71|1|6|40|41|28|99|53|68|76|0|65|69|36|22|42|34|67|24|20|66
...
...
```