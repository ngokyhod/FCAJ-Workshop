---
title: "Giới thiệu"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Tổng quan ZeroBug Agent

Nền tảng AI trên AWS sinh Unit Test (Java/JUnit, Python/pytest, .NET/xUnit). Import Git/Zip → IDE → Generate Test qua **Amazon Bedrock Mantle** (`openai.gpt-oss-120b`).

#### Phân công 5 thành viên — 1 account chung

| Thành viên | Khối | Mục workshop |
| --- | --- | --- |
| Trí | IAM + VPC + NAT + SG | [5.3](../5.3-tv1-iam-vpc/) |
| Kiệt | S3 + RDS + Secrets DB + pgvector + Bedrock model access | [5.4](../5.4-tv2-data-secrets/) |
| Toàn | EC2 Spring Boot + ALB | [5.5](../5.5-TV3-ec2-alb/) |
| Hoa | Lambda + Step Functions + Bedrock Mantle | [5.6](../5.6-TV4-serverless/) |
| Trinh | Cognito + API GW + CloudFront + WAF | [5.7](../5.7-tv5-auth-edge/) |

#### Luồng production

```
Client → CloudFront → WAF → API Gateway (JWT) → ALB → EC2
  → Step Functions
    → ProjectImportLambda → S3
    → ContextBuilderLambda (embed + pgvector trên RDS)
    → BedrockInvokeLambda (chat Mantle)
    → ResultServiceLambda / HistoryServiceLambda → RDS
```

#### Thay đổi quan trọng

| | Ý tưởng cũ | Workshop |
| --- | --- | --- |
| AI chat | Bedrock Claude | **Bedrock Mantle** (`openai.gpt-oss-120b`, `us-east-1`) |
| AI context | — | **RAG gọn (A):** embed `cohere.embed-multilingual-v3` → pgvector `code_embeddings` trên RDS |
| DNS | Route 53 | **CloudFront `*.cloudfront.net`** |
| Trigger | — | EC2 → Step Fn trực tiếp (không SQS) |

{{% notice warning %}}
NAT Gateway **không free tier**. Xóa khi xong — [5.10](../5.10-cleanup/).
{{% /notice %}}

<!-- Hình: /images/5-Workshop/5.1-Workshop-overview/diagram1.png -->
