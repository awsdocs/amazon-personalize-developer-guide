# Increasing recommendation relevance with contextual metadata<a name="contextual-metadata"></a>

To increase recommendation relevance, include contextual metadata for a user, such as their device type or the time of day, when you get recommendations or get a personalized ranking\. 

To use contextual metadata, you must meet the following requirements: 
+ The schema of the Interactions dataset must have a metadata fields for the contextual data\. For example, a DEVICE field \(see [Datasets and schemas](how-it-works-dataset-schema.md)\)\. 
+ The solution version backing the campaign must use a recipe of type USER\_PERSONALIZATION or PERSONALIZED\_RANKING \(see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\)\. 

 For more information about the benefits of including contextual metadata, see [Contextual metadata](interactions-datasets.md#interactions-contextual-metadata)\. 

You can get recommendations with contextual metadata with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\.

## Getting recommendations using contextual metadata \(AWS Python SDK\)<a name="get-recommendations-metadata-sdk-example"></a>

To increase recommendation relevance, include contextual metadata for a user, such as their device type or the time of day, when you get recommendations or get a personalized ranking\. 

To use contextual metadata, you must meet the following requirements: 
+ The schema of the Interactions dataset must have contextual metadata fields \(see [Datasets and schemas](how-it-works-dataset-schema.md)\)\. 
+ The solution version backing the campaign must use a recipe of type USER\_PERSONALIZATION or PERSONALIZED\_RANKING \(see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\)\. 

 For more information about the benefits of including contextual metadata, see [Contextual metadata](interactions-datasets.md#interactions-contextual-metadata)\. 

Use the following code to get a recommendation based on contextual metadata\. For `context`, for each key\-value pair, provide the metadata field as the key and the context data as the value\. In the following sample code, the key is `DEVICE` and the value is `mobile phone`\. Replace these values and the `Campaign ARN` and `User ID` with your own\. A list of recommended items for the user displays\.

```
import boto3

personalizeRt = boto3.client('personalize-runtime')

response = personalizeRt.get_recommendations(
    campaignArn = 'Campaign ARN',
    userId = 'User ID',
    context = {
      'DEVICE': 'mobile phone'
    }
)

print("Recommended items")
for item in response['itemList']:
    print (item['itemId'])
```