---
title: "Introduction"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### ZeroBug Agent overview

An AWS-based AI platform that generates Unit Tests (Java/JUnit, Python/pytest, .NET/xUnit). Import Git/Zip → IDE → Generate Test via **Amazon Bedrock Mantle** (`openai.gpt-oss-120b`).

#### 5-member assignment — 1 shared account

| Member | Block | Workshop section |
| --- | --- | --- |
| Trí | IAM + VPC + NAT + SG | [5.3](../5.3-tv1-iam-vpc/) |
| Kiệt | S3 + RDS + DB Secrets + pgvector + Bedrock model access | [5.4](../5.4-tv2-data-secrets/) |
| Toàn | EC2 Spring Boot + ALB | [5.5](../5.5-TV3-ec2-alb/) |
| Hoa | Lambda + Step Functions + Bedrock Mantle | [5.6](../5.6-TV4-serverless/) |
| Trinh | Cognito + API GW + CloudFront + WAF | [5.7](../5.7-tv5-auth-edge/) |

#### Production flow

```
Client → CloudFront → WAF → API Gateway (JWT) → ALB → EC2
  → Step Functions
    → ProjectImportLambda → S3
    → ContextBuilderLambda (embed + pgvector on RDS)
    → BedrockInvokeLambda (chat Mantle)
    → ResultServiceLambda / HistoryServiceLambda → RDS
```

#### Important changes

| | Old idea | Workshop |
| --- | --- | --- |
| AI chat | Bedrock Claude | **Bedrock Mantle** (`openai.gpt-oss-120b`, `us-east-1`) |
| AI context | — | **Compact RAG (A):** embed `cohere.embed-multilingual-v3` → pgvector `code_embeddings` on RDS |
| DNS | Route 53 | **CloudFront `*.cloudfront.net`** |
| Trigger | — | EC2 → Step Fn directly (no SQS) |

{{% notice warning %}}
NAT Gateway is **not free tier**. Delete when done — [5.10](../5.10-cleanup/).
{{% /notice %}}

<!-- Image: /images/5-Workshop/5.1-Workshop-overview/diagram1.png -->
