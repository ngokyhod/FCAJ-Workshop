---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# ZeroBug Agent – Deploy the system on AWS (Combined workshop)

#### Overview

This workshop guides **5 members** through deploying **one complete ZeroBug Agent system** on **the same AWS account**. Each member owns one infrastructure block; when finished, the parts are **combined** into one end-to-end flow.

**Changes from the original design:**

- **AI chat:** **Amazon Bedrock Mantle** (`openai.gpt-oss-120b`) — Lambda in `ap-southeast-1`, cross-region calls via NAT.
- **AI context (compact RAG):** embed `cohere.embed-multilingual-v3` → store pgvector on RDS (`code_embeddings`) — `ContextBuilderLambda` runs before chat.
- **DNS:** **no Route 53**; use the **default CloudFront domain** (`*.cloudfront.net`).
- **Orchestration:** EC2 calls **Step Functions** directly (no SQS).

#### Deployment flow (in order)

```
Trí (IAM + VPC foundation)
  → Kiệt (S3 + RDS + DB Secrets + pgvector + Bedrock model access)
    → Toàn (EC2 Spring Boot + ALB)
      → Hoa (Lambda + Step Functions + Bedrock Mantle / RAG)
        → Trinh (Cognito + API Gateway + CloudFront + WAF)
```

#### Production access flow

```
Client (Vite SPA / Electron)
  → CloudFront (*.cloudfront.net)
    → WAF (Web ACL)
      → API Gateway (JWT Authorizer + Cognito)
        → ALB (Public Subnet)
          → EC2 Spring Boot (Private Subnet)
            → Step Functions
              → ContextBuilderLambda (embed + pgvector)
              → BedrockInvokeLambda (chat Mantle)
              → S3 / RDS
```

#### Member assignments

| Member | Block owned | Section |
| --- | --- | --- |
| **Trí** | IAM, VPC, Subnet, NAT, Security Groups | [5.3](5.3-tv1-iam-vpc/) |
| **Kiệt** | S3, RDS, DB Secrets, pgvector, Bedrock model access | [5.4](5.4-tv2-data-secrets/) |
| **Toàn** | EC2 (Spring Boot), ALB, Target Group | [5.5](5.5-TV3-ec2-alb/) |
| **Hoa** | Lambda, Step Functions (Bedrock Mantle) | [5.6](5.6-TV4-serverless/) |
| **Trinh** | Cognito, API Gateway, CloudFront, WAF | [5.7](5.7-tv5-auth-edge/) |

#### Contents

1. [Introduction](5.1-Workshop-overview/)
2. [Shared prerequisites](5.2-prerequisites/)
3. [Trí – IAM & VPC foundation](5.3-tv1-iam-vpc/)
4. [Kiệt – Data & secrets layer](5.4-tv2-data-secrets/)
5. [Toàn – Backend compute: EC2 & ALB](5.5-TV3-ec2-alb/)
6. [Hoa – Serverless: Lambda & Step Functions](5.6-TV4-serverless/)
7. [Trinh – Authentication & edge layer](5.7-tv5-auth-edge/)
8. [End-to-end testing](5.8-end-to-end-testing/)
9. [Monitoring & operations](5.9-monitoring-operations/)
10. [Resource cleanup](5.10-cleanup/)

{{% notice note %}}
The whole team uses **one shared AWS account**, with each person having their **own IAM User** (`tri`, `kiet`, `toan`, `hoa`, `trinh`). Maintain **one shared parameter table** (VPC ID, Subnet, SG, Role ARN, S3, RDS endpoint, Mantle URL/model, ALB DNS, CloudFront domain…) for handoff between members.
{{% /notice %}}
