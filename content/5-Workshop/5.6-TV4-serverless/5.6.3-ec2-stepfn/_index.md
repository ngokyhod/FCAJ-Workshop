---
title: "EC2 Configuration & Workflow Trigger"
date: 2026-07-03
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---

For the Spring Boot application running on the EC2 instance to send execution triggers (`StartExecution`) to the Step Functions workflow created in the previous step, the server instance must be assigned a secure IAM Role with appropriate privileges.

#### 1. Creating the IAM Role for EC2
First, we need to create a Role to grant the EC2 instance permission to communicate with AWS services.

1. Navigate to the **IAM** dashboard on the AWS Console → Select **Roles** from the left menu → Click **Create role**.
2. Under **Trusted entity type**, select **AWS service**. For the **Use case**, select **EC2** and click **Next**.
3. On the **Add permissions** page, search for and check the `AmazonSSMManagedInstanceCore` policy (This grants baseline access to securely manage your instance via Session Manager).
4. Proceed to the final step, name the Role `ZeroBug-App-Role`, and click **Create role**.

![](/images/5-Workshop/5.6/18.png)
![](/images/5-Workshop/5.6/19.png)
![](/images/5-Workshop/5.6/20.png)
![](/images/5-Workshop/5.6/21.png)

#### 2. Granting Step Functions Execution Rights (Inline Policy)
By default, `ZeroBug-App-Role` cannot interfere with automation state machines. We must attach a specific Inline Policy to grant this permission:

1. Click on the newly created `ZeroBug-App-Role` from the list.
2. Under the **Permissions** tab, open the **Add permissions** dropdown → Select **Create inline policy**.

![](/images/5-Workshop/5.6/22.png)

3. Switch the editor to the **JSON** tab and paste the following permission block (Make sure to replace the ARN string with your actual State Machine ARN):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowBackendToStartStepFunctions",
            "Effect": "Allow",
            "Action": "states:StartExecution",
            "Resource": "arn:aws:states:ap-southeast-1:123456789012:stateMachine:ZeroBug-Workflow"
        }
    ]
}
```
Click save to enforce the policy modifications.

![](/images/5-Workshop/5.6/23.png)

> ⚠️ **MANDATORY ACTION:** Once the IAM configurations are complete, return to the **EC2 Instances** dashboard → Select your project server instance → Choose **Actions** → **Security** → **Modify IAM role** → Select `ZeroBug-App-Role` and hit **Update** to officially apply the role to your instance.

#### 3. Triggering the Workflow in Spring Boot
With server permissions fully established, the final step is to use the `AWS SDK` within your Backend's Java source code to fire an execution request to Step Functions:

```java
import software.amazon.awssdk.services.sfn.SfnClient;
import software.amazon.awssdk.services.sfn.model.StartExecutionRequest;
import software.amazon.awssdk.services.sfn.model.StartExecutionResponse;

// Instantiate the internal AWS SfnClient interface
SfnClient sfnClient = SfnClient.builder().region(Region.AP_SOUTHEAST_1).build();

// Format input parameters into a standard JSON string payload
String jsonPayload = "{ \"projectId\": \"1783058\", \"sourceUrl\": \"https://github/...\" }";

// Build the execution request targeting the specific State Machine
StartExecutionRequest executionRequest = StartExecutionRequest.builder()
        .stateMachineArn("arn:aws:states:ap-southeast-1:123456789012:stateMachine:ZeroBug-Workflow")
        .input(jsonPayload)
        .build();

// Fire the end-to-end pipeline execution signal
StartExecutionResponse response = sfnClient.startExecution(executionRequest);
System.out.println("Workflow successfully triggered. Execution ARN: " + response.executionArn());
```