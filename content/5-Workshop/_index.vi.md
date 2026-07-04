---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# ZeroBug Agent – Triển khai hệ thống trên AWS (Workshop gộp chung)

#### Tổng quan

Workshop này hướng dẫn **5 thành viên** triển khai **một hệ thống ZeroBug Agent hoàn chỉnh** trên **cùng một tài khoản AWS**. Mỗi thành viên phụ trách một khối hạ tầng; sau khi hoàn thành, các phần được **gộp lại** thành một luồng end-to-end.

**Thay đổi so với thiết kế ban đầu:**

- **AI chat:** **Amazon Bedrock Mantle** (`openai.gpt-oss-120b`) — Lambda ở `ap-southeast-1`, gọi cross-region qua NAT.
- **AI context (RAG gọn):** embed `cohere.embed-multilingual-v3` → lưu pgvector trên RDS (`code_embeddings`) — `ContextBuilderLambda` trước khi chat.
- **DNS:** **không dùng Route 53**; dùng **CloudFront domain mặc định** (`*.cloudfront.net`).
- **Orchestration:** EC2 gọi trực tiếp **Step Functions** (không SQS).

#### Luồng triển khai theo thứ tự

```
Trí (Nền tảng IAM + VPC)
  → Kiệt (S3 + RDS + Secrets DB + pgvector + Bedrock model access)
    → Toàn (EC2 Spring Boot + ALB)
      → Hoa (Lambda + Step Functions + Bedrock Mantle / RAG)
        → Trinh (Cognito + API Gateway + CloudFront + WAF)
```

#### Luồng truy cập production

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

#### Phân công thành viên

| Thành viên | Khối phụ trách | Mục |
| --- | --- | --- |
| **Trí** | IAM, VPC, Subnet, NAT, Security Groups | [5.3](5.3-tv1-iam-vpc/) |
| **Kiệt** | S3, RDS, Secrets DB, pgvector, Bedrock model access | [5.4](5.4-tv2-data-secrets/) |
| **Toàn** | EC2 (Spring Boot), ALB, Target Group | [5.5](5.5-TV3-ec2-alb/) |
| **Hoa** | Lambda, Step Functions (Bedrock Mantle) | [5.6](5.6-TV4-serverless/) |
| **Trinh** | Cognito, API Gateway, CloudFront, WAF | [5.7](5.7-tv5-auth-edge/) |

#### Nội dung

1. [Giới thiệu](5.1-Workshop-overview/)
2. [Chuẩn bị chung](5.2-prerequisites/)
3. [Trí – Nền tảng IAM & VPC](5.3-tv1-iam-vpc/)
4. [Kiệt – Lớp dữ liệu & bí mật](5.4-tv2-data-secrets/)
5. [Toàn – Backend Compute: EC2 & ALB](5.5-TV3-ec2-alb/)
6. [Hoa – Serverless: Lambda & Step Functions](5.6-TV4-serverless/)
7. [Trinh – Xác thực & lớp Edge](5.7-tv5-auth-edge/)
8. [Kiểm thử end-to-end](5.8-end-to-end-testing/)
9. [Giám sát & vận hành](5.9-monitoring-operations/)
10. [Dọn dẹp tài nguyên](5.10-cleanup/)

{{% notice note %}}
Cả nhóm dùng **chung 1 tài khoản AWS**, mỗi người có **IAM User riêng** (`tri`, `kiet`, `toan`, `hoa`, `trinh`). Duy trì **một bảng tham số chung** (VPC ID, Subnet, SG, Role ARN, S3, RDS endpoint, Mantle URL/model, ALB DNS, CloudFront domain…) để bàn giao giữa các thành viên.
{{% /notice %}}
