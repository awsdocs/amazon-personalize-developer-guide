# Creating recommenders \(AWS SDKs\)<a name="creating-recommenders-sdk"></a>

Create a recommender for a domain use case with the following code\. Give your recommender a name and provide your Domain dataset group's Amazon Resource Name \(ARN\)\. For `recipeArn`, provide the ARN for your use case\. Run this code for each of your domain use cases\. The available use cases depend on your domain\. For a list of use cases, their ARNs, and their requirements, see [Choosing recommender use cases](domain-use-cases.md)\. 

For `Top picks for your` or `Recommended for you` use cases, Amazon Personalize uses exploration when recommending items\. For more information, see [Configuring exploration](#domain-config-explore-sdk)\.

------
#### [ SDK for Python \(Boto3\) ]

```
import boto3

personalize = boto3.client('personalize')

create_recommender_response = personalize.create_recommender(
  name = 'recommender name',
  recipeArn = 'recipe ARN',
  datasetGroupArn = 'dataset group ARN'     
)

recommender_arn = create_recommender_response['recommenderArn']

print('Recommender ARN:' + recommender_arn)
```

------
#### [ SDK for Java 2\.x ]

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
#### [ SDK for JavaScript v3 ]

```
// Get service clients module and commands using ES6 syntax.
import { CreateRecommenderCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the recommender's parameters.
export const createRecommenderParam = {
  name: 'NAME', /* required */
  recipeArn: 'RECIPE_ARN', /* required */
  datasetGroupArn: 'DATASET_GROUP_ARN'  /* required */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateRecommenderCommand(createRecommenderParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

------

## Configuring exploration<a name="domain-config-explore-sdk"></a>

For `Top picks for your` or `Recommended for you` use cases, Amazon Personalize uses exploration when recommending items\. Exploration involves testing different item recommendations to learn how users respond to items with very little interaction data\. You can configure exploration with the following: 
+ Emphasis on exploring less relevant items \(for APIs, this is called explorationWeight in the [RecommenderConfig](API_RecommenderConfig.md)\): Configure how much to explore\. Specify a decimal value between 0 to 1\. The default is 0\.3\. The closer the value is to 1, the more exploration\. With more exploration, recommendations include more items with less interactions data or relevance\. At zero, no exploration occurs and recommendations are based on current data \(relevance\)\.
+ Exploration item age cutoff: Specify the maximum item age in days since the latest interaction\. This defines the scope of item exploration\. For example, if you enter 10, Amazon Personalize exploration includes only items with interactions data from the 10 days since the latest interaction in the dataset\.

  To increase the items Amazon Personalize considers during exploration, enter a greater value\. The minimum is 1 day and the default is 30 days\. Recommendations might include items without interactions data from outside this time frame\. This is because these items are relevant to the user and exploration didn't identify them\.

The following code shows how to configure exploration when you create a recommender\. The example uses the default values\.

```
import boto3

personalize = boto3.client('personalize')

create_recommender_response = personalize.create_recommender(
  name = 'recommender name',
  recipeArn = 'arn:aws:personalize:::recipe/aws-vod-top-picks',
  datasetGroupArn = 'dataset group ARN',
  recommenderConfig = {"itemExplorationConfig": {"explorationWeight": "0.3", "explorationItemAgeCutOff": "30"}}
)

recommender_arn = create_recommender_response['recommenderArn']

print('Recommender ARN:' + recommender_arn)
```