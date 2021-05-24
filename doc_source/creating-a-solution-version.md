# Step 3: Creating a solution version<a name="creating-a-solution-version"></a>

After you have completed [Step 1: Choosing a recipe](working-with-predefined-recipes.md) and [Step 2: Configuring a solution](customizing-solution-config.md), you are ready to create a solution version\. 

 A *solution version* refers to a trained machine learning model you can deploy to get recommendations for customers\. You create a solution version using the console, AWS Command Line Interface \(AWS CLI\), or AWS SDK\. If your solution version has a status of CREATE\_PENDING or CREATE\_IN\_PROGRESS, you can use the [StopSolutionVersionCreation](API_StopSolutionVersionCreation.md) operation to stop the solution version creation process\. See [Stopping the creation of a solution version](stop-solution-version.md)\. 

**Topics**
+ [Creating a solution version \(console\)](#create-solution-version-console)
+ [Creating a solution version \(AWS CLI\)](#create-solution-version-cli)
+ [Creating a solution version \(AWS Python SDK\)](#create-solution-version-sdk)
+ [Stopping the creation of a solution version](stop-solution-version.md)

## Creating a solution version \(console\)<a name="create-solution-version-console"></a>

If you just completed [Step 2: Configuring a solution](customizing-solution-config.md) and the **Create solution version** is displayed, choose **FINISH** to create a solution version\.

On the solution details page, you can track training progress in the **Solution versions** section\. When training is complete, the status is **Active** and you are ready to deploy a campaign and get recommendations\. See step 3 in the [Getting started \(console\)](getting-started-console.md) tutorial\.

If you navigated away from the **Create solution version** page or want to create an additional solution version for an existing solution, create a new solution version from the solution overview page as follows\.

**To create a new solution version**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\.

1. Navigate to the dataset groups page and choose the dataset group with your new solution\.

1. In the navigation pane, choose **Solutions and recipes**\. 

1. On the **Solution and recipes** page, choose the solution you want to create a solution version for\.

1. On the solution overview page, choose **Create solution version** to start training a new model\.

On the solution details page, you can track training progress in the **Solution versions** section\. When training is complete, the status is **Active** you can evaluate it using metrics supplied by Amazon Personalize\. For more information, see [Step 4: Evaluating a solution version with metrics](working-with-training-metrics.md)\.

If training does not complete because of an error, you are not charged for the training\. If your solution version has a status of CREATE\_PENDING or CREATE\_IN\_PROGRESS, you can stop the solution version creation process\. To stop solution version creation, navigate to the solution version details page and choose **Stop**\. See [Stopping the creation of a solution version](stop-solution-version.md)\.

## Creating a solution version \(AWS CLI\)<a name="create-solution-version-cli"></a>

When your solution is ACTIVE, train the model by running the following command\. Replace `solution arn` with the solution Amazon Resource Name \(ARN\) from [Step 2: Configuring a solution](customizing-solution-config.md)\.

```
aws personalize create-solution-version \
  --solution-arn solution arn
```

The solution version ARN is displayed, for example:

```
{
  "solutionVersionArn": "arn:aws:personalize:us-west-2:acct-id:solution/SolutionName/<version-id>"
}
```

Check the training status of the solution version by using the `describe-solution-version` command\. Provide the solution version ARN that was returned in the previous step\. For more information about the API, see [DescribeSolutionVersion](API_DescribeSolutionVersion.md)\.

```
aws personalize describe-solution-version \
  --solution-version-arn solution version arn
```

The properties of the solution version and the training `status` are displayed\. Initially, the status shows as CREATE PENDING, for example:

```
{
  "solutionVersion": {
      "solutionVersionArn": "arn:aws:personalize:us-west-2:acct-id:solution/solutionName/<version-id>",
      ...,
      "status": "CREATE PENDING"
  }
}
```

Training is complete when the `status` is `ACTIVE` and you can evaluate it using metrics supplied by Amazon Personalize\. For more information, see [Step 4: Evaluating a solution version with metrics](working-with-training-metrics.md)\. If training does not complete because of an error, you are not charged for the training\. 

If your solution version has a status of CREATE\_PENDING or CREATE\_IN\_PROGRESS, you can use the [StopSolutionVersionCreation](API_StopSolutionVersionCreation.md) operation to stop the solution version creation process\. See [Stopping the creation of a solution version](stop-solution-version.md)\.

## Creating a solution version \(AWS Python SDK\)<a name="create-solution-version-sdk"></a>

When your solution is ACTIVE, use the following code to create a solution version with the AWS SDK for Python \(Boto3\) or the AWS SDK for Java 2\.x\.

------
#### [ SDK for Python \(Boto3\) ]

To create a solution version, using the following `create_solution_version` method\. Replace the `solution arn` with the Amazon Resource Name \(ARN\) of the solution from [Step 2: Configuring a solution](customizing-solution-config.md)\. The following code uses the [DescribeSolutionVersion](API_DescribeSolutionVersion.md) operation to retrieve the solution version's status\.

```
import boto3

personalize = boto3.client('personalize')
# Store the solution ARN
solution_arn = 'solution arn'
        
# Use the solution ARN to get the solution status.
solution_description = personalize.describe_solution(solutionArn = 'solution_arn')['solution']
print('Solution status: ' + solution_description['status'])

# Use the solution ARN to create a solution version.
print ('Creating solution version')
response = personalize.create_solution_version(solutionArn = solution_arn)
solution_version_arn = response['solutionVersionArn']
print('Solution version ARN: ' + solution_version_arn)

# Use the solution version ARN to get the solution version status.
solution_version_description = personalize.describe_solution_version(
    solutionVersionArn = solution_version_arn)['solutionVersion']
print('Solution version status: ' + solution_version_description['status'])
```

------
#### [ SDK for Java 2\.x ]

To create a solution version, use the following `createPersonalizeSolutionVersion` method and pass as a parameter the Amazon Resource Name \(ARN\) of the solution from [Step 2: Configuring a solution](customizing-solution-config.md)\. The following code uses the [DescribeSolutionVersion](API_DescribeSolutionVersion.md) operation to retrieve the solution version's status\. 

```
public static String createPersonalizeSolutionVersion(PersonalizeClient personalizeClient, String solutionArn) {
        long maxTime = 0;
        long waitInMilliseconds = 30 * 1000; // 30 seconds
        String solutionStatus = "";
        String solutionVersionStatus = "";
        String solutionVersionArn = "";

        try {
            DescribeSolutionRequest describeSolutionRequest = DescribeSolutionRequest.builder()
                .solutionArn(solutionArn)
                .build();
            
            maxTime = Instant.now().getEpochSecond() + 3 * 60 * 60;

            // Wait until solution is active. 
            while (Instant.now().getEpochSecond() < maxTime) {

                solutionStatus = personalizeClient.describeSolution(describeSolutionRequest).solution().status();
                System.out.println("Solution status: " + solutionStatus);

                if (solutionStatus.equals("ACTIVE") || solutionStatus.equals("CREATE FAILED")) {
                    break;
                }
                try {
                    Thread.sleep(waitInMilliseconds);
                } catch (InterruptedException e) {
                    System.out.println(e.getMessage());
                }
            }
            
            // Once the solution is active, start creating a solution version.
            
            if (solutionStatus.equals("ACTIVE")) {

                CreateSolutionVersionRequest createSolutionVersionRequest = CreateSolutionVersionRequest.builder()
                    .solutionArn(solutionArn)
                    .build();
                
                CreateSolutionVersionResponse createSolutionVersionResponse = personalizeClient.createSolutionVersion(createSolutionVersionRequest);
                solutionVersionArn = createSolutionVersionResponse.solutionVersionArn();

                System.out.println("Solution version ARN: " + solutionVersionArn);

                DescribeSolutionVersionRequest describeSolutionVersionRequest = DescribeSolutionVersionRequest.builder() 
                    .solutionVersionArn(solutionVersionArn)
                    .build();
                
                maxTime = Instant.now().getEpochSecond() + 3 * 60 * 60;
                
                while (Instant.now().getEpochSecond() < maxTime) {

                    // Use the solution version ARN to get the solution version status.
                    solutionVersionStatus = personalizeClient.describeSolutionVersion(describeSolutionVersionRequest).solutionVersion().status();
                    System.out.println("Solution version status: " + solutionVersionStatus);
    
                    if (solutionVersionStatus.equals("ACTIVE") || solutionVersionStatus.equals("CREATE FAILED")) {
                        break;
                    }
                    try {
                        Thread.sleep(waitInMilliseconds);
                    } catch (InterruptedException e) {
                        System.out.println(e.getMessage());
                    }
                }
                return solutionVersionArn;
            }
        } catch(PersonalizeException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        return "";
    }
```

------



To check the current solution version status, call the [DescribeSolutionVersion](API_DescribeSolutionVersion.md) operation and pass the ARN of the solution version returned from the `CreateSolutionVersion` operation\. Training is complete when the `status` is `ACTIVE` and you can evaluate it using metrics supplied by Amazon Personalize\. For more information, see [Step 4: Evaluating a solution version with metrics](working-with-training-metrics.md)\. If training does not complete because of an error, you are not charged for the training\. 

If your solution version has a status of CREATE\_PENDING or CREATE\_IN\_PROGRESS, you can use the [StopSolutionVersionCreation](API_StopSolutionVersionCreation.md) operation to stop the solution version creation process\. See [Stopping the creation of a solution version](stop-solution-version.md)\.