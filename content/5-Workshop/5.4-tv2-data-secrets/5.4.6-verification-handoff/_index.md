---
title: "Verification & Handoff"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.4.6. </b> "
---

#### Checklist

- [x] S3 private, upload OK.
- [x] RDS Available, Public access = No, DB `zerobug`.
- [x] DB Secret + ARN copied.
- [x] Extension **`vector`** and table **`code_embeddings`** created on RDS ([5.4.4](../5.4.4-schema-jpa/)).
- [x] Bedrock Console **`us-east-1`**: **Access granted** for `openai.gpt-oss-120b` **and** `cohere.embed-multilingual-v3`.
- [x] Parameter table has Mantle URL, chat/embedding models, vector table name.

#### Handoff

| Recipient | Needs |
| --- | --- |
| **Toàn** | S3 bucket, RDS endpoint, DB Secret ARN |
| **Hoa** | S3, DB Secret, RDS endpoint, Mantle URL + chat/embedding models, `code_embeddings` table, RAG top-k |

**Hoa** uses the same **`BEDROCK_MANTLE_API_KEY`** on **`ContextBuilderLambda`** (embed/retrieve) and **`BedrockInvokeLambda`** (chat).

→ Next: [Toàn — EC2 & ALB](../../5.5-TV3-ec2-alb/).
