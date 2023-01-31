# What is Amazon Personalize?<a name="what-is-personalize"></a>

Amazon Personalize is a fully managed machine learning service that uses your data to generate item recommendations for your users\. It can also generate user segments based on the users' affinity for certain items or item metadata\. 

Common use case examples include the following:
+ **Personalizing a video streaming app** – You can use preconfigured or customizable Amazon Personalize resources to add multiple types of personalized video recommendations to your streaming app\. For example, *Top picks for you*, *More like X* and *Most popular* video recommendations\. 
+ **Adding product recommendations to an ecommerce app** – You can use preconfigured or customizable Amazon Personalize resources to add multiple types of personalized product recommendations to your retail app\. For example, *Recommended for you*, *Frequently bought together* and *Customers who viewed X also viewed* product recommendations\. 
+ **Creating personalized emails** – You can use customizable Amazon Personalize resources to generate batch recommendations for all users on an email list\. Then you can use an [AWS service](#related-services) or [third party service](#third-parties) to send users personalized emails recommending items in your catalog\. 
+ **Creating a targeted marketing campaign** – You can use Amazon Personalize to generate segments of users who will most likely interact with items in your catalog\. Then you can use an [AWS service](#related-services) or [third party service](#third-parties) to create a targeted marketing campaign that promotes different items to different user segments\.

Amazon Personalize generates recommendations primarily based on interactions data\. Interactions data is data from your users interacting with items in your catalog\. For example, users clicking different items\. Your interactions data can come from both your historical bulk interaction records in a CSV file, and real\-time events from your users as they interact with your catalog\. In some cases, Amazon Personalize also uses metadata about items and users, such as genre, price, or gender\. 

Amazon Personalize includes API operations for real\-time personalization, and batch operations for bulk recommendations and user segments\. You can get started quickly with use\-case optimized recommenders for your business domain, or you can create your own configurable custom resources\. 

**Topics**
+ [Pricing for Amazon Personalize](#whatis-pricing)
+ [Guidance for first\-time Amazon Personalize users](first-time-user.md)
+ [Related AWS services and solutions](#related-services)
+ [Third\-party services](#third-parties)
+ [Learn more](#experienced-user)

## Pricing for Amazon Personalize<a name="whatis-pricing"></a>

 With Amazon Personalize, there are no minimum fees and no upfront commitments\. The [AWS Free Tier](https://aws.amazon.com/free/) provides a monthly quota of up to 20 GB of data processing per available AWS region, up to 100 hours of training time per eligible AWS region, and up to 50 TPS\-hours of real\-time recommendations/month\. The free tier is valid for the first two months of usage\.

For a complete list of charges and prices, see [Amazon Personalize pricing](https://aws.amazon.com/personalize/pricing/)\.

## Related AWS services and solutions<a name="related-services"></a>

Amazon Personalize integrates seamlessly with other AWS services and solutions\. For example, you can:
+ Use AWS Amplify to record user interaction events\. Amplify includes a JavaScript library for recording events from web client applications, and a library for recording events in server code\. For more information, see [Amplify \- analytics](https://aws-amplify.github.io/docs/js/analytics)\.
+  Automate and schedule Amazon Personalize tasks with [Maintaining Personalized Experiences with Machine Learning](https://aws.amazon.com/solutions/implementations/maintaining-personalized-experiences-with-ml/)\. This AWS Solutions Implementation automates the Amazon Personalize workflow, including data import, solution version training, and batch workflows\. 
+  Use Amazon CloudWatch Evidently to perform A/B testing with Amazon Personalize recommendations\. For more information, see [Perform launches and A/B experiments with CloudWatch Evidently](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Evidently.html) in the *Amazon CloudWatch User Guide*\. 
+ Use Amazon Pinpoint to create targeted marketing campaigns\. For an example that shows how to use Amazon Pinpoint and Amplify to add Amazon Personalize recommendations to a marketing email campaign and a web app, see [Web Analytics with Amplify](https://catalog.us-east-1.prod.workshops.aws/workshops/bb080ee8-4722-4290-ac6e-d4cde0a65142/en-US)\. 

## Third\-party services<a name="third-parties"></a>

Amazon Personalize works well with various third\-party services\.
+ **Amplitude** – You can use Amplitude to track user actions to help you understand your users' behavior\. For information on using Amplitude and Amazon Personalize, see the following AWS Partner Network \(APN\) blog post: [Measuring the Effectiveness of Personalization with Amplitude and Amazon Personalize](http://aws.amazon.com/blogs/apn/measuring-the-effectiveness-of-personalization-with-amplitude-and-amazon-personalize/)\. 
+ **Braze** – You can use Braze to send users personalized emails recommending items in your catalog\. Braze is a market leading messaging platform \(email, push, SMS\)\. For a workshop that shows how to integrate Amazon Personalize and Braze, see [Amazon Personalize workshop](https://www.braze.com/docs/partners/message_personalization/dynamic_content/amazon_personalize/workshop/)\.
+ **mParticle** – You can use mParticle to collect event data from your app\. For an example that shows how to use mParticle and Amazon Personalize to implement personalized product recommendations, see [How to harness the power of a CDP for machine learning: Part 2](https://www.mparticle.com/blog/cdp-machine-learning-part-2/)\.
+ **Optimizely** – You can use Optimizely to perform A/B testing with Amazon Personalize recommendations\. For information on using Optimizely and Amazon Personalize, see [Optimizely integrates with Amazon Personalize to combine powerful machine learning with experimentation](https://www.optimizely.com/insights/blog/optimizely-for-amazon-personalize/)\.
+ **Segment** – You can use Segment to send your data to Amazon Personalize\. For more information on integrating Segment with Amazon Personalize, see [Amazon Personalize Destination](https://segment.com/docs/connections/destinations/catalog/amazon-personalize/)\. 

For a complete list of partners, see [Amazon Personalize Partners](https://aws.amazon.com/personalize/partners/)\.

## Learn more<a name="experienced-user"></a>

The following resources provide additional information about Amazon Personalize:
+ For a quick reference to help you determine if Amazon Personalize fits your use case, see the [Amazon Personalize Cheat Sheet](https://github.com/aws-samples/amazon-personalize-samples/blob/master/PersonalizeCheatSheet2.0.md) in the [Amazon Personalize samples](https://github.com/aws-samples/amazon-personalize-samples) repository\.
+ For a series of videos on how to use Amazon Personalize, see the [Amazon Personalize Deep Dive Video Series](https://www.youtube.com/watch?v=3gJmhoLaLIo) found on YouTube\.
+ For in\-depth tutorials and code samples, see the [amazon\-personalize\-samples GitHub repository](https://github.com/aws-samples/amazon-personalize-samples)\.