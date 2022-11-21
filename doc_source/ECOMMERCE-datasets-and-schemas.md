# ECOMMERCE datasets and schemas<a name="ECOMMERCE-datasets-and-schemas"></a>

 When you create a Domain dataset group for the ECOMMERCE domain, each dataset type has a default schema with a set of ECOMMERCE\-specific required and recommended fields\. You can use the default schema or create a new one based on the default schema\. The data you import must match your schema in format and type\. Use the default domain schemas listed in the sections below as a guide to determine what data to import to create your ECOMMERCE\-based recommender\.

For information about general Amazon Personalize schema requirements, such as formatting requirements and available field data types, see [Datasets and schemas](how-it-works-dataset-schema.md)\. These requirements apply to all schemas, regardless of domain\.

 The following topics provide information about each dataset's required and recommended fields for the ECOMMERCE domain\. Each dataset section includes the default ECOMMERCE schema in JSON format\. 

**Topics**
+ [ECOMMERCE domain dataset and schema requirements](#ECOMMERCE-dataset-requirements)
+ [Interactions dataset requirements \(ECOMMERCE domain\)](ECOMMERCE-interactions-dataset.md)
+ [Users dataset requirements \(ECOMMERCE domain\)](ECOMMERCE-users-dataset.md)
+ [Items dataset requirements \(ECOMMERCE domain\)](ECOMMERCE-items-dataset.md)

## ECOMMERCE domain dataset and schema requirements<a name="ECOMMERCE-dataset-requirements"></a>

Each dataset type has the following required fields and reserved keywords\. Reserved keywords are optional, non\-metadata fields\. These fields are considered reserved because you must define the fields as their required data type when you use them\. Reserved categorical string fields must have `categorical` set to `true`, while reserved string fields can't be categorical\. The keywords can't be in your data\.


| Dataset type | Required fields | Reserved keywords | 
| --- | --- | --- | 
| Interactions \([default schema](ECOMMERCE-interactions-dataset.md#ECOMMERCE-interactions-schema)\) |  USER\_ID \(`string`\) ITEM\_ID \(`string`\) TIMESTAMP \(`long`\) EVENT\_TYPE \(`string` and depending on [use case](domain-use-cases.md), `Purchase` and `View` event types\)  |  EVENT\_VALUE \(`float`, `null`\) IMPRESSION \(`string`\) RECOMMENDATION\_ID \(`string`, `null`\) EVENT\_ATTRIBUTION\_SOURCE \(`string`, `null`\)  | 
| Users \([default schema](ECOMMERCE-users-dataset.md#ECOMMERCE-users-dataset-schema)\) |  USER\_ID \(`string`\) 1 metadata field \(categorical `string` or numerical\)  |   | 
| Items \([default schema](ECOMMERCE-items-dataset.md#ECOMMERCE-items-dataset-schema)\) |  ITEM\_ID \(`string`\) PRICE \(`float`\) CATEGORY\_L1 \(categorical `string`\)  |  CATEGORY\_L2 \(categorical `string`\) CATEGORY\_L3 \(categorical `string`\) PRODUCT\_DESCRIPTION \(`textual`\) CREATION\_TIMESTAMP \(`long`\) AGE\_GROUP \(categorical `string`\) ADULT \(categorical `string`\) GENDER \(categorical `string`\)  | 