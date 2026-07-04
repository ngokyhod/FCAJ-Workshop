---
title: "Kiểm tra & bàn giao"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.4.6. </b> "
---

#### Checklist

- [x] S3 private, upload OK.
- [x] RDS Available, Public access = No, DB `zerobug`.
- [x] Secret DB + ARN đã copy.
- [x] Extension **`vector`** và bảng **`code_embeddings`** đã tạo trên RDS ([5.4.4](../5.4.4-schema-jpa/)).
- [x] Bedrock Console **`us-east-1`**: **Access granted** cho `openai.gpt-oss-120b` **và** `cohere.embed-multilingual-v3`.
- [x] Bảng tham số đã có Mantle URL, model chat/embedding, tên bảng vector.

#### Bàn giao

| Nhận | Cần gì |
| --- | --- |
| **Toàn** | S3 bucket, RDS endpoint, Secret ARN DB |
| **Hoa** | S3, Secret DB, RDS endpoint, Mantle URL + model chat/embedding, bảng `code_embeddings`, RAG top-k |

**Hoa** dùng cùng **`BEDROCK_MANTLE_API_KEY`** trên **`ContextBuilderLambda`** (embed/retrieve) và **`BedrockInvokeLambda`** (chat).

→ Tiếp theo: [Toàn — EC2 & ALB](../../5.5-TV3-ec2-alb/).
