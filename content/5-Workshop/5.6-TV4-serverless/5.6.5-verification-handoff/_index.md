---
title: "End-to-End Verification (Verifying Handoff)"
date: 2026-07-03
weight: 5
chapter: false
pre: " <b> 5.6.5. </b> "
---

This is the final step to validate the entire ZeroBug-Agent system architecture. We will simulate an end-user scenario, walking through the actual User Flow on the Frontend to verify seamless data traversal: from the Browser $\rightarrow$ Backend (Spring Boot) $\rightarrow$ AWS Step Functions $\rightarrow$ Amazon Bedrock $\rightarrow$ Final UI Response.

#### 1. User Authentication
Access the application via the Frontend portal. At the login screen, authenticate using a registered account (securely provisioned and stored in AWS RDS) to obtain a valid Session/Token. This guarantees that all subsequent AI invocations are properly identified and strictly authorized.

![](/images/5-Workshop/5.6/28.png)

#### 2. Workspace Dashboard & Project Initialization
Upon successful login, the system redirects the user to the Project Management dashboard. Here, the history of previous Unit Test generation tasks is fetched from the database. Click **Tạo dự án mới** (Create New Project) to initiate a new workflow.

![](/images/5-Workshop/5.6/29.png)

#### 3. Importing Source Code
The system offers two flexible methods for ingesting source code: uploading a local `.zip` file or connecting directly via a repository URL (GitHub/GitLab). Once you trigger the import, the Backend seamlessly uploads the payload to an AWS S3 Bucket, establishing the RAG foundation for the AI model.

![](/images/5-Workshop/5.6/30.png)

#### 4. Context Selection and Prompting
Once the source code is successfully extracted and parsed, a built-in mini IDE workspace appears:
* **Action 1:** Select the specific file or function requiring a Unit Test from the left-hand file tree. The system extracts this specific code block as the primary Context.
* **Action 2:** Input your prompt into the ZeroBug-Agent chat interface (e.g., *"Write a unit test for this function"*). Hitting send officially triggers the underlying AWS Step Functions state machine.

![](/images/5-Workshop/5.6/31.png)

#### 5. AI Generation & Result Verification
Following a brief background execution across the serverless pipeline (File Identification $\rightarrow$ RAG Context Building $\rightarrow$ Bedrock Model Invocation $\rightarrow$ History Logging), the Agent streams the generated Unit Test code directly into the chat UI. The output is cleanly formatted and ready to be copied and integrated into the developer's local IDE.

![](/images/5-Workshop/5.6/32.png)

**Conclusion:** The Handoff process is a complete success. The AI-integrated Serverless architecture is now fully operational and ready for real-world deployment!