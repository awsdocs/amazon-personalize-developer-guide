# Creating recommenders \(AWS SDKs\)<a name="creating-recommenders-sdk"></a>

Create a recommender for a domain use case with the following code\. Run this code for each of your domain use cases\. The available use cases depend on your domain\. Some use cases require specific event types\. For a list of use cases, their ARNs, and their required event types see [Choosing recommender use cases](domain-use-cases.md)\. 

------
#### [ SDK for Python \(Boto3\) ]

Give your recommender a name and provide your Domain dataset group's Amazon Resource Name \(ARN\)\. For `recipeArn`, provide the ARN for your use case\. 

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

------
#### [ SDK for Java 2\.x ]

Use the following code to create a recommender with the SDK for Java 2\.x Run this code to create recommenders for each of your domain use cases\. Pass as parameters the following: an Amazon Personalize service client, a name for the recommender, your Domain dataset group's ARN \(datasetGroupArn\), and the ARN for your use case \(recipeArn\)\.

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

------