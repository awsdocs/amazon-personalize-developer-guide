# Guidelines and requirements<a name="metric-attribution-requirements"></a>

 Amazon Personalize starts calculating and reporting the impact of recommendations only after you create a metric attribution\. To build the most complete history, we recommend creating a metric attribution before you import your interactions data\. When you create an Interactions dataset import job with the Amazon Personalize console, you have the option to create a metric attribution in a new tab\. Then you can return to the import job to complete it\. 

 After you create a metric attribution and record events or import incremental bulk data, you will incur some monthly CloudWatch cost per metric\. For information about CloudWatch pricing, see the [Amazon CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/) page\. To stop sending metrics to CloudWatch, [delete the metric attribution](deleting-metric-attribution.md)\.

 To see the impact of recommendations over time, keep importing data as customers interact with recommendations\. If you have already imported data, you can still create a metric attribution and start measuring recommendation impact\. However, Amazon Personalize won’t report on data that you imported before you created it\. 

The following are guidelines and requirements for generating reports with a metric attribution:
+ You must grant Amazon Personalize permission to access and put data in CloudWatch\. For policy examples, see [Giving Amazon Personalize access to CloudWatch](#metric-attribution-cw-permissions)\.
+ To publish metrics to Amazon S3, give Amazon Personalize permission to write to your bucket\. You also must provide the bucket path in your metric attribution\. For policy examples, see [Giving Amazon Personalize access to your Amazon S3 bucket](#metric-attribution-s3-permissions)\.
+  To publish metrics to CloudWatch, records must be less than 14 days old\. If your data is older, these records won't be included in calculations or reports\. 
+  Importing duplicate events \(events that match for all attributes exactly\) can lead to unexpected behavior including inaccurate metrics\. We recommend that you remove duplicate records from any bulk data before import, and avoid importing duplicate events with the `PutEvents` operation\. 
+ Your Interactions dataset must have an `EVENT_TYPE` column\.
+ You can create at most one metric attribution per dataset group\. Each metric attribution can have at most 10 metrics\.

To compare sources, each interaction event must include a `recommendationId` or `eventAttributionSource`\. You can provide at most 100 unique event attribution sources\. For `PutEvents` code samples, see [Event metrics and attribution reports](recording-events.md#event-metrics)\.
+  If you provide a `recommendationId`, Amazon Personalize automatically determines the source campaign or recommender and identifies it in reports in an EVENT\_ATTRIBUTION\_SOURCE column\. 
+  If you provide both attributes, Amazon Personalize uses only the `eventAttributionSource`\. 
+  If you don't provide a source, Amazon Personalize labels the source `SOURCE_NAME_UNDEFINED` in reports\. 

**Topics**
+ [Giving Amazon Personalize access to CloudWatch](#metric-attribution-cw-permissions)
+ [Giving Amazon Personalize access to your Amazon S3 bucket](#metric-attribution-s3-permissions)

## Giving Amazon Personalize access to CloudWatch<a name="metric-attribution-cw-permissions"></a>

**Important**  
When you grant permissions, Amazon Personalize places and validates a small amount of data in CloudWatch\. This will incur a one\-time cost of less than $0\.30\. For more information about CloudWatch pricing, see the [Amazon CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/) page\.

To give Amazon Personalize access to CloudWatch, attach a new AWS Identity and Access Management \(IAM\) policy to your Amazon Personalize service role that grants the role permission to use the `PutMetricData` Action for CloudWatch\. The following policy example grants `PutMetricData` permissions\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:PutMetricData"
      ],
      "Resource": "*"
    }
  ]
}
```

## Giving Amazon Personalize access to your Amazon S3 bucket<a name="metric-attribution-s3-permissions"></a>

 To give Amazon Personalize access to your Amazon S3 bucket:
+ Attach an IAM policy to your Amazon Personalize service role that grants the role permission to use the `PutObject` Action on your bucket\. 

  ```
  {
      "Version": "2012-10-17",
      "Id": "PersonalizeS3BucketAccessPolicy",
      "Statement": [
          {
              "Sid": "PersonalizeS3BucketAccessPolicy",
              "Effect": "Allow",
              "Action": [
                  "s3:PutObject"
              ],
              "Resource": [
                  "arn:aws:s3:::bucket-name",
                  "arn:aws:s3:::bucket-name/*"
              ]
          }
      ]
  }
  ```
+ Attach a bucket policy to your output Amazon S3 bucket that grants the Amazon Personalize principle permission to use the `PutObject` Actions\.

   If you use AWS Key Management Service \(AWS KMS\) for encryption, you must grant Amazon Personalize and your Amazon Personalize IAM service role permission to use your key\. For more information, see [Giving Amazon Personalize permission to use your AWS KMS key](granting-personalize-key-access.md)\.

  ```
  {
      "Version": "2012-10-17",
      "Id": "PersonalizeS3BucketAccessPolicy",
      "Statement": [
          {
              "Sid": "PersonalizeS3BucketAccessPolicy",
              "Effect": "Allow",
              "Principal": {
                  "Service": "personalize.amazonaws.com"
              },
              "Action": [
                  "s3:PutObject"
              ],
              "Resource": [
                  "arn:aws:s3:::bucket-name",
                  "arn:aws:s3:::bucket-name/*"
              ]
          }
      ]
  }
  ```