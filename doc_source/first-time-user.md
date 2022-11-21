# Guidance for first\-time Amazon Personalize users<a name="first-time-user"></a>

If you're a first\-time user of Amazon Personalize, we recommend you read the following sections in order:

1. **[How it works](how-it-works.md)** – This section introduces the Amazon Personalize workflow and walks you through the steps to create personalized experiences for your users\. This section also includes common Amazon Personalize terms and their definitions\. Start with this section to make sure you have good understanding of Amazon Personalize workflows and terms before you start getting recommendations\. 

1. **[Setting up Amazon Personalize](setup.md)** – In this section you set up your AWS account, set up the required permissions to use Amazon Personalize, and set up the AWS CLI and the AWS SDKs to use and manage Amazon Personalize\.

1. **[Getting started](getting-started.md)** – In this section you get started using Amazon Personalize with a simple movie dataset\. Complete these tutorials to get hands\-on experience with Amazon Personalize\. You can choose to either get started with a Domain dataset group or a Custom dataset group: 
   +  To get started creating a Domain dataset group, complete the [Getting started prerequisites](gs-prerequisites.md) and then start the tutorials in [Getting started with a Domain dataset group](getting-started-domain.md)\. 
   +  To get started with a Custom dataset group, complete the [Getting started prerequisites](gs-prerequisites.md) and then start the tutorials in [Getting started with a Custom dataset group](getting-started-custom.md)\. 

1. Depending on your application, complete either of the following sections:
   + **[Domain dataset groups](domain-dataset-groups.md)** – If you have a streaming video or ecommerce application, follow the procedures in this section to create a Domain dataset group and recommenders for your use cases\. These procedures build upon what you learned when completing the Domain dataset group getting started exercises\. They provide more in\-depth information about recommenders and domain use cases\. 
   + **[Custom dataset groups](custom-dataset-groups.md)** – If you don't have a streaming video or ecommerce application, follow the procedures in this section to create a Custom dataset group\. These procedures build upon what you learned when completing the Custom dataset group getting started exercises\. They provide more in depth information about custom recipes and models\. 

1. **[Recording events](recording-events.md)** – This section covers how to record user interaction events in real time\. After you have set up your Amazon Personalize resources, complete this section to learn how to keep your Interactions dataset up to date with your users' behavior\. You do this by recording interaction *[events](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#event)* with an event tracker and the [PutEvents](API_UBS_PutEvents.md) operation\. 

1. **[Filtering recommendations and user segments](filter.md)** – This section covers how to filter recommendations\. Complete this section to learn how to construct filter expressions to filter recommendations based on custom criteria\. For example, you might not want to recommend products that a user has already purchased, or recommend movies that a user has already watched\. 