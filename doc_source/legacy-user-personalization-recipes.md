# Legacy user personalization recipes<a name="legacy-user-personalization-recipes"></a>

**Note**  
 We recommend using the aws\-user\-personalizaton \(User\-Personalization\) recipe over the legacy HRNN recipes\. User\-Personalization improves upon and unifies the functionality offered by the HRNN recipes\. For more information, see [User\-Personalization recipe](native-recipe-new-item-USER_PERSONALIZATION.md)\. 

The following are legacy USER\_PERSONALIZATION recipes\.
+ [HRNN recipe \(legacy\)](native-recipe-hrnn.md)
+ [HRNN\-Coldstart recipe \(legacy\)](native-recipe-hrnn-coldstart.md)
+ [HRNN\-Metadata recipe \(legacy\)](native-recipe-hrnn-metadata.md)

## Using AutoML to choose an HRNN recipe \(API only\)<a name="training-solution-auto-ml"></a>

Amazon Personalize can automatically choose the most appropriate hierarchical recurrent neural network \(HRNN\) recipe based on its analysis of the input data\. This option is called AutoML\. To perform AutoML, set the `performAutoML` parameter to `true` when you call the [CreateSolution](API_CreateSolution.md) API\. 

You can also specify the list of recipes that Amazon Personalize examines to determine the optimal recipe, based on a metric you specify\. In this case, you call the `CreateSolution` operation, specify `true` for the `performAutoML` parameter, omit the `recipeArn` parameter, and include the `solutionConfig` parameter, specifying the `metricName` and `recipeList` as part of the `autoMLConfig` object\. 

How a recipe is chosen is shown in the following table\. Either `performAutoML`or `recipeArn` must be specified but not both\. AutoML is only performed using the HRNN recipes\.


| performAutoML | recipeArn | solutionConfig | Result | 
| --- | --- | --- | --- | 
| true | omit | omitted | Amazon Personalize chooses the recipe | 
| true | omit | autoMLConfig: metricName and recipeList specified | Amazon Personalize chooses a recipe from the list that optimizes the metric | 
| omit | specified | omitted | You specify the recipe | 
| omit | specified | specified | You specify the recipe and override the default training properties | 

**Note**  
When `performAutoML` is `true`, all parameters of the `solutionConfig` object are ignored except for `autoMLConfig`\.