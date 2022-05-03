# VIDEO\_ON\_DEMAND datasets and schemas<a name="VIDEO-ON-DEMAND-datasets-and-schemas"></a>

 When you create a Domain dataset group for the VIDEO\_ON\_DEMAND domain, each dataset type has a default schema with a set of VIDEO\_ON\_DEMAND specific required and recommended fields\. You can either use the default schema or create a new one based on the default schema\. The data you import must match your schema in format and type\. Use the default domain schemas listed in the sections below as a guide to determine what data to import to create your VIDEO\_ON\_DEMAND\-based recommender\.

For information about general Amazon Personalize schema requirements, such as formatting requirements and available field data types, see [Datasets and schemas](how-it-works-dataset-schema.md)\. These requirements apply to all schemas, regardless of domain\.

 The following topics provide information about each dataset's required and recommended fields for the VIDEO\_ON\_DEMAND domain\. Each dataset section includes the default VIDEO\_ON\_DEMAND schema in JSON format\. 

**Topics**
+ [VIDEO\_ON\_DEMAND domain dataset and schema requirements](#VIDEO-ON-DEMAND-dataset-requirements)
+ [Interactions dataset requirements \(VIDEO\_ON\_DEMAND domain\)](VIDEO-ON-DEMAND-interactions-dataset.md)
+ [Users dataset requirements \(VIDEO\_ON\_DEMAND domain\)](VIDEO-ON-DEMAND-users-dataset.md)
+ [Items dataset requirements \(VIDEO\_ON\_DEMAND domain\)](VIDEO-ON-DEMAND-items-dataset.md)

## VIDEO\_ON\_DEMAND domain dataset and schema requirements<a name="VIDEO-ON-DEMAND-dataset-requirements"></a>

Each dataset type has the following required fields and reserved keywords\. Reserved keywords are optional, non\-metadata fields\. These fields are considered reserved because you must define the fields as their required data type when you use them\. Reserved categorical string fields must have `categorical` set to `true`, while reserved string fields can't be categorical\. The keywords can't be in your data\.


| Dataset type | Required fields | Reserved keywords | 
| --- | --- | --- | 
| Interactions \([default schema](VIDEO-ON-DEMAND-interactions-dataset.md#VIDEO-ON-DEMAND-interactions-schema)\) |  USER\_ID \(`string`\) ITEM\_ID \(`string`\) TIMESTAMP \(`long`\) EVENT\_TYPE \(`string` and depending on [use case](domain-use-cases.md), `Watch` and `Click` event types\)  |  EVENT\_VALUE \(`float`, `null`\) IMPRESSION \(`string`\) RECOMMENDATION\_ID \(`string`, `null`\)  | 
| Users \([default schema](VIDEO-ON-DEMAND-users-dataset.md#VIDEO-ON-DEMAND-users-dataset-schema)\) |  USER\_ID \(`string`\) 1 metadata field \(categorical `string` or numerical\)  |  SUBSCRIBTION\_MODEL \(categorical `string`\)  | 
| Items \([default schema](VIDEO-ON-DEMAND-items-dataset.md#VIDEO-ON-DEMAND-items-dataset-schema)\) |  ITEM\_ID \(`string`\) CREATION\_TIMESTAMP \(`long`\) GENRES \(categorical `string`\)  |   PRICE \(`float`, `null`\) DURATION \(`float`, `null`\) GENRE\_L2 \(categorical `string`\) GENRE\_L3 \(categorical `string`\) AVERAGE\_RATING \(`float`, `null`\) PRODUCT\_DESCRIPTION \(`textual`\) CONTENT\_OWNER \(categorical `string`\) CONTENT\_CLASSIFICATION \(categorical `string`\)  | 