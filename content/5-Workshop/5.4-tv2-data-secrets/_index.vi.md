---
title: "Kiệt – Lớp dữ liệu & bí mật"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

**Thành viên phụ trách:** Kiệt  
**Phụ thuộc Trí:** Private Subnet A/B, `zerobug-rds-sg`, ARN `zerobug-ec2-role` & `zerobug-lambda-role`.

#### Kiệt làm gì?

Triển khai **lớp lưu trữ và bí mật**: S3, RDS PostgreSQL, Secrets Manager (DB) + **bật model access Bedrock Mantle** (mục 5.4.5).

#### Chức năng & dùng ở đâu

| Thành phần | Chức năng | Dùng ở đâu |
| --- | --- | --- |
| **Amazon S3** | Lưu source + prefix `deploy/` cho JAR | Lambda (Hoa); EC2 kéo JAR (Toàn) |
| **Amazon RDS** | Metadata, kết quả test, lịch sử, vector RAG (`code_embeddings`) | EC2 JPA; Lambda Result/History/Context Builder (Hoa) |
| **Secrets (DB)** | Credential RDS | EC2 + Lambda runtime |
| **Bedrock Mantle** | Model access chat + embedding (`us-east-1`) | `ContextBuilderLambda` (embed) + `BedrockInvokeLambda` (chat) — key trên env Lambda |

#### Thiết kế AI (RAG gọn — phương án A)

- **Chat:** Mantle **`/v1/chat/completions`**, model `openai.gpt-oss-120b`.
- **Embedding:** Mantle **`/v1/embeddings`**, model `cohere.embed-multilingual-v3` — vector lưu RDS **pgvector** (`code_embeddings`).
- **`ContextBuilderLambda`** (Hoa): đọc S3 → chunk → embed → lưu/retrieve top-k → bàn giao `context` cho **`BedrockInvokeLambda`**.
- Lambda deploy **`ap-southeast-1`**, gọi Mantle **cross-region** qua NAT.
- Bảng ứng dụng (`users`, `projects`, …): **Toàn** tạo qua JPA; bảng vector: **Kiệt** bật extension pgvector ([5.4.4](5.4.4-schema-jpa/)).

#### Chuẩn bị riêng Kiệt

- Bật **Model access** trên Bedrock Console region **`us-east-1`** (chi tiết [5.4.5](5.4.5-bedrock-mantle/)).
- Tham số Kiệt điền vào [bảng chung](../5.2-prerequisites/5.2.3-parameter-table/):

| Tham số | Ví dụ |
| --- | --- |
| S3 Bucket | `zerobug-projects-hutech01` |
| RDS Endpoint | `zerobug-db.xxxx.rds.amazonaws.com` |
| DB name | `zerobug` |
| Secret DB | `zerobug/rds/credentials` |
| Mantle base URL | `https://bedrock-mantle.us-east-1.api.aws` |
| Model ID (chat) | `openai.gpt-oss-120b` |
| Model ID (embedding) | `cohere.embed-multilingual-v3` |
| Vector table | `code_embeddings` |

#### Nội dung triển khai

1. [Thiết lập Amazon S3](5.4.1-s3/)
2. [Triển khai RDS PostgreSQL](5.4.2-rds/)
3. [AWS Secrets Manager](5.4.3-secrets-manager/)
4. [Khởi tạo schema (JPA)](5.4.4-schema-jpa/)
5. [Bedrock Mantle — model access](5.4.5-bedrock-mantle/)
6. [Kiểm tra & bàn giao](5.4.6-verification-handoff/)
