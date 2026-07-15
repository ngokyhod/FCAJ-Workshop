---
title: "Resource Cleanup – Hoa"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.10.2. </b> "
---

Delete **Step Functions** before **Lambda** (state machine references Lambda). Delete **CloudWatch Log groups** for Lambda last in this section.

Function names per the [parameter table](../../5.2-prerequisites/5.2.3-parameter-table/) — **PascalCase**, no `zerobug-` prefix for Lambda.

#### Step 1 — Step Functions State Machine

1. **Step Functions** → **State machines**.
2. Select ZeroBug state machine (e.g. `zerobug-workflow` or team-chosen name).
3. **Stop** any **Running** executions (tab **Executions**).
4. **Delete state machine**.

#### Step 2 — Lambda Functions (6 functions)

**Lambda** → **Functions** — delete one by one:

| Function | Notes |
| --- | --- |
| `ProjectImportLambda` | Project Import |
| `SourceFileServiceLambda` | Source File Service |
| `ContextBuilderLambda` | Context Builder + RAG embed/retrieve |
| `BedrockInvokeLambda` | AI Invoke (Bedrock Mantle chat) |
| `ResultServiceLambda` | Result Service |
| `HistoryServiceLambda` | History Service |

For each function:

1. **Configuration** → **Triggers** → remove all triggers (if any).
2. **Actions** → **Delete** → type `delete` to confirm.

{{% notice tip %}}
If **Delete** fails due to Step Functions: return to Step 1, ensure state machine is deleted.
{{% /notice %}}

#### Step 3 — Lambda Layers / Versions (if any)

1. **Lambda** → **Layers** — delete custom ZeroBug layer (if created).
2. Tab **Versions** on each function — usually removed automatically when function is deleted.

#### Step 4 — CloudWatch Log groups

**CloudWatch** → **Log groups** → delete:

- `/aws/lambda/ProjectImportLambda`
- `/aws/lambda/SourceFileServiceLambda`
- `/aws/lambda/ContextBuilderLambda`
- `/aws/lambda/BedrockInvokeLambda`
- `/aws/lambda/ResultServiceLambda`
- `/aws/lambda/HistoryServiceLambda`
- `/aws/states/zerobug-workflow` *(or state machine prefix)*

#### Step 5 — EventBridge / SNS (if Hoa created extras)

Delete rule/schedule for test Lambda invoke (if added in extended workshop).

#### Confirmation checklist

- [ ] No ZeroBug state machine remaining
- [ ] No 6 Lambda functions remaining
- [ ] Lambda/Step Functions log groups deleted

→ Next: [Toàn — EC2 & ALB](5.10.3-toan/)
