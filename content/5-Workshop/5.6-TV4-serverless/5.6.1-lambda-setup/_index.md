---
title: "Create AWS Lambda"
date: 2026-07-03
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---

#### Create Lambda Functions

1. Search for `Lambda` in the AWS Console → click **Create function**.
2. Select **Author from scratch**.
3. **Function name:** `ProjectImportLambda` (Create the first function).
4. **Runtime:** Select `Java 17`.
5. Click **Create function**.

![](/images/5-Workshop/5.6/1.png)
![](/images/5-Workshop/5.6/2.png)

Repeat the steps above to create a total of 5 Lambda functions for the system:
* `FileTreeLambda`
* `BedrockInvokeLambda`
* `ResultAndHistoryLambda`
* `ProjectImportLambda`
* `RagContextLambda`

![](/images/5-Workshop/5.6/3.png)

#### Configure ProjectImportLambda

1. Click on the newly created `ProjectImportLambda` function.
2. Switch to the **Configuration** tab.
3. Select **Environment variables** from the left menu → Click **Edit** to add variables for database and S3 connections:
   * `DB_PASS`
   * `DB_URL`
   * `DB_USER`
   * `SOURCE_CODE_BUCKET`

![](/images/5-Workshop/5.6/4.png)
![](/images/5-Workshop/5.6/5.png)

4. Still under the **Configuration** tab, select **VPC** from the left menu → Click **Edit**.
5. Select the project's **VPC**.
6. Choose the **Subnets** (Private subnets are recommended for secure RDS connections).
7. Select the default **Security groups** of the VPC.
8. Click **Save**.

![](/images/5-Workshop/5.6/6.png)
![](/images/5-Workshop/5.6/7.png)

#### Configure RagContextLambda

Similar to `ProjectImportLambda`, the `RagContextLambda` function requires environment and network configurations to chunk source code and store vectors in RDS.

1. Go back to the Functions list and select `RagContextLambda`.
2. Switch to the **Configuration** tab → **Environment variables** → Click **Edit** to add:
   * `DB_PASS`
   * `DB_URL`
   * `DB_USER`
   * `SOURCE_CODE_BUCKET`
   * `GEMINI_API_KEY` *(Note: This is based on the legacy configuration in the image; adjust accordingly if using Bedrock Mantle).*

![](/images/5-Workshop/5.6/8.png)

3. Switch to the **VPC** section → Click **Edit**.
4. Set up the **VPC**, **Subnets**, and **Security groups** exactly as done for the import function.
5. Click **Save**.

![](/images/5-Workshop/5.6/9.png)
![](/images/5-Workshop/5.6/10.png)
#### Configure ResultAndHistoryLambda

This function is responsible for saving the generated Unit Test results and history to the database, so it requires similar connection configurations.

1. From the Functions list, select the `ResultAndHistoryLambda` function.
2. Switch to the **Configuration** tab.
3. Select **Environment variables** from the left menu → Click **Edit** to add the RDS connection variables:
   * `DB_PASS`
   * `DB_URL`
   * `DB_USER`

![](/images/5-Workshop/5.6/11.png)

4. Still under the **Configuration** tab, select **VPC** from the left menu → Click **Edit**.
5. Select the **VPC**, **Subnets** (Private Subnets), and **Security groups** just like in previous steps to ensure the function can communicate internally with the database.
6. Click **Save** to apply the configuration.

![](/images/5-Workshop/5.6/12.png)
![](/images/5-Workshop/5.6/13.png)

#### Configure BedrockInvokeLambda

This function acts as the direct communicator with the AI (Bedrock Mantle) to generate source code. It only requires the API key (no VPC attachment is needed unless internal RDS connection is explicitly required).

1. Open the `BedrockInvokeLambda` function.
2. Switch to the **Configuration** tab → **Environment variables** → Click **Edit**.
3. Add the environment variable containing the API key for authentication (the example image uses `OpenAi`, adjust the variable name if using a different system).

![](/images/5-Workshop/5.6/14.png)

#### Configure FileTreeLambda

This function is tasked with reading and analyzing the project's File Tree structure uploaded to S3.

1. Open the `FileTreeLambda` function.
2. Switch to the **Configuration** tab → **Environment variables** → Click **Edit**.
3. Add the environment variable pointing to the source code repository:
   * `S3_BUCKET_NAME` (e.g., `zerobug-source-code-hutech-2026`)

![](/images/5-Workshop/5.6/15.png)