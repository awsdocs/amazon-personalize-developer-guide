# DescribeSolution<a name="API_DescribeSolution"></a>

Describes a solution\. For more information on solutions, see [CreateSolution](API_CreateSolution.md)\.

## Request Syntax<a name="API_DescribeSolution_RequestSyntax"></a>

```
{
   "[solutionArn](#personalize-DescribeSolution-request-solutionArn)": "string"
}
```

## Request Parameters<a name="API_DescribeSolution_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [solutionArn](#API_DescribeSolution_RequestSyntax) **   <a name="personalize-DescribeSolution-request-solutionArn"></a>
The Amazon Resource Name \(ARN\) of the solution to describe\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

## Response Syntax<a name="API_DescribeSolution_ResponseSyntax"></a>

```
{
   "[solution](#personalize-DescribeSolution-response-solution)": { 
      "[autoMLResult](API_Solution.md#personalize-Type-Solution-autoMLResult)": { 
         "[bestRecipeArn](API_AutoMLResult.md#personalize-Type-AutoMLResult-bestRecipeArn)": "string"
      },
      "[creationDateTime](API_Solution.md#personalize-Type-Solution-creationDateTime)": number,
      "[datasetGroupArn](API_Solution.md#personalize-Type-Solution-datasetGroupArn)": "string",
      "[eventType](API_Solution.md#personalize-Type-Solution-eventType)": "string",
      "[lastUpdatedDateTime](API_Solution.md#personalize-Type-Solution-lastUpdatedDateTime)": number,
      "[latestSolutionVersion](API_Solution.md#personalize-Type-Solution-latestSolutionVersion)": { 
         "[creationDateTime](API_SolutionVersionSummary.md#personalize-Type-SolutionVersionSummary-creationDateTime)": number,
         "[failureReason](API_SolutionVersionSummary.md#personalize-Type-SolutionVersionSummary-failureReason)": "string",
         "[lastUpdatedDateTime](API_SolutionVersionSummary.md#personalize-Type-SolutionVersionSummary-lastUpdatedDateTime)": number,
         "[solutionVersionArn](API_SolutionVersionSummary.md#personalize-Type-SolutionVersionSummary-solutionVersionArn)": "string",
         "[status](API_SolutionVersionSummary.md#personalize-Type-SolutionVersionSummary-status)": "string"
      },
      "[name](API_Solution.md#personalize-Type-Solution-name)": "string",
      "[performAutoML](API_Solution.md#personalize-Type-Solution-performAutoML)": boolean,
      "[performHPO](API_Solution.md#personalize-Type-Solution-performHPO)": boolean,
      "[recipeArn](API_Solution.md#personalize-Type-Solution-recipeArn)": "string",
      "[solutionArn](API_Solution.md#personalize-Type-Solution-solutionArn)": "string",
      "[solutionConfig](API_Solution.md#personalize-Type-Solution-solutionConfig)": { 
         "[algorithmHyperParameters](API_SolutionConfig.md#personalize-Type-SolutionConfig-algorithmHyperParameters)": { 
            "string" : "string" 
         },
         "[autoMLConfig](API_SolutionConfig.md#personalize-Type-SolutionConfig-autoMLConfig)": { 
            "[metricName](API_AutoMLConfig.md#personalize-Type-AutoMLConfig-metricName)": "string",
            "[recipeList](API_AutoMLConfig.md#personalize-Type-AutoMLConfig-recipeList)": [ "string" ]
         },
         "[eventValueThreshold](API_SolutionConfig.md#personalize-Type-SolutionConfig-eventValueThreshold)": "string",
         "[featureTransformationParameters](API_SolutionConfig.md#personalize-Type-SolutionConfig-featureTransformationParameters)": { 
            "string" : "string" 
         },
         "[hpoConfig](API_SolutionConfig.md#personalize-Type-SolutionConfig-hpoConfig)": { 
            "[algorithmHyperParameterRanges](API_HPOConfig.md#personalize-Type-HPOConfig-algorithmHyperParameterRanges)": { 
               "[categoricalHyperParameterRanges](API_HyperParameterRanges.md#personalize-Type-HyperParameterRanges-categoricalHyperParameterRanges)": [ 
                  { 
                     "[name](API_CategoricalHyperParameterRange.md#personalize-Type-CategoricalHyperParameterRange-name)": "string",
                     "[values](API_CategoricalHyperParameterRange.md#personalize-Type-CategoricalHyperParameterRange-values)": [ "string" ]
                  }
               ],
               "[continuousHyperParameterRanges](API_HyperParameterRanges.md#personalize-Type-HyperParameterRanges-continuousHyperParameterRanges)": [ 
                  { 
                     "[maxValue](API_ContinuousHyperParameterRange.md#personalize-Type-ContinuousHyperParameterRange-maxValue)": number,
                     "[minValue](API_ContinuousHyperParameterRange.md#personalize-Type-ContinuousHyperParameterRange-minValue)": number,
                     "[name](API_ContinuousHyperParameterRange.md#personalize-Type-ContinuousHyperParameterRange-name)": "string"
                  }
               ],
               "[integerHyperParameterRanges](API_HyperParameterRanges.md#personalize-Type-HyperParameterRanges-integerHyperParameterRanges)": [ 
                  { 
                     "[maxValue](API_IntegerHyperParameterRange.md#personalize-Type-IntegerHyperParameterRange-maxValue)": number,
                     "[minValue](API_IntegerHyperParameterRange.md#personalize-Type-IntegerHyperParameterRange-minValue)": number,
                     "[name](API_IntegerHyperParameterRange.md#personalize-Type-IntegerHyperParameterRange-name)": "string"
                  }
               ]
            },
            "[hpoObjective](API_HPOConfig.md#personalize-Type-HPOConfig-hpoObjective)": { 
               "[metricName](API_HPOObjective.md#personalize-Type-HPOObjective-metricName)": "string",
               "[metricRegex](API_HPOObjective.md#personalize-Type-HPOObjective-metricRegex)": "string",
               "[type](API_HPOObjective.md#personalize-Type-HPOObjective-type)": "string"
            },
            "[hpoResourceConfig](API_HPOConfig.md#personalize-Type-HPOConfig-hpoResourceConfig)": { 
               "[maxNumberOfTrainingJobs](API_HPOResourceConfig.md#personalize-Type-HPOResourceConfig-maxNumberOfTrainingJobs)": "string",
               "[maxParallelTrainingJobs](API_HPOResourceConfig.md#personalize-Type-HPOResourceConfig-maxParallelTrainingJobs)": "string"
            }
         }
      },
      "[status](API_Solution.md#personalize-Type-Solution-status)": "string"
   }
}
```

## Response Elements<a name="API_DescribeSolution_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [solution](#API_DescribeSolution_ResponseSyntax) **   <a name="personalize-DescribeSolution-response-solution"></a>
An object that describes the solution\.  
Type: [Solution](API_Solution.md) object

## Errors<a name="API_DescribeSolution_Errors"></a>

 **InvalidInputException**   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeSolution_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/DescribeSolution) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/DescribeSolution) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DescribeSolution) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DescribeSolution) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/personalize-2018-05-22/DescribeSolution) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/DescribeSolution) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/DescribeSolution) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/DescribeSolution) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/DescribeSolution) 