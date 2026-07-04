---
title: "Budgets & SNS"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.9.2. </b> "
---

#### AWS Budgets & SNS

- **AWS Budgets:** cost alert when threshold exceeded (NAT, WAF, EC2, RDS).
- **SNS:** email when CloudWatch Alarm fires.
- Track **Amazon Bedrock** (Mantle inference — chat + embedding) in **AWS Billing / Cost Explorer** — same bill as NAT, EC2, RDS.

#### Bedrock API key (Hoa)

- Key created in Bedrock Console → **API keys**, default TTL **~30 days**.
- Both **`ContextBuilderLambda`** and **`BedrockInvokeLambda`** use env **`BEDROCK_MANTLE_API_KEY`** — expiry → **401** on all Mantle calls.
- **Before expiry:** create new key → update env on **both** Lambdas → test invoke → deactivate old key.
- Record rotation schedule in the team (calendar / parameter table) — workshop does not store key in Secrets Manager.

#### Suggested Mantle cost tracking

| Item | Notes |
| --- | --- |
| Chat (`openai.gpt-oss-120b`) | Each Unit Test generation via Step Functions |
| Embeddings (`cohere.embed-multilingual-v3`) | Each chunk on import/re-index source |
| NAT | Egress cross-region `ap-southeast-1` → `us-east-1` |

<!-- Image: /images/5-Workshop/5.9-Monitoring/budgets.png -->
