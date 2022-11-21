# Creating a campaign<a name="campaigns"></a>

For real\-time recommendations, after you complete [Preparing and importing data](data-prep.md) and [Creating a solution](training-deploying-solutions.md), you are ready to deploy your solution version to generate recommendations\. You deploy a solution version by creating an Amazon Personalize campaign\. If you are getting batch recommendations, you don't need to create a campaign\. For more information see [Getting batch recommendations and user segments](recommendations-batch.md)\.

 A campaign is a deployed solution version \(trained model\) with provisioned dedicated transaction capacity for creating real\-time recommendations for your application users\. After you create a campaign, you use the [GetRecommendations](API_RS_GetRecommendations.md) or [GetPersonalizedRanking](API_RS_GetPersonalizedRanking.md) API operations to get recommendations\. 

You create a campaign with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\.

**Important**  
 If you manually retrain your solution version or want to change your campaign settings, you must update your campaign\. For more information see [Updating a campaign](update-campaigns.md)\. 

**Topics**
+ [Minimum provisioned transactions per second and auto\-scaling](#min-tps-auto-scaling)
+ [Creating a campaign \(console\)](#create-campaign-console)
+ [Creating a campaign \(AWS CLI\)](#create-campaign-cli)
+ [Creating a campaign \(AWS SDKs\)](#create-campaign-sdk)
+ [Updating a campaign](update-campaigns.md)

## Minimum provisioned transactions per second and auto\-scaling<a name="min-tps-auto-scaling"></a>

When you create an Amazon Personalize campaign, you specify a dedicated transaction capacity for creating real\-time recommendations for your application users\. A transaction is a single `GetRecommendations` or `GetPersonalizedRanking` call\. Transactions per second \(TPS\) is the throughput and unit of billing for Amazon Personalize\. The minimum provisioned TPS \(`minProvisionedTPS`\) specifies the baseline throughput provisioned by Amazon Personalize, and thus, the minimum billing charge\. 

 If your TPS increases beyond `minProvisionedTPS`, Amazon Personalize auto\-scales the provisioned capacity up and down, but never below `minProvisionedTPS`\. There's a short time delay while the capacity is increased that might cause loss of transactions\.

The actual TPS used is calculated as the average requests/second within a 5\-minute window\. You pay for maximum of the minimum provisioned TPS or the actual TPS\. We recommend starting with a low `minProvisionedTPS`, track your usage using Amazon CloudWatch metrics, and then increase the `minProvisionedTPS` as necessary\.

## Creating a campaign \(console\)<a name="create-campaign-console"></a>

After your solution version status is Active you are ready to deploy it with an Amazon Personalize campaign\.

**To create a campaign \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\.

1.  Choose the dataset group with the solution version you want to deploy\. 

1. In the navigation pane, choose **Campaigns**\.

1. On the **Campaigns** page, choose **Create campaign**\.

1. On the **Create new campaign** page, for **Campaign details**, provide the following information: 
   + **Campaign name:** Enter the name of the campaign\. The text you enter here appears on the Campaign dashboard and details page\.
   + **Solution:** Choose the solution that you just created\.
   + **Solution version ID:** Choose the ID of the solution version that you just created\.
   + **Minimum provisioned transactions per second:** Set the minimum provisioned transactions per second that Amazon Personalize supports\. For more information, see [Minimum provisioned transactions per second and auto\-scaling](#min-tps-auto-scaling)\.

1. If you used the User\-Personalization recipe, in **Campaign configuration** optionally enter values for the **Exploration weight** and **Exploration item age cut off**\. For more information see [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md)\.

1. For **Tags**, optionally add any tags\. For more information about tagging Amazon Personalize resources, see [Tagging Amazon Personalize resources](tagging-resources.md)\.

1. Choose **Create campaign**\.

1. On the campaign details page, when the campaign status is **Active**, you can use the campaign to get recommendations and record impressions\. For more information, see [Getting recommendations \(Custom dataset group\)](getting-recommendations.md)\. 

   The campaign is ready when its status is ACTIVE\. If you retrain your solution version or want to change your campaign settings, you must update your campaign\. For more information see [Updating a campaign](update-campaigns.md)\. 

## Creating a campaign \(AWS CLI\)<a name="create-campaign-cli"></a>

After your solution version status is Active, you are ready to deploy it with an Amazon Personalize campaign\. Use the following `create-campaign` AWS CLI command to create a campaign that deploys a solution version trained using the User\-Personalization recipe\. Give the campaign a name and specify the solution version ARN \(Amazon Resource Name\)\. Optionally change the `minProvisionedTPS` if your use case requires a higher provisioned capacity\. The minimum value is 1\. 

 The `campaign-config` parameters are specific to the recipe that you used to train the solution version \(for more information about recipes see [Step 1: Choosing a recipe](working-with-predefined-recipes.md)\)\. The example uses the following User\-Personalization recipe specific `itemExplorationConfig` fields with their default values: `explorationWeight` and `explorationItemAgeCutOff`\. If you omit the `campaign-config` parameter, the default values apply\. For more information about the `itemExplorationConfig` fields, see the [Properties and hyperparameters](native-recipe-new-item-USER_PERSONALIZATION.md#bandit-hyperparameters) for the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) recipe\.

```
aws personalize create-campaign \
--name campaign name \
--solution-version-arn solution version arn \
--min-provisioned-tps 1 \
--campaign-config "{\"itemExplorationConfig\":{\"explorationWeight\":\"0.3\",\"explorationItemAgeCutOff\":\"30\"}}"
```

The campaign is ready when its status is ACTIVE\. To get the current status, call [DescribeCampaign](API_DescribeCampaign.md) and check that the `status` field is `ACTIVE`\.

 If you retrain your solution version or want to change your campaign settings, you must update your campaign\. For more information see [Updating a campaign](update-campaigns.md)\. 

Amazon Personalize provides operations for managing campaigns such as [ListCampaigns](API_ListCampaigns.md) to list the campaigns you have created\. You can delete a campaign by calling [DeleteCampaign](API_DeleteCampaign.md)\. If you delete a campaign, the solution versions that are part of the campaign are not deleted\.

After you have created your campaign, use it to make recommendations\. For more information, see [Getting recommendations \(Custom dataset group\)](getting-recommendations.md)\.

## Creating a campaign \(AWS SDKs\)<a name="create-campaign-sdk"></a>

 After your solution version status is Active you are ready to deploy it with an Amazon Personalize campaign\. Use the following code to create a campaign\. Give the campaign a name, specify the Amazon Resource Name \(ARN\) of the solution version to deploy, and optionally specify the [Minimum provisioned TPS](#min-tps-auto-scaling) the campaign will support \(the default value for this parameter is 1\)\. If you use the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) recipe, you can configure item exploration with the `itemExplorationWeight` and `explorationItemAgeCutOff` parameters\.

------
#### [ SDK for Python \(Boto3\) ]

In this example, the `itemExplorationWeight` and `explorationItemAgeCutOff` parameters are specific to the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) recipe\. The default itemExplorationWeight is *0\.3* and the default explorationItemAgeCutOff is *30*\. If you leave out campaign configuration parameters, the default values apply\.

```
import boto3

personalize = boto3.client('personalize')

response = personalize.create_campaign(
    name = 'campaign name',
    solutionVersionArn = 'solution version arn',
    minProvisionedTPS = 1,
    campaignConfig = {"itemExplorationConfig": {"explorationWeight": "0.3", "explorationItemAgeCutOff": "30"}}
)

arn = response['campaignArn']

description = personalize.describe_campaign(campaignArn = arn)['campaign']
print('Name: ' + description['name'])
print('ARN: ' + description['campaignArn'])
print('Status: ' + description['status'])
```

------
#### [ SDK for Java 2\.x ]

In this example, the `itemExplorationWeight` and `explorationItemAgeCutOff` parameters are specific to the [User\-Personalization](native-recipe-new-item-USER_PERSONALIZATION.md) recipe\. The default itemExplorationWeight is *0\.3* and the default explorationItemAgeCutOff is *30*\. If you leave out campaign configuration parameters, the default values apply\.

```
public static void createCampaign(PersonalizeClient personalizeClient, 
                                String campaignName, 
                                String solutionVersionArn, 
                                Integer minProvisionedTPS, 
                                String itemExplorationWeight, 
                                String explorationItemAgeCutOff) {
                                
    //Optional code to instantiate a HashMap and add the explorationWeight and explorationItemAgeCutOff values.
    //Remove if you aren't using User-Personaliztion.
    Map<String,String> itemExploration = new HashMap<String,String>();
    itemExploration.put("explorationWeight", itemExplorationWeight);
    itemExploration.put("explorationItemAgeCutOff", explorationItemAgeCutOff);

    try {
        // Build a User-Personalization recipe specific campaignConfig object with the itemExploration map.
        // CampaignConfig construction will vary by recipe.
        CampaignConfig campaignConfig = CampaignConfig.builder()
            .itemExplorationConfig(itemExploration)
            .build();
    
        // build the createCampaignRequest
        CreateCampaignRequest createCampaignRequest = CreateCampaignRequest.builder()
            .name(campaignName)
            .solutionVersionArn(solutionVersionArn)
            .minProvisionedTPS(minProvisionedTPS)
            .campaignConfig(campaignConfig) //
            .build();

        // create the campaign
        CreateCampaignResponse campaignResponse = personalizeClient.createCampaign(createCampaignRequest);
        String campaignArn = campaignResponse.campaignArn();
        
        DescribeCampaignRequest campaignRequest = DescribeCampaignRequest.builder()
            .campaignArn(campaignArn)
            .build();
    
        DescribeCampaignResponse campaignResponse = personalizeClient.describeCampaign(campaignRequest);
        Campaign newCampaign = campaignResponse.campaign();
        
        System.out.println("The Campaign status is " + newCampaign.status());
    
    } catch (PersonalizeException e) {
        System.err.println(e.awsErrorDetails().errorMessage());
        System.exit(1);
    }
}
```

------
#### [ SDK for JavaScript v3 ]

```
// Get service clients module and commands using ES6 syntax.

import { CreateCampaignCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the campaign's parameters.
export const createCampaignParam = {
  solutionVersionArn: 'SOLUTION_VERSION_ARN', /* required */
  name: 'NAME',  /* required */
  minProvisionedTPS: 1    /* optional integer */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateCampaignCommand(createCampaignParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

------

The campaign is ready when its status is ACTIVE\. To get the current status, call [DescribeCampaign](API_DescribeCampaign.md) and check that the `status` field is `ACTIVE`\.

 If you manually retrain your solution version or want to change your campaign settings, you must update your campaign\. For more information see [Updating a campaign](update-campaigns.md)\. 

Amazon Personalize provides operations for managing campaigns such as [ListCampaigns](API_ListCampaigns.md) to list the campaigns you have created\. You can delete a campaign by calling [DeleteCampaign](API_DeleteCampaign.md)\. If you delete a campaign, the solution versions that are part of the campaign are not deleted\.

After you have created your campaign, use it to make recommendations\. For more information, see [Getting recommendations \(Custom dataset group\)](getting-recommendations.md)\.