---
title: "Monitoring & Debugging"
date: 2026-07-03
weight: 4
chapter: false
pre: " <b> 5.6.4. </b> "
---

In a Serverless architecture, our code runs in ephemeral compute environments. There is no persistent physical server where we can simply open a Terminal to check for runtime errors. Therefore, **Amazon CloudWatch** acts as the "eyes and ears" of the entire system. Every execution, whether successful or failed, is logged here in extreme detail.

Below is the standard 4-step process to trace and debug a Lambda function (using `ProjectImportLambda` as an example):

#### 1. Select the Target Lambda Function
First, navigate to the **Lambda** service on the AWS Console. On the **Functions** dashboard, you will see a list of all functions within the project. Click on the name of the function you suspect is causing issues (in this case, `ProjectImportLambda`).

![](/images/5-Workshop/5.6/24.png)

#### 2. Open the CloudWatch Monitoring Portal
Inside the Lambda function's detailed configuration screen, perform the following actions:
1. Switch to the **Monitor** tab.
2. Click the **View CloudWatch logs** button on the right side of the screen. This will automatically redirect you to the dedicated log repository for this specific Lambda function in CloudWatch.

![](/images/5-Workshop/5.6/25.png)

#### 3. Select the Latest Log Stream
Every time a Lambda function is triggered, AWS creates a new **Log stream**.
On the CloudWatch dashboard, under the **Log streams** section, locate and click on the top-most stream (this represents the most recent execution of the function).

![](/images/5-Workshop/5.6/26.png)

#### 4. Read Logs and Troubleshoot
The **Log events** interface will display a detailed, millisecond-by-millisecond breakdown of your code's execution. Pay close attention to lines containing keywords like `LỖI` (ERROR), `ERROR`, `Exception`, or `Task timed out`.

**Practical example from the image below:** The system throws an error: `Authentication is required but no CredentialsProvider has been registered`.
* **Root Cause:** The JGit code is attempting to clone a Private Repository from GitHub but has not been provided with the necessary Username/Token credentials.
* **Solution:** Change the GitHub Repository visibility to Public, or inject a CredentialsProvider into the JGit source code.

![](/images/5-Workshop/5.6/27.png)

Thanks to these detailed logs, pinpointing the root cause of an issue and resolving it in a Serverless architecture becomes highly transparent and manageable!