# Preparing and importing batch input data<a name="batch-data-upload"></a>

 Batch inference and batch segment jobs use a solution version to make recommendations or user segments based on data that you provide in an input JSON file\. Before you can get batch recommendations or user segments, you must prepare and upload your JSON file to an Amazon S3 bucket\. We recommend that you create an output folder in your Amazon S3 bucket or use a separate output Amazon S3 bucket\. You can then run multiple batch inference jobs using the same input data location\. 

**To prepare and import data**

1. 

   Format your batch input data depending on the type of batch workflow you are using and the recipe your solution uses\. For both workflows, separate input data element with a new line\.
   + For batch recommendations, your input data is a JSON file with a list of userIds \(USER\_PERSONALIZATION recipes\), a list of itemIds \(RELATED\_ITEMS\), or a list of userIds each paired with a collection of itemIds \(PERSONALIZED\_RANKING recipes\)\. For input data examples, see [Batch inference job input and output JSON examples](#batch-inference-job-json-examples)
   + For batch segment jobs, your input data is either a list of itemIds \(Item\-Affinity\) or item attributes \(Item\-Attribute\-Affinity\)\. For item attributes, input data can include logical expressions with the `AND` operator to get users for multiple items or attributes per query\. For input data examples, see [Batch segment job input and output JSON examples](#batch-segment-job-json-examples)\.

1.  Upload your input JSON to an input folder in your Amazon S3 bucket\. For more information, see [Uploading files and folders by using drag and drop](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) in the *Amazon Simple Storage Service User Guide* 

1.  Create a separate location for your output data, either a folder or a different Amazon S3 bucket\. By creating a separate location for the output JSON, you can run multiple batch inference or batch segment jobs with the same input data location\.

1.  Create a batch inference job or batch segment job and Amazon Personalize will output the recommendations or user segments from your solution version to your output data location\. 

## Input and output JSON examples<a name="batch-recommendations-json-examples"></a>

How you format your input data depends on the type of batch job you create and the recipe you use\. The following sections list correctly formatted JSON input and output examples for batch inference jobs and batch segment jobs\.

**Topics**
+ [Batch inference job input and output JSON examples](#batch-inference-job-json-examples)
+ [Batch segment job input and output JSON examples](#batch-segment-job-json-examples)

### Batch inference job input and output JSON examples<a name="batch-inference-job-json-examples"></a>

The following are correctly formatted JSON input and output examples for batch inference jobs organized by recipe\.

**User\-Personalization and legacy HRNN recipes**

------
#### [ Input ]

Separate each `userId` with a new line as follows\.

```
{"userId": "4638"}
{"userId": "663"}
{"userId": "3384"}
...
```

------
#### [ Output ]

```
{"input":{"userId":"4638"},"output":{"recommendedItems":["63992","115149","110102","148626","148888","31685","102445","69526","92535","143355","62374","7451","56171","122882","66097","91542","142488","139385","40583","71530","39292","111360","34048","47099","135137"],"scores":[0.0152238,0.0069081,0.0068222,0.006394,0.0059746,0.0055851,0.0049357,0.0044644,0.0042968,0.004015,0.0038805,0.0037476,0.0036563,0.0036178,0.00341,0.0033467,0.0033258,0.0032454,0.0032076,0.0031996,0.0029558,0.0029021,0.0029007,0.0028837,0.0028316]},"error":null}
{"input":{"userId":"663"},"output":{"recommendedItems":["368","377","25","780","1610","648","1270","6","165","1196","1097","300","1183","608","104","474","736","293","141","2987","1265","2716","223","733","2028"],"scores":[0.0406197,0.0372557,0.0254077,0.0151975,0.014991,0.0127175,0.0124547,0.0116712,0.0091098,0.0085492,0.0079035,0.0078995,0.0075598,0.0074876,0.0072006,0.0071775,0.0068923,0.0066552,0.0066232,0.0062504,0.0062386,0.0061121,0.0060942,0.0060781,0.0059263]},"error":null}
{"input":{"userId":"3384"},"output":{"recommendedItems":["597","21","223","2144","208","2424","594","595","920","104","520","367","2081","39","1035","2054","160","1370","48","1092","158","2671","500","474","1907"],"scores":[0.0241061,0.0119394,0.0118012,0.010662,0.0086972,0.0079428,0.0073218,0.0071438,0.0069602,0.0056961,0.0055999,0.005577,0.0054387,0.0051787,0.0051412,0.0050493,0.0047126,0.0045393,0.0042159,0.0042098,0.004205,0.0042029,0.0040778,0.0038897,0.0038809]},"error":null}
...
```

------

**Popularity\-Count**

------
#### [ Input ]

Separate each `userId` with a new line as follows\.

```
{"userId": "12"}
{"userId": "105"}
{"userId": "41"}
...
```

------
#### [ Output ]

```
{"input": {"userId": "12"}, "output": {"recommendedItems": ["105", "106", "441"]}}
{"input": {"userId": "105"}, "output": {"recommendedItems": ["105", "106", "441"]}}
{"input": {"userId": "41"}, "output": {"recommendedItems": ["105", "106", "441"]}}
...
```

------

**PERSONALIZED\_RANKING recipes**

------
#### [ Input ]

Separate each `userId` and list of `itemIds` to be ranked with a new line as follows\.

```
{"userId": "891", "itemList": ["27", "886", "101"]}
{"userId": "445", "itemList": ["527", "55", "901"]}
{"userId": "71", "itemList": ["27", "351", "101"]}
...
```

------
#### [ Output ]

```
{"input":{"userId":"891","itemList":["27","886","101"]},"output":{"recommendedItems":["27","101","886"],"scores":[0.48421,0.28133,0.23446]}}
{"input":{"userId":"445","itemList":["527","55","901"]},"output":{"recommendedItems":["901","527","55"],"scores":[0.46972,0.31011,0.22017]}}
{"input":{"userId":"71","itemList":["29","351","199"]},"output":{"recommendedItems":["351","29","199"],"scores":[0.68937,0.24829,0.06232]}}
...
```

------

**RELATED\_ITEMS recipes**

------
#### [ Input ]

Separate each `itemId` with a new line as follows\.

```
{"itemId": "105"}
{"itemId": "106"}
{"itemId": "441"}
...
```

------
#### [ Output ]

```
{"input": {"itemId": "105"}, "output": {"recommendedItems": ["106", "107", "49"]}}
{"input": {"itemId": "106"}, "output": {"recommendedItems": ["105", "107", "49"]}}
{"input": {"itemId": "441"}, "output": {"recommendedItems": ["2", "442", "435"]}}
...
```

------

### Batch segment job input and output JSON examples<a name="batch-segment-job-json-examples"></a>

When you create a batch segment job, your input data is either a list of itemIds \(Item\-Affinity recipe\) or item attributes \(Item\-Attribute\-Affinity\)\. Each line of input data is a separate inference query\. Each user segment is sorted in descending order based on the probability that each user will interact with items in your inventory\.

For item attributes, you can mix different columns of metadata\. For example one row might be a numerical column and the next might be a categorical column\. Also, your input item metadata can include logical expressions with the `AND` operator to get a user segment for multiple attributes\. For example, a line of your input data might be `{"itemAttributes": "ITEMS.genres = "\Comedy\" AND ITEMS.genres = "\Action\""}` or `{"itemAttributes": "ITEMS.genres = "\Comedy\" AND ITEMS.audience = "\teen\""}`\. When you combine two attributes with the `AND` operator, you create a user segment with users who are more likely to interact with items that have both attributes based on the users interactions history\. Unlike filter expressions \(which use the `IN` operator for string equality\), batch segment input expressions support only the `=` symbol for equality for string matching\. 



The following are correctly formatted JSON input and output examples for batch segment jobs organized by recipe\.

**Item\-Affinity**

------
#### [ Input ]

Separate each `itemId` with a new line as follows\.

```
{"itemId": "105"}
{"itemId": "106"}
{"itemId": "441"}
...
```

------
#### [ Output ]

```
{"input": {"itemId": "105"}, "output": {"recommendedUsers": ["106", "107", "49"]}}
{"input": {"itemId": "106"}, "output": {"recommendedUsers": ["105", "107", "49"]}}
{"input": {"itemId": "441"}, "output": {"recommendedUsers": ["2", "442", "435"]}}
...
```

------

**Item\-Attribute\-Affinity**

------
#### [ Input ]

Separate each attribute with a new line as follows\.

```
{"itemAttributes": "ITEMS.genres = \"Comedy\" AND ITEMS.genres = \"Action\""}
{"itemAttributes": "ITEMS.genres = \"Comedy\""}
{"itemAttributes": "ITEMS.genres = \"Horror\" AND ITEMS.genres = \"Action\""}
...
```

------
#### [ Output ]

```
{"itemAttributes": "ITEMS.genres = \"Comedy\" AND ITEMS.genres = \"Action\"", "output": {"recommendedUsers": ["25", "78", "108"]}}
{"itemAttributes": "ITEMS.genres = \"Adventure\"", "output": {"recommendedUsers": ["87", "31", "129"]}}
{"itemAttributes": "ITEMS.genres = \"Horror\" AND ITEMS.genres = \"Action\"", "output": {"recommendedUsers": ["8", "442", "435"]}}
...
```

------