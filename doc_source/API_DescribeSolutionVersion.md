# DescribeSolutionVersion<a name="API_DescribeSolutionVersion"></a>

Describes a specific version of a solution\. For more information on solutions, see [CreateSolution](API_CreateSolution.md)\.

## Request Syntax<a name="API_DescribeSolutionVersion_RequestSyntax"></a>

```
{
   "[solutionVersionArn](#personalize-DescribeSolutionVersion-request-solutionVersionArn)": "string"
}
```

## Request Parameters<a name="API_DescribeSolutionVersion_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [solutionVersionArn](#API_DescribeSolutionVersion_RequestSyntax) **   <a name="personalize-DescribeSolutionVersion-request-solutionVersionArn"></a>
The Amazon Resource Name \(ARN\) of the solution version\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

## Response Syntax<a name="API_DescribeSolutionVersion_ResponseSyntax"></a>

```
{
   "[solutionVersion](#personalize-DescribeSolutionVersion-response-solutionVersion)": { 
      "[creationDateTime](API_SolutionVersion.md#personalize-Type-SolutionVersion-creationDateTime)": number,
      "[datasetGroupArn](API_SolutionVersion.md#personalize-Type-SolutionVersion-datasetGroupArn)": "string",
      "[eventType](API_SolutionVersion.md#personalize-Type-SolutionVersion-eventType)": "string",
      "[failureReason](API_SolutionVersion.md#personalize-Type-SolutionVersion-failureReason)": "string",
      "[lastUpdatedDateTime](API_SolutionVersion.md#personalize-Type-SolutionVersion-lastUpdatedDateTime)": number,
      "[performAutoML](API_SolutionVersion.md#personalize-Type-SolutionVersion-performAutoML)": boolean,
      "[performHPO](API_SolutionVersion.md#personalize-Type-SolutionVersion-performHPO)": boolean,
      "[recipeArn](API_SolutionVersion.md#personalize-Type-SolutionVersion-recipeArn)": "string",
      "[solutionArn](API_SolutionVersion.md#personalize-Type-SolutionVersion-solutionArn)": "string",
      "[solutionConfig](API_SolutionVersion.md#personalize-Type-SolutionVersion-solutionConfig)": { 
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
      "[solutionVersionArn](API_SolutionVersion.md#personalize-Type-SolutionVersion-solutionVersionArn)": "string",
      "[status](API_SolutionVersion.md#personalize-Type-SolutionVersion-status)": "string",
      "[trainingHours](API_SolutionVersion.md#personalize-Type-SolutionVersion-trainingHours)": number,
      "[trainingMode](API_SolutionVersion.md#personalize-Type-SolutionVersion-trainingMode)": "string",
      "[tunedHPOParams](API_SolutionVersion.md#personalize-Type-SolutionVersion-tunedHPOParams)": { 
         "[algorithmHyperParameters](API_TunedHPOParams.md#personalize-Type-TunedHPOParams-algorithmHyperParameters)": { 
            "string" : "string" 
         }
      }
   }
}
```

## Response Elements<a name="API_DescribeSolutionVersion_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [solutionVersion](#API_DescribeSolutionVersion_ResponseSyntax) **   <a name="personalize-DescribeSolutionVersion-response-solutionVersion"></a>
The solution version\.  
Type: [SolutionVersion](API_SolutionVersion.md) object

## Errors<a name="API_DescribeSolutionVersion_Errors"></a>

 **InvalidInputException**   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeSolutionVersion_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/DescribeSolutionVersion) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/DescribeSolutionVersion) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DescribeSolutionVersion) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DescribeSolutionVersion) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/personalize-2018-05-22/DescribeSolutionVersion) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/DescribeSolutionVersion) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/DescribeSolutionVersion) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/DescribeSolutionVersion) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/DescribeSolutionVersion) 