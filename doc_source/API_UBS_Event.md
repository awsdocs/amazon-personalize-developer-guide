# Event<a name="API_UBS_Event"></a>

Represents user interaction event information sent using the `PutEvents` API\.

## Contents<a name="API_UBS_Event_Contents"></a>

 **eventId**   <a name="personalize-Type-UBS_Event-eventId"></a>
An ID associated with the event\. If an event ID is not provided, Amazon Personalize generates a unique ID for the event\. An event ID is not used as an input to the model\. Amazon Personalize uses the event ID to distinquish unique events\. Any subsequent events after the first with the same event ID are not used in model training\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Required: No

 **eventType**   <a name="personalize-Type-UBS_Event-eventType"></a>
The type of event\. This property corresponds to the `EVENT_TYPE` field of the Interactions schema\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Required: No

 **properties**   <a name="personalize-Type-UBS_Event-properties"></a>
A string map of event\-specific data that you might choose to record\. For example, if a user rates a movie on your site, you might send the movie ID and rating, and the number of movie ratings made by the user\.  
Each item in the map consists of a key\-value pair\. For example,  
 `{"itemId": "movie1"}`   
 `{"itemId": "movie2", "eventValue": "4.5"}`   
 `{"itemId": "movie3", "eventValue": "3", "numberOfRatings": "12"}`   
The keys use camel case names that match the fields in the Interactions schema\. The `itemId` and `eventValue` keys correspond to the `ITEM_ID` and `EVENT_VALUE` fields\. In the above example, the `eventType` might be 'MovieRating' with `eventValue` being the rating\. The `numberOfRatings` would match the 'NUMBER\_OF\_RATINGS' field defined in the Interactions schema\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Required: No

 **sentAt**   <a name="personalize-Type-UBS_Event-sentAt"></a>
The timestamp \(in Unix time\) on the client side when the event occurred\.  
Type: Timestamp  
Required: Yes

## See Also<a name="API_UBS_Event_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/personalize-events-2018-03-22/Event) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/personalize-events-2018-03-22/Event) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/personalize-events-2018-03-22/Event) 
+  [AWS SDK for Ruby V3](https://docs.aws.amazon.com/goto/SdkForRubyV3/personalize-events-2018-03-22/Event) 