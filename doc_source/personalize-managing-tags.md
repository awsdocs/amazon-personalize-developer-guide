# Managing tags<a name="personalize-managing-tags"></a>

Each tag consists of a required tag key and an optional tag value, both of which you define\. A tag key is a general label that acts as a category for more specific tag values\. A tag value acts as a descriptor for a tag key\.

For example, if you have two versions of an Amazon Personalize dataset group \(one for internal testing and another for production\), you might assign an `Environment` tag key to both projects\. The tag value of the `Environment` tag might be `Test` for one version of the dataset group and `Production` for the other version\.

The following restrictions apply to tags:
+ Maximum number of tags per resource – 50
+ Maximum key length – 128 Unicode characters in UTF\-8
+ Maximum value length – 256 Unicode characters in UTF\-8
+ Tag keys and values can contain the following characters: A\-Z, a\-z, 0\-9, space, and \_ \. : / = \+ @ – \(hyphen\)\. This is the standard set of characters available across AWS services that support tags\. Some services support additional symbols\.
+ Tag keys and tag values are case sensitive\.
+ For each associated resource, each tag key must be unique and it can have only one tag value\.
+ Your tag keys and tag values can't start with `aws:`\. AWS services apply tags that start with `aws:`, and those tags can't be modified\. They don't count towards tag limits\.
+ You can't update or delete a resource based only on its tags\. You must also specify the Amazon Resource Name \(ARN\) or resource ID, depending on the operation that you use\.