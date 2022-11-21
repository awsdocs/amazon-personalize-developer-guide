# Getting started \(SDK for Java 2\.x\)<a name="getting-started-java"></a>

This tutorial shows you how to complete the Amazon Personalize workflow from start to finish with the AWS SDK for Java 2\.x\. 

To avoid incurring unnecessary charges, when you finish the getting started exercise follow the steps in [Cleaning up resources](gs-cleanup.md) to delete the resources you create in the tutorial\. 

For more examples, see [Complete Amazon Personalize project](#gs-java-example)\.

**Topics**
+ [Prerequisites](#gs-java-prerequisites)
+ [Complete Amazon Personalize project](#gs-java-example)

## Prerequisites<a name="gs-java-prerequisites"></a>

The following are prerequisite steps for completing this tutorial:
+ Complete the [Getting started prerequisites](gs-prerequisites.md), to set up the required permissions and create the training data\. You can use the same source data used in the [Getting started \(console\)](getting-started-console.md) or [Getting started \(AWS CLI\)](getting-started-cli.md) exercises\. If you are using your own source data, make sure that your data is formatted like in the prerequisites\.
+ Set up your SDK for Java 2\.x environment and AWS credentials as specified in the [Setting up the AWS SDK for Java 2\.x](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/setup.html) procedure in the *AWS SDK for Java 2\.x Developer Guide*\. 

### Tutorial<a name="gs-custom-java-tutorial"></a>

In the following steps you set up your project to use Amazon Personalize packages and create Amazon Personalize SDK for Java 2\.x clients\. Then you import data, create and deploy a solution version with a campaign, and get recommendations\.

#### Step 1: Set up your project to use Amazon Personalize packages<a name="gs-java-set-up-project"></a>

After you complete the prerequisites, add Amazon Personalize dependencies to your pom\.xml file and import Amazon Personalize packages\. 

1.  Add the following dependencies to your pom\.xml file\. The latest version numbers may be different than the example code\. 

   ```
   <dependency>
   	<groupId>software.amazon.awssdk</groupId>
   	<artifactId>personalize</artifactId>
   	<version>2.16.83</version>
   </dependency>
   <dependency>
   	<groupId>software.amazon.awssdk</groupId>
   	<artifactId>personalizeruntime</artifactId>
   	<version>2.16.83</version>
   </dependency>
   <dependency>
   	<groupId>software.amazon.awssdk</groupId>
   	<artifactId>personalizeevents</artifactId>
   	<version>2.16.83</version>
   </dependency>
   ```

1.  Add the following import statements to your project\. 

   ```
   // import client packages
   import software.amazon.awssdk.services.personalize.PersonalizeClient;
   import software.amazon.awssdk.services.personalizeruntime.PersonalizeRuntimeClient;
   // Amazon Personalize exception package
   import software.amazon.awssdk.services.personalize.model.PersonalizeException;
   // schema packages
   import software.amazon.awssdk.services.personalize.model.CreateSchemaRequest;
   // dataset group packages
   import software.amazon.awssdk.services.personalize.model.CreateDatasetGroupRequest;
   import software.amazon.awssdk.services.personalize.model.DescribeDatasetGroupRequest;
   // dataset packages
   import software.amazon.awssdk.services.personalize.model.CreateDatasetRequest;
   // dataset import job packages
   import software.amazon.awssdk.services.personalize.model.CreateDatasetImportJobRequest;
   import software.amazon.awssdk.services.personalize.model.DataSource;
   import software.amazon.awssdk.services.personalize.model.DatasetImportJob;
   import software.amazon.awssdk.services.personalize.model.DescribeDatasetImportJobRequest;
   // solution packages
   import software.amazon.awssdk.services.personalize.model.CreateSolutionRequest;
   import software.amazon.awssdk.services.personalize.model.CreateSolutionResponse;
   // solution version packages
   import software.amazon.awssdk.services.personalize.model.DescribeSolutionRequest;
   import software.amazon.awssdk.services.personalize.model.CreateSolutionVersionRequest;
   import software.amazon.awssdk.services.personalize.model.CreateSolutionVersionResponse;
   import software.amazon.awssdk.services.personalize.model.DescribeSolutionVersionRequest;
   // campaign packages
   import software.amazon.awssdk.services.personalize.model.CreateCampaignRequest;
   import software.amazon.awssdk.services.personalize.model.CreateCampaignResponse;
   // get recommendations packages
   import software.amazon.awssdk.services.personalizeruntime.model.GetRecommendationsRequest;
   import software.amazon.awssdk.services.personalizeruntime.model.GetRecommendationsResponse;
   import software.amazon.awssdk.services.personalizeruntime.model.PredictedItem;
   // Java time utility package
   import java.time.Instant;
   ```

#### Step 2: Create Amazon Personalize clients<a name="getting-started-java-clients"></a>

After you add Amazon Personalize dependencies to your pom\.xml file and import the required packages, create the following Amazon Personalize clients:

```
PersonalizeClient personalizeClient = PersonalizeClient.builder()
  .region(region)
  .build();

PersonalizeRuntimeClient personalizeRuntimeClient = PersonalizeRuntimeClient.builder() 
  .region(region)
  .build();
```

#### Step 3: Import data<a name="getting-started-java-import-dataset"></a>

After you initialize your Amazon Personalize clients, import the historical data you created when you completed the [Getting started prerequisites](gs-prerequisites.md)\. To import historical data into Amazon Personalize, do the following:

1.  Save the following Avro schema as a JSON file in your working directory\. This schema matches the columns in the CSV file you created when you completed the [Getting started prerequisites](gs-prerequisites.md)\. 

   ```
   {
     "type": "record",
     "name": "Interactions",
     "namespace": "com.amazonaws.personalize.schema",
     "fields": [
         {
             "name": "USER_ID",
             "type": "string"
         },
         {
             "name": "ITEM_ID",
             "type": "string"
         },
         {
             "name": "TIMESTAMP",
             "type": "long"
         }
     ],
     "version": "1.0"
   }
   ```

1. Use the following `createSchema` method to create a schema in Amazon Personalize\. Pass the following as parameters: an Amazon Personalize service client, the name for your schema, and the file path for the schema JSON file you created in the previous step\. The method returns the Amazon Resource Name \(ARN\) of your new schema\. Store it for later use\. 

   ```
       public static String createSchema(PersonalizeClient personalizeClient, String schemaName, String filePath) {
   
           String schema = null;
           try {
               schema = new String(Files.readAllBytes(Paths.get(filePath)));
           } catch (IOException e) {
               System.out.println(e.getMessage());
           }
   
           try {
               CreateSchemaRequest createSchemaRequest = CreateSchemaRequest.builder()
                       .name(schemaName)
                       .schema(schema)
                       .build();
   
               String schemaArn = personalizeClient.createSchema(createSchemaRequest).schemaArn();
   
               System.out.println("Schema arn: " + schemaArn);
   
               return schemaArn;
   
           } catch (PersonalizeException e) {
               System.err.println(e.awsErrorDetails().errorMessage());
               System.exit(1);
           }
           return "";
       }
   ```

1. Create a dataset group\. Use the following `createDatasetGroup` method to create a dataset group\. Pass the following as parameters: an Amazon Personalize service client and the name for the dataset group\. The method returns the ARN of your new dataset group\. Store it for later use\.

   ```
       public static String createDatasetGroup(PersonalizeClient personalizeClient, String datasetGroupName) {
   
           try {
               CreateDatasetGroupRequest createDatasetGroupRequest = CreateDatasetGroupRequest.builder()
                       .name(datasetGroupName)
                       .build();
               return personalizeClient.createDatasetGroup(createDatasetGroupRequest).datasetGroupArn();
           } catch (PersonalizeException e) {
               System.out.println(e.awsErrorDetails().errorMessage());
           }
           return "";
       }
   ```

1. Create an Interactions dataset\. Use the following `createDataset` method to create an Interactions dataset\. Pass the following as parameters: an Amazon Personalize service client, the name for your dataset, your schema's ARN, your dataset group's ARN, and `Interactions` for the dataset type\. The method returns the ARN of your new dataset\. Store it for later use\.

   ```
       public static String createDataset(PersonalizeClient personalizeClient,
                                          String datasetName,
                                          String datasetGroupArn,
                                          String datasetType,
                                          String schemaArn) {
           try {
               CreateDatasetRequest request = CreateDatasetRequest.builder()
                   .name(datasetName)
                   .datasetGroupArn(datasetGroupArn)
                   .datasetType(datasetType)
                   .schemaArn(schemaArn)
                   .build();
   
               String datasetArn = personalizeClient.createDataset(request)
                   .datasetArn();
               System.out.println("Dataset " + datasetName + " created.");
               return datasetArn;
   
           } catch (PersonalizeException e) {
               System.err.println(e.awsErrorDetails().errorMessage());
               System.exit(1);
           }
           return "";
       }
   ```

1. Import your data with a dataset import job\. Use the following `createPersonalizeDatasetImportJob` method to create a dataset import job\. 

   Pass the following as parameters: an Amazon Personalize service client, a name for the job, your Interactions dataset's ARN, the Amazon S3 bucket path \(`s3://bucket name/folder name/ratings.csv`\) where you stored the training data, and your service role's ARN \(you created this role as part of the [Getting started prerequisites](gs-prerequisites.md)\)\. The method returns the ARN of your dataset import job\. Optionally store it for later use\. 

   ```
       public static String createPersonalizeDatasetImportJob(PersonalizeClient personalizeClient,
                                                              String jobName,
                                                              String datasetArn,
                                                              String s3BucketPath,
                                                              String roleArn) {
   
           long waitInMilliseconds = 60 * 1000;
           String status;
           String datasetImportJobArn;
   
           try {
               DataSource importDataSource = DataSource.builder()
                       .dataLocation(s3BucketPath)
                       .build();
   
               CreateDatasetImportJobRequest createDatasetImportJobRequest = CreateDatasetImportJobRequest.builder()
                       .datasetArn(datasetArn)
                       .dataSource(importDataSource)
                       .jobName(jobName)
                       .roleArn(roleArn)
                       .build();
   
               datasetImportJobArn = personalizeClient.createDatasetImportJob(createDatasetImportJobRequest)
                       .datasetImportJobArn();
               DescribeDatasetImportJobRequest describeDatasetImportJobRequest = DescribeDatasetImportJobRequest.builder()
                       .datasetImportJobArn(datasetImportJobArn)
                       .build();
   
               long maxTime = Instant.now().getEpochSecond() + 3 * 60 * 60;
   
               while (Instant.now().getEpochSecond() < maxTime) {
   
                   DatasetImportJob datasetImportJob = personalizeClient
                           .describeDatasetImportJob(describeDatasetImportJobRequest)
                           .datasetImportJob();
   
                   status = datasetImportJob.status();
                   System.out.println("Dataset import job status: " + status);
   
                   if (status.equals("ACTIVE") || status.equals("CREATE FAILED")) {
                       break;
                   }
                   try {
                       Thread.sleep(waitInMilliseconds);
                   } catch (InterruptedException e) {
                       System.out.println(e.getMessage());
                   }
               }
               return datasetImportJobArn;
   
           } catch (PersonalizeException e) {
               System.out.println(e.awsErrorDetails().errorMessage());
           }
           return "";
       }
   ```

#### Step 4: Create a solution<a name="getting-started-java-create-solution"></a>

After you import your data, you create a solution and solution version as follows\. The *solution* contains the configurations to train a model and a *solution version* is a trained model\. 

1. Create a new solution with the following `createPersonalizeSolution` method\. Pass the following as parameters: an Amazon Personalize service client, your dataset groups Amazon Resource Name \(ARN\), a name for the solution, and the ARN for the User\-Personalization recipe \(`arn:aws:personalize:::recipe/aws-user-personalization`\)\. The method returns the ARN your new solution\. Store it for later use\. 

   ```
       public static String createPersonalizeSolution(PersonalizeClient personalizeClient,
                                                    String datasetGroupArn,
                                                    String solutionName,
                                                    String recipeArn) {
   
           try {
               CreateSolutionRequest solutionRequest = CreateSolutionRequest.builder()
                   .name(solutionName)
                   .datasetGroupArn(datasetGroupArn)
                   .recipeArn(recipeArn)
                   .build();
   
               CreateSolutionResponse solutionResponse = personalizeClient.createSolution(solutionRequest);
               return solutionResponse.solutionArn();
   
           } catch (PersonalizeException e) {
               System.err.println(e.awsErrorDetails().errorMessage());
               System.exit(1);
           }
           return "";
       }
   ```

1. Create a solution version with the following `createPersonalizeSolutionVersion` method\. Pass as a parameter the ARN of the solution the previous step\. The following code first checks to see if your solution is ready and then creates a solution version\. During training, the code uses the [DescribeSolutionVersion](API_DescribeSolutionVersion.md) operation to retrieve the solution version's status\. When training is complete, the method returns the ARN of your new solution version\. Store it for later use\. 

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
   
                   while (Instant.now().getEpochSecond() < maxTime) {
   
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

For more information, see [Creating a solution](training-deploying-solutions.md)\. When you create a solution version, you can evaluate its performance before proceeding\. For more information, see [Step 4: Evaluating a solution version with metrics](working-with-training-metrics.md)\.

#### Step 5: Create a campaign<a name="getting-started-java-deploy-solution"></a>

After you train and evaluate your solution version, deploy it with an Amazon Personalize campaign\. Use the following `createPersonalCampaign` method to deploy a solution version\. Pass the following as parameters: an Amazon Personalize service client, the Amazon Resource Name \(ARN\) of the solution version you created in the previous step, and a name for the campaign\. The method returns the ARN of your new campaign\. Store it for later use\.

```
public static String createPersonalCompaign(PersonalizeClient personalizeClient, String solutionVersionArn, String name) {

    try {
        CreateCampaignRequest createCampaignRequest = CreateCampaignRequest.builder()
            .minProvisionedTPS(1)
            .solutionVersionArn(solutionVersionArn)
            .name(name)
            .build();

        CreateCampaignResponse campaignResponse = personalizeClient.createCampaign(createCampaignRequest);
        System.out.println("The campaign ARN is "+campaignResponse.campaignArn());
        return campaignResponse.campaignArn();

    } catch (PersonalizeException e) {
        System.err.println(e.awsErrorDetails().errorMessage());
        System.exit(1);
    }
}
```

For more information about Amazon Personalize campaigns, see [Creating a campaign](campaigns.md)\.

#### Step 6: Get recommendations<a name="getting-started-java-get-recommendations"></a>

After you create a campaign, you use it to get recommendations\. Use the following `getRecs` method to get recommendations for a user\. Pass as parameters an Amazon Personalize runtime client, the Amazon Resource Name \(ARN\) of the campaign you created in the previous step, and a user ID \(for example, `123`\) from the historical data you imported\. The method prints the list of recommended items to the screen\.

```
    public static void getRecs(PersonalizeRuntimeClient personalizeRuntimeClient, String campaignArn, String userId){

        try {
            GetRecommendationsRequest recommendationsRequest = GetRecommendationsRequest.builder()
                .campaignArn(campaignArn)
                .numResults(20)
                .userId(userId)
                .build();

            GetRecommendationsResponse recommendationsResponse = personalizeRuntimeClient.getRecommendations(recommendationsRequest);
            List<PredictedItem> items = recommendationsResponse.itemList();
            for (PredictedItem item: items) {
                System.out.println("Item Id is : "+item.itemId());
                System.out.println("Item score is : "+item.score());
            }

        } catch (AwsServiceException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```

## Complete Amazon Personalize project<a name="gs-java-example"></a>

For an all\-in\-one project that shows you how to complete the Amazon Personalize workflow with the SDK for Java 2\.x, see the [Amazon\-Personalize\-Java\-App](https://github.com/seashman/Amazon-Personalize-Java-App) on GitHub\. This project includes training multiple solution versions with different recipes, and recording events with the PutEvents operation\.

 For additional examples, see code the found in the [personalize](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javav2/example_code/personalize/src/main/java/com/example/personalize) folder of the AWS SDK examples repository\. 