# Hyperparameters and HPO<a name="customizing-solution-config-hpo"></a>

Hyperparameters are used to optimize the trained model and are set before training begins\. This contrasts with model parameters whose values are determined during the training process\.

Hyperparameters are specified using the `algorithmHyperParameters` key that is part of the [SolutionConfig](API_SolutionConfig.md) object that is passed to the [CreateSolution](API_CreateSolution.md) API\.

Different recipes use different hyperparameters\. For the available hyperparameters, see the individual recipes in [Using Predefined Recipes](working-with-predefined-recipes.md)\.

Hyperparameter optimization \(HPO\), or tuning, is the task of choosing optimal hyperparameters for a specific learning objective\. The optimal hyperparameters are determined by running many training jobs using different values from the specified ranges of possibilities\. By default, Amazon Personalize does not perform HPO\. To use HPO, set `performHPO` to `true`, and include the `hpoConfig` object\.

Hyperparameters can be categorical, continuous, or integer\-valued\. The `hpoConfig` object has keys that correspond to each of these types, where you specify the hyperparameters and their ranges\. Note that not all hyperparameters can be tuned \(see the recipe tables\)\.

The following is a partial example of a `CreateSolution` request using the [HRNN](native-recipe-hrnn.md) recipe\.

```
{
    "performAutoML": false,
    "recipeArn": "arn:aws:personalize:::recipe/aws-hrnn",
    "performHPO": true,
    "solutionConfig": {
        "algorithmHyperParameters": {
            "hidden_dimension": "55"
        },
        "hpoConfig": {
            "algorithmHyperParameterRanges": {
                "categoricalHyperParameterRanges": [
                    {
                        "name": "recency_mask",
                        "values": [ "true", "false" ]
                    }
                ],
                "integerHyperParameterRanges": [
                    {
                        "name": "bptt",
                        "minValue": 20,
                        "maxValue": 40
                    }
                ]
            },
            "hpoObjective": {
                "metricName": "precision_at_25",
                "metricRegex": "string",
                "type": "float"
            },
            "hpoResourceConfig": {
                "maxNumberOfTrainingJobs": "4",
                "maxParallelTrainingJobs": "2"
            }
        }
    }
}
```

For more information, see [Automatic Model Tuning](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning.html)\.