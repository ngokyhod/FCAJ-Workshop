---
title: "AWS Step Functions"
date: 2026-07-03
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---

Now that we have built 5 independent Lambda functions, we need an "orchestrator" to chain them into an automated process. This is exactly where **AWS Step Functions** comes into play.

#### 1. Building the Workflow (State Machine)
Instead of having the Spring Boot application (on EC2) sequentially invoke each Lambda function and manage the state itself, we offload this entire heavy lifting to Step Functions.

The workflow is designed as a linear pipeline:
`Import Project` → `Build Context (RAG)` → `File Tree` → `AI Invoke` → `Result And History`.

![](/images/5-Workshop/5.6/16.png)

#### 2. Fault Tolerance (Error Handling & Retry)
Communicating with AI models (LLMs) often involves high latency or temporary network failures (Timeouts/Rate Limits). In Step Functions, we configure a dedicated **Retrier** mechanism for the `AI Invoke` step:

* **Errors:** Catches `States.Timeout` and `States.TaskFailed`.
* **Interval:** Waits 2 seconds before the first retry attempt.
* **Max attempts:** Allows a maximum of 3 retries.
* **Backoff rate (2.0):** Doubles the wait time after each failure (2s → 4s → 8s) to implement exponential backoff and reduce load on the AI system.

![](/images/5-Workshop/5.6/17.png)
