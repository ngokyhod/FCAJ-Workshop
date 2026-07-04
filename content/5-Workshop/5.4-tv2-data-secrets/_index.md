---
title: "Kiệt – Data & Secrets Layer"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

**Owner:** Kiệt  
**Depends on Trí:** Private Subnet A/B, `zerobug-rds-sg`, ARN `zerobug-ec2-role` & `zerobug-lambda-role`.

#### What does Kiệt do?

Deploy the **storage and secrets layer**: S3, RDS PostgreSQL, Secrets Manager (DB) + **enable Bedrock Mantle model access** (section 5.4.5).

#### Components & where they are used

| Component | Function | Used by |
| --- | --- | --- |
| **Amazon S3** | Store source files + `deploy/` prefix for JAR | Lambda (Hoa); EC2 pulls JAR (Toàn) |
| **Amazon RDS** | Metadata, test results, history, RAG vectors (`code_embeddings`) | EC2 JPA; Lambda Result/History/Context Builder (Hoa) |
| **Secrets (DB)** | RDS credentials | EC2 + Lambda runtime |
| **Bedrock Mantle** | Chat + embedding model access (`us-east-1`) | `ContextBuilderLambda` (embed) + `BedrockInvokeLambda` (chat) — key on Lambda env |

#### AI design (lightweight RAG — option A)

- **Chat:** Mantle **`/v1/chat/completions`**, model `openai.gpt-oss-120b`.
- **Embedding:** Mantle **`/v1/embeddings`**, model `cohere.embed-multilingual-v3` — vectors stored in RDS **pgvector** (`code_embeddings`).
- **`ContextBuilderLambda`** (Hoa): read S3 → chunk → embed → store/retrieve top-k → pass `context` to **`BedrockInvokeLambda`**.
- Lambda deployed in **`ap-southeast-1`**, calls Mantle **cross-region** via NAT.
- Application tables (`users`, `projects`, …): **Toàn** creates via JPA; vector table: **Kiệt** enables pgvector extension ([5.4.4](5.4.4-schema-jpa/)).

#### Kiệt-specific preparation

- Enable **Model access** in Bedrock Console region **`us-east-1`** (details in [5.4.5](5.4.5-bedrock-mantle/)).
- Parameters Kiệt fills in the [shared parameter table](../5.2-prerequisites/5.2.3-parameter-table/):

| Parameter | Example |
| --- | --- |
| S3 Bucket | `zerobug-projects-hutech01` |
| RDS Endpoint | `zerobug-db.xxxx.rds.amazonaws.com` |
| DB name | `zerobug` |
| Secret DB | `zerobug/rds/credentials` |
| Mantle base URL | `https://bedrock-mantle.us-east-1.api.aws` |
| Model ID (chat) | `openai.gpt-oss-120b` |
| Model ID (embedding) | `cohere.embed-multilingual-v3` |
| Vector table | `code_embeddings` |

#### Deployment content

1. [Set up Amazon S3](5.4.1-s3/)
2. [Deploy RDS PostgreSQL](5.4.2-rds/)
3. [AWS Secrets Manager](5.4.3-secrets-manager/)
4. [Initialize schema (JPA)](5.4.4-schema-jpa/)
5. [Bedrock Mantle — model access](5.4.5-bedrock-mantle/)
6. [Verification & handoff](5.4.6-verification-handoff/)
