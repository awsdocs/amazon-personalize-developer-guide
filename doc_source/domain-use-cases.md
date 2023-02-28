# Choosing recommender use cases<a name="domain-use-cases"></a>

 Each domain has different use cases\. When you create a recommender, you create it for a specific use case\. Each use case has different requirements for getting recommendations\. Some use cases require specific event types\. You are free to include additional event types\. 

 For all use cases, your interactions data must have the following: 
+ At minimum 1000 interactions records from users interacting with items in your catalog\. These interactions can be from bulk imports, or streamed events, or both\.
+ At minimum 25 unique user IDs with at least 2 interactions for each\.

For quality recommendations, we recommend that you have at minimum 50,000 interactions from at least 1,000 users with 2 or more interactions each\.

**Topics**
+ [VIDEO\_ON\_DEMAND use cases](VIDEO_ON_DEMAND-use-cases.md)
+ [ECOMMERCE use cases](ECOMMERCE-use-cases.md)