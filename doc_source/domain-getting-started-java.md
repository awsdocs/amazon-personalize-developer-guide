# Getting started with a Domain dataset group \(SDK for Java 2\.x\)<a name="domain-getting-started-java"></a>

This tutorial shows you how to use the SDK for Java 2\.x to create a Domain dataset group for the VIDEO\_ON\_DEMAND domain\. In this tutorial, you create a recommender for the *Top picks for you* use case\.

To avoid incurring unnecessary charges, when you finish the getting started exercise see [Cleaning up resources](gs-cleanup.md) for information on deleting the resources you create in the tutorial\. 

## Prerequisites<a name="domain-gs-java-prerequisites"></a>

The following are prerequisite steps for completing this tutorial:
+ Complete the [Getting started prerequisites](gs-prerequisites.md) to set up the required permissions and create the training data\. If you also completed the [Getting started with a Domain dataset group \(console\)](getting-started-console-domain.md), you can reuse the same source data\. If you are using your own source data, make sure that your data is formatted like in the prerequisites\.
+ Set up your SDK for Java 2\.x environment and AWS credentials as specified in the [Setting up the AWS SDK for Java 2\.x](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/setup.html) procedure in the *AWS SDK for Java 2\.x Developer Guide*\. 

## Tutorial<a name="gs-java-domain-tutorial"></a>

In the following steps, you set up your project to use Amazon Personalize packages and create Amazon Personalize SDK for Java 2\.x clients\. Then you import data, create a recommender for the *Top picks for you* use case, and get recommendations\.

### Step 1: Set up your project to use Amazon Personalize packages<a name="domain-gs-java-set-up-project"></a>

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
   // recommender packages
   import software.amazon.awssdk.services.personalize.model.CreateRecommenderRequest;
   import software.amazon.awssdk.services.personalize.model.CreateRecommenderResponse;
   import software.amazon.awssdk.services.personalize.model.DescribeRecommenderRequest;
   // get recommendations packages
   import software.amazon.awssdk.services.personalizeruntime.model.GetRecommendationsRequest;
   import software.amazon.awssdk.services.personalizeruntime.model.GetRecommendationsResponse;
   import software.amazon.awssdk.services.personalizeruntime.model.PredictedItem;
   // Java time utility package
   import java.time.Instant;
   ```

### Step 2: Create Amazon Personalize clients<a name="domain-gs-java-clients"></a>

After you add Amazon Personalize dependencies to your pom\.xml file and import the required packages, create the following Amazon Personalize clients:

```
PersonalizeClient personalizeClient = PersonalizeClient.builder()
  .region(region)
  .build();

PersonalizeRuntimeClient personalizeRuntimeClient = PersonalizeRuntimeClient.builder() 
  .region(region)
  .build();
```

### Step 3: Import data<a name="domain-gs-java-import-dataset"></a>

After you initialize your Amazon Personalize clients, import the historical data you created when you completed the [Getting started prerequisites](gs-prerequisites.md)\. To import historical data into Amazon Personalize, do the following:

1.  Save the following Avro schema as a JSON file in your working directory\. This schema matches the columns in the CSV file that you created when you completed the [Creating the training data \(Domain dataset group\)](gs-prerequisites.md#gs-data-prep-domain)\. 

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
             "name": "EVENT_TYPE",
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

1. Use the following `createDomainSchema` method to create a domain schema in Amazon Personalize\. Pass the following as parameters: an Amazon Personalize service client, the name for your schema, `VIDEO_ON_DEMAND` for the domain, and the file path for the schema JSON file that you created in the previous step\. The method returns the Amazon Resource Name \(ARN\) of your new schema\. Store it for later use\. 

   ```
       public static String createDomainSchema(PersonalizeClient personalizeClient, String schemaName, String domain, String filePath) {
   
           String schema = null;
           try {
               schema = new String(Files.readAllBytes(Paths.get(filePath)));
           } catch (IOException e) {
               System.out.println(e.getMessage());
           }
   
           try {
               CreateSchemaRequest createSchemaRequest = CreateSchemaRequest.builder()
                       .name(schemaName)
                       .domain(domain)
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

1. Create a dataset group\. Use the following `createDomainDatasetGroup` method to create a domain dataset group\. Pass the following as parameters: an Amazon Personalize service client, a name for the dataset group, and pass `VIDEO_ON_DEMAND` for the domain\. The method returns the ARN of your new dataset group\. Store it for later use\.

   ```
       public static String createDomainDatasetGroup(PersonalizeClient personalizeClient,
                                                     String datasetGroupName,
                                                     String domain) {
   
           try {
               CreateDatasetGroupRequest createDatasetGroupRequest = CreateDatasetGroupRequest.builder()
                       .name(datasetGroupName)
                       .domain(domain)
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

   Pass the following as parameters: an Amazon Personalize service client, a name for the job, and your Interactions dataset's ARN\. Pass the Amazon S3 bucket path \(`s3://bucket name/folder name/ratings.csv`\) where you stored the training data, and your service role's ARN\. You created this role as part of the [Getting started prerequisites](gs-prerequisites.md)\. The method returns the ARN of your dataset import job\. Optionally store it for later use\. 

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

### Step 4: Create a recommender<a name="domain-gs-create-recommender"></a>

After your dataset import job completes, you are ready create a recommender\. Use the following `createRecommender` method to create a recommender\. Pass the following as parameters: an Amazon Personalize service client, a name for the recommender, your dataset group's Amazon Resource Name \(ARN\), and `arn:aws:personalize:::recipe/aws-vod-top-picks` for the recipe ARN\. The method returns the ARN of your new recommender\. Store it for later use\.

```
    public static String createRecommender(PersonalizeClient personalizeClient,
                                           String name,
                                           String datasetGroupArn,
                                           String recipeArn) {

        long maxTime = 0;
        long waitInMilliseconds = 30 * 1000; // 30 seconds
        String recommenderStatus = "";

        try {
                CreateRecommenderRequest createRecommenderRequest = CreateRecommenderRequest.builder()
                        .datasetGroupArn(datasetGroupArn)
                        .name(name)
                        .recipeArn(recipeArn)
                        .build();

                CreateRecommenderResponse recommenderResponse = personalizeClient.createRecommender(createRecommenderRequest);
                String recommenderArn = recommenderResponse.recommenderArn();
                System.out.println("The recommender ARN is " + recommenderArn);

                DescribeRecommenderRequest describeRecommenderRequest = DescribeRecommenderRequest.builder()
                        .recommenderArn(recommenderArn)
                        .build();

                maxTime = Instant.now().getEpochSecond() + 3 * 60 * 60;

                while (Instant.now().getEpochSecond() < maxTime) {

                    recommenderStatus = personalizeClient.describeRecommender(describeRecommenderRequest).recommender().status();
                    System.out.println("Recommender status: " + recommenderStatus);

                    if (recommenderStatus.equals("ACTIVE") || recommenderStatus.equals("CREATE FAILED")) {
                        break;
                    }
                    try {
                        Thread.sleep(waitInMilliseconds);
                    } catch (InterruptedException e) {
                        System.out.println(e.getMessage());
                    }
                }
                return recommenderArn;

        } catch(PersonalizeException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        return "";
    }
```

### Step 5: Get recommendations<a name="domain-gs-java-get-recommendations"></a>

After you create a recommender, you use it to get recommendations\. Use the following `getRecs` method to get recommendations for a user\. Pass as parameters an Amazon Personalize runtime client, the Amazon Resource Name \(ARN\) of the recommender you created in the previous step, and a user ID \(for example, `123`\)\. The method prints the list of recommended items to the screen\. 

```
    public static void getRecs(PersonalizeRuntimeClient personalizeRuntimeClient, String recommenderArn, String userId){

        try {
            GetRecommendationsRequest recommendationsRequest = GetRecommendationsRequest.builder()
                    .recommenderArn(recommenderArn)
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