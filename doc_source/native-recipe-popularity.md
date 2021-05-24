# Popularity\-Count recipe<a name="native-recipe-popularity"></a>

Popularity\-Count recommends the most popular item items based on all of your user behavioral data\. The most popular items have the most interactions with unique users\. The recipe returns the same popular items for all users\. Popularity\-Count is a good baseline for comparing with other recipes using the evaluation metrics Amazon Personalize generates when you create a solution version\. For more information see [Step 4: Evaluating a solution version with metrics](working-with-training-metrics.md)\. 

This predefined recipe has the following properties:
+  **Name** – `aws-popularity-count`
+  **Recipe ARN** – `arn:aws:personalize:::recipe/aws-popularity-count`
+  **Algorithm ARN** – `arn:aws:personalize:::algorithm/aws-popularity-count`
+  **Feature transformation ARN** – `arn:aws:personalize:::feature-transformation/sims`
+  **Recipe type** – `USER_PERSONALIZATION`

Popularity\-Count has no exposed hyperparameters\.