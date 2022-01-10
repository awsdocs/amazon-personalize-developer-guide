# Creating a schema with SDK for Python \(Boto3\)<a name="python-schema-ex"></a>

1. Define the Avro format schema that you want to use\.

1. Save the schema in a JSON file in the default Python folder\.

1. Create the schema using the following code\.

   ```
   import boto3
   
   personalize = boto3.client('personalize')
   
   with open('schema.json') as f:
       createSchemaResponse = personalize.create_schema(
           name = 'YourSchema',
           schema = f.read()
       )
   
   schema_arn = createSchemaResponse['schemaArn']
   
   print('Schema ARN:' + schema_arn )
   ```

1. Amazon Personalize returns the ARN of the new schema\. Store it for later use\.

Amazon Personalize provides operations for managing schemas\. For example, you can use the [ListSchemas](API_ListSchemas.md) API to get a list of the available schemas\.

After you create a schema, use it with datasets that match the schema\. For more information, see [Formatting your input data](data-prep-formatting.md)\. 