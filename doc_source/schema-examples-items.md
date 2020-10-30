# Items Schema Example<a name="schema-examples-items"></a>

The following example shows an Items schema\. The `ITEM_ID` field is required\. The `GENRE` field is metadata\. At least one metadata field is required\. The `CREATION_TIMESTAMP` field is a reserved keyword\. For information on schema requirements see [Dataset and Schema Requirements](how-it-works-dataset-schema.md#dataset-requirements)\.

```
{
  "type": "record",
  "name": "Items",
  "namespace": "com.amazonaws.personalize.schema",
  "fields": [
    {
      "name": "ITEM_ID",
      "type": "string"
    },
    {
      "name": "GENRES",
      "type": [
        "null",
        "string"
      ],
      "categorical": true
    },
    {
      "name": "CREATION_TIMESTAMP",
      "type": "long"
    }
  ],
  "version": "1.0"
}
```