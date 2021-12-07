# Creating recommenders \(SDK for Python \(Boto3\)\)<a name="creating-recommenders-sdk"></a>

Create a recommender for a domain use case with the following SDK for Python \(Boto3\) code\. Run this code for each of your domain use cases\. For `recipeArn`, provide the Amazon Resource Name \(ARN\) for your use case\. The available use cases depend on your domain\. For a list of use cases and their ARNs see [Choosing recommender use cases](domain-use-cases.md)\. 

```
import boto3

personalize = boto3.client('personalize')

create_recommender_response = personalize.create_recommender(
  name = 'recommender name',
  recipeArn = 'recipe ARN',
  datasetGroupArn = 'dataset group ARN'     
)

schema_arn = createSchemaResponse['schemaArn']

print('Schema ARN:' + schema_arn )
```