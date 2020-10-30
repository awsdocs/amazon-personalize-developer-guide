# Interactions Schema Example<a name="schema-examples-interactions"></a>

The following example shows a schema for an Interactions dataset\. The `USER_ID`, `ITEM_ID`, and `TIMESTAMP` fields are required\. The `EVENT_TYPE`, `EVENT_VALUE`, `IMPRESSION`, `RECOMMENDATION_ID` fields are optional reserved keywords recognized by Amazon Personalize\. `LOCATION` and `DEVICE` are optional contextual metadata fields\. For information on schema requirements see [Dataset and Schema Requirements](how-it-works-dataset-schema.md#dataset-requirements)\. 

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
      },
      {
          "name": "RECOMMENDATION_ID",
          "type": "string"
      }
  ],
  "version": "1.0"
}
```