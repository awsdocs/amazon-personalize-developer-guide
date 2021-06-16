# Updating a campaign<a name="update-campaigns"></a>

To deploy a retrained solution version with an existing campaign or to change your campaign's [Minimum provisioned TPS](campaigns.md#min-tps-auto-scaling) or campaign configuration, you must manually update the campaign\. 

 With the User\-Personalization recipe, Amazon Personalize automatically updates your latest solution version \(trained with `trainingMode` set to `FULL`\) every two hours to include new items in recommendations, and your campaign automatically uses the updated solution version\. Manually update a campaign only when you manually retrain the solution version with `trainingMode` set to `FULL`, or when you want to make changes to your campaign's `minProvisionedTPS` or campaign configuration\. For more information on automatic updates with the User\-Personalization recipe see [Automatic updates](native-recipe-new-item-USER_PERSONALIZATION.md#automatic-updates)\. 

You manually update a campaign with the Amazon Personalize console, AWS Command Line Interface \(AWS CLI\), or AWS SDKs\.

**Topics**
+ [Updating a campaign \(console\)](#update-campaign-console)
+ [Updating a campaign \(AWS CLI\)](#update-campaign-cli)
+ [Updating a campaign \(AWS SDKs\)](#update-campaign-sdk)

## Updating a campaign \(console\)<a name="update-campaign-console"></a>

To deploy a manually retrained solution version or make changes to your campaign configuration, you must update your campaign\.

**To update a campaign \(console\)**

1. Open the Amazon Personalize console at [https://console\.aws\.amazon\.com/personalize/home](https://console.aws.amazon.com/personalize/home) and sign into your account\.

1.  Choose the dataset group with the campaign you want to update\. 

1. In the navigation pane, choose **Campaigns**\.

1. On the **Campaigns** page, choose the campaign you want to update\.

1. On the campaign details page, choose **Update**\.

1. On the **Update campaign** page, make your changes\. For example, if you are deploying a retrained solution version, for **Solution version ID**, choose the identification number for the new solution version\.

1. Choose **Update**\. Amazon Personalize updates the campaign to use the new solution version and any changed configurations\.

## Updating a campaign \(AWS CLI\)<a name="update-campaign-cli"></a>

To deploy a new solution version, change your campaign's [Minimum provisioned TPS](campaigns.md#min-tps-auto-scaling), or change your campaign's configuration, you must update your campaign\. Use the following `update-campaign` command to update a campaign to use a new solution version with the AWS CLI\. 

Replace `campaign arn` with the Amazon Resource Name \(ARN\) of the campaign you want to update\. Replace `new solution version arn` with the solution version you want to deploy\. 

```
aws personalize update-campaign \
--campaign-arn campaign arn \
--solution-version-arn new solution version arn \
--min-provisioned-tps 1
```

## Updating a campaign \(AWS SDKs\)<a name="update-campaign-sdk"></a>

To deploy a new solution version, change your campaign's [Minimum provisioned TPS](campaigns.md#min-tps-auto-scaling) or change your campaign's configuration, you must update your campaign\. Use the following code to update a campaign with the SDK for Python \(Boto3\) or SDK for Java 2\.x\. For a complete list of parameters, see [UpdateCampaign](API_UpdateCampaign.md)\. 

------
#### [ SDK for Python \(Boto3\) ]

Use the following `update_campaign` method to deploy a new solution version\. Replace `campaign arn` with the Amazon Resource Name \(ARN\) of the campaign you want to update, replace the `new solution version arn` with the new solution version ARN and optionally change the `minProvisionedTPS`\.

```
import boto3

personalize = boto3.client('personalize')

response = personalize.update_campaign(
    campaignArn = 'campaign arn',
    solutionVersionArn = 'new solution version arn',
    minProvisionedTPS = 1,
)

arn = response['campaignArn']

description = personalize.describe_campaign(campaignArn = arn)['campaign']
print('Name: ' + description['name'])
print('ARN: ' + description['campaignArn'])
print('Status: ' + description['status'])
```

------
#### [ SDK for Java 2\.x ]

Use the following `updateCampaign` method to update a campaign to use a new solution version\. Pass as parameters an Amazon Personalize service client, the new solution version's Amazon Resource Name \(ARN\), and the [Minimum provisioned TPS](campaigns.md#min-tps-auto-scaling)\. 

```
public static void updateCampaign(PersonalizeClient personalizeClient, 
                                String campaignArn,
                                String solutionVersionArn, 
                                Integer minProvisionedTPS) {

    try {    
        // build the updateCampaignRequest
        UpdateCampaignRequest updateCampaignRequest = UpdateCampaignRequest.builder()
            .campaignArn(campaignArn)
            .solutionVersionArn(solutionVersionArn)
            .minProvisionedTPS(minProvisionedTPS)
            .build();
        
        // update the campaign
        personalizeClient.updateCampaign(updateCampaignRequest);
        
        DescribeCampaignRequest campaignRequest = DescribeCampaignRequest.builder()
              .campaignArn(campaignArn)
              .build();
    
        DescribeCampaignResponse campaignResponse = personalizeClient.describeCampaign(campaignRequest);
        Campaign updatedCampaign = campaignResponse.campaign();
        
        System.out.println("The Campaign status is " + updatedCampaign.status());
    
    } catch (PersonalizeException e) {
        System.err.println(e.awsErrorDetails().errorMessage());
        System.exit(1);
    }
}
```

------