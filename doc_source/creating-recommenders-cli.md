# Creating recommenders \(AWS CLI\)<a name="creating-recommenders-cli"></a>

 Use the following AWS CLI code to create a recommender for a domain use case\. Run this code for each of your domain use cases\. For `recipeArn`, provide the Amazon Resource Name \(ARN\) for your use case\. The available use cases depend on your domain\. For a list of use cases and their ARNs see [Choosing recommender use cases](domain-use-cases.md)\.he available use cases depend on your domain\. For information on choosing a use case see [Choosing recommender use cases](domain-use-cases.md)\. 

```
aws personalize create-recommender \
--name recommender name \
--dataset-group-arn dataset group ARN \
--recipe-arn recipe ARN
```