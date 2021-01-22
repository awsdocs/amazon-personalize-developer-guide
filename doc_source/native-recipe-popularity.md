# Popularity\-Count Recipe<a name="native-recipe-popularity"></a>

Popularity\-count recommends the most popular item items based on all of your user behavioral data\. The most popular items have the most interactions with unique users\. The recipe returns the same popular items for all users\. Popularity\-count is a good baseline for comparing with other recipes using the evaluation metrics Amazon Personalize generates when you create a solution version\. For more information see [Step 4: Evaluating a Solution Version](working-with-training-metrics.md)\. 

This predefined recipe has the following properties:
+  **Name** – `aws-popularity-count`
+  **Recipe ARN** – `arn:aws:personalize:::recipe/aws-popularity-count`
+  **Algorithm ARN** – `arn:aws:personalize:::algorithm/aws-popularity-count`
+  **Feature Transformation ARN** – `arn:aws:personalize:::feature-transformation/sims`
+  **Recipe type** – `USER_PERSONALIZATION`

Popularity\-count has no exposed hyperparameters\.