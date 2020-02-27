# DescribeAlgorithm<a name="API_DescribeAlgorithm"></a>

Describes the given algorithm\.

## Request Syntax<a name="API_DescribeAlgorithm_RequestSyntax"></a>

```
{
   "[algorithmArn](#personalize-DescribeAlgorithm-request-algorithmArn)": "string"
}
```

## Request Parameters<a name="API_DescribeAlgorithm_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** [algorithmArn](#API_DescribeAlgorithm_RequestSyntax) **   <a name="personalize-DescribeAlgorithm-request-algorithmArn"></a>
The Amazon Resource Name \(ARN\) of the algorithm to describe\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:([a-z\d-]+):personalize:.*:.*:.+`   
Required: Yes

## Response Syntax<a name="API_DescribeAlgorithm_ResponseSyntax"></a>

```
{
   "[algorithm](#personalize-DescribeAlgorithm-response-algorithm)": { 
      "[algorithmArn](API_Algorithm.md#personalize-Type-Algorithm-algorithmArn)": "string",
      "[algorithmImage](API_Algorithm.md#personalize-Type-Algorithm-algorithmImage)": { 
         "[dockerURI](API_AlgorithmImage.md#personalize-Type-AlgorithmImage-dockerURI)": "string",
         "[name](API_AlgorithmImage.md#personalize-Type-AlgorithmImage-name)": "string"
      },
      "[creationDateTime](API_Algorithm.md#personalize-Type-Algorithm-creationDateTime)": number,
      "[defaultHyperParameterRanges](API_Algorithm.md#personalize-Type-Algorithm-defaultHyperParameterRanges)": { 
         "[categoricalHyperParameterRanges](API_DefaultHyperParameterRanges.md#personalize-Type-DefaultHyperParameterRanges-categoricalHyperParameterRanges)": [ 
            { 
               "[isTunable](API_DefaultCategoricalHyperParameterRange.md#personalize-Type-DefaultCategoricalHyperParameterRange-isTunable)": boolean,
               "[name](API_DefaultCategoricalHyperParameterRange.md#personalize-Type-DefaultCategoricalHyperParameterRange-name)": "string",
               "[values](API_DefaultCategoricalHyperParameterRange.md#personalize-Type-DefaultCategoricalHyperParameterRange-values)": [ "string" ]
            }
         ],
         "[continuousHyperParameterRanges](API_DefaultHyperParameterRanges.md#personalize-Type-DefaultHyperParameterRanges-continuousHyperParameterRanges)": [ 
            { 
               "[isTunable](API_DefaultContinuousHyperParameterRange.md#personalize-Type-DefaultContinuousHyperParameterRange-isTunable)": boolean,
               "[maxValue](API_DefaultContinuousHyperParameterRange.md#personalize-Type-DefaultContinuousHyperParameterRange-maxValue)": number,
               "[minValue](API_DefaultContinuousHyperParameterRange.md#personalize-Type-DefaultContinuousHyperParameterRange-minValue)": number,
               "[name](API_DefaultContinuousHyperParameterRange.md#personalize-Type-DefaultContinuousHyperParameterRange-name)": "string"
            }
         ],
         "[integerHyperParameterRanges](API_DefaultHyperParameterRanges.md#personalize-Type-DefaultHyperParameterRanges-integerHyperParameterRanges)": [ 
            { 
               "[isTunable](API_DefaultIntegerHyperParameterRange.md#personalize-Type-DefaultIntegerHyperParameterRange-isTunable)": boolean,
               "[maxValue](API_DefaultIntegerHyperParameterRange.md#personalize-Type-DefaultIntegerHyperParameterRange-maxValue)": number,
               "[minValue](API_DefaultIntegerHyperParameterRange.md#personalize-Type-DefaultIntegerHyperParameterRange-minValue)": number,
               "[name](API_DefaultIntegerHyperParameterRange.md#personalize-Type-DefaultIntegerHyperParameterRange-name)": "string"
            }
         ]
      },
      "[defaultHyperParameters](API_Algorithm.md#personalize-Type-Algorithm-defaultHyperParameters)": { 
         "string" : "string" 
      },
      "[defaultResourceConfig](API_Algorithm.md#personalize-Type-Algorithm-defaultResourceConfig)": { 
         "string" : "string" 
      },
      "[lastUpdatedDateTime](API_Algorithm.md#personalize-Type-Algorithm-lastUpdatedDateTime)": number,
      "[name](API_Algorithm.md#personalize-Type-Algorithm-name)": "string",
      "[roleArn](API_Algorithm.md#personalize-Type-Algorithm-roleArn)": "string",
      "[trainingInputMode](API_Algorithm.md#personalize-Type-Algorithm-trainingInputMode)": "string"
   }
}
```

## Response Elements<a name="API_DescribeAlgorithm_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [algorithm](#API_DescribeAlgorithm_ResponseSyntax) **   <a name="personalize-DescribeAlgorithm-response-algorithm"></a>
A listing of the properties of the algorithm\.  
Type: [Algorithm](API_Algorithm.md) object

## Errors<a name="API_DescribeAlgorithm_Errors"></a>

 **InvalidInputException**   
Provide a valid value for the field or parameter\.  
HTTP Status Code: 400

 **ResourceNotFoundException**   
Could not find the specified resource\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeAlgorithm_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/personalize-2018-05-22/DescribeAlgorithm) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/personalize-2018-05-22/DescribeAlgorithm) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-2018-05-22/DescribeAlgorithm) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-2018-05-22/DescribeAlgorithm) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/personalize-2018-05-22/DescribeAlgorithm) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/personalize-2018-05-22/DescribeAlgorithm) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/personalize-2018-05-22/DescribeAlgorithm) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/personalize-2018-05-22/DescribeAlgorithm) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-2018-05-22/DescribeAlgorithm) 