---
title: "Resource Cleanup – Hoa"
date: 2026-07-03
weight: 2
chapter: false
pre: " <b> 5.10.2. </b> "
---

After successfully verifying the system, we must proceed with cleaning up the provisioned Serverless resources to prevent unexpected storage and operational charges. Please execute the following steps in order.

#### Step 1 — Delete Lambda Functions (5 Functions)

We will proceed to delete all 5 Lambda functions created in section 5.6. The target list includes:

* `ProjectImportLambda`
* `FileTreeLambda`
* `RagContextLambda`
* `BedrockInvokeLambda`
* `ResultAndHistoryLambda`

**Execution Steps:**
1. Navigate to the **Lambda** service → select **Functions**.
2. In the functions list, check the box next to the function you wish to delete (e.g., `alllambda` or project-specific functions).
3. Click the **Actions** dropdown menu in the top right corner → select **Delete**.

![](/images/5-Workshop/5.6/33.png)

4. A warning modal will appear. You must type `confirm` into the text field to verify the permanent deletion of the function's code and configuration. Click **Delete** to finalize.

![](/images/5-Workshop/5.6/34.png)

*(Repeat this process until all 5 project Lambda functions are removed).*

#### Step 2 — Clear CloudWatch Logs

When a Lambda function is deleted, its execution logs persist in the system and continue to incur storage costs. Therefore, cleaning up CloudWatch is a mandatory step.

1. Navigate to the **CloudWatch** service on the AWS Console.
2. In the left-hand navigation pane, locate the **Logs** section → Select **Log Management** (or Log groups).

![](/images/5-Workshop/5.6/35.png)

3. In the search bar, type the Lambda function's name (e.g., `/aws/lambda/BedrockInvokeLambda`) to filter the corresponding log group. Click on the Log group's name.

![](/images/5-Workshop/5.6/36.png)

4. Inside the detailed view, scroll down to the **Log streams** tab. Check the boxes to select all existing log streams.
5. Click the **Delete** button located on the toolbar above the list.

![](/images/5-Workshop/5.6/37.png)

6. Confirm the deletion of these log streams by clicking **Delete** on the prompt modal.

![](/images/5-Workshop/5.6/38.png)

> 💡 **Pro Tip:** Instead of deleting individual Log streams, you can select the parent **Log groups** directly from the main dashboard (Image 36), click **Actions** → **Delete log group(s)** for a faster and cleaner wipe. Repeat this for all 5 Log groups associated with your Lambda functions.

#### Confirmation Checklist

- [ ] All 5 Lambda functions deleted successfully.
- [ ] All associated Log groups/streams in CloudWatch cleared.
- [ ] (Optional) Navigate to Step Functions and delete the `ZeroBug-Workflow` State Machine if you were the creator.

→ Next: [Toàn — EC2 & RDS Cleanup](5.10.3-toan/)