---
title: "Bedrock Mantle"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---

#### Bước 1 — Lấy API Key từ Model catalog (Region Mantle)

1. Console → **Amazon Bedrock** → chuyển region sang **`us-east-1` (N. Virginia)** *(Region của Mantle endpoint)*.
2. Vào **Model catalog** → tìm các model mà bạn sẽ tích hợp:
   - **`openai.gpt-oss-120b`** — Sinh Unit Test (Chat Completions)
   - **`cohere.embed-multilingual-v3`** — **Bắt buộc** cho luồng RAG (Embeddings)
3. Tạo và copy trực tiếp **API Key** từ giao diện của model. Key này sẽ được sử dụng làm Bearer token trong các hàm Lambda của bạn.

{{% notice info %}}
Workshop triển khai VPC/Lambda ở **`ap-southeast-1`**, nhưng gọi inference (suy luận AI) thông qua **`bedrock-mantle.us-east-1.api.aws`** — **xuyên vùng (cross-region)**. Ghi chép rõ điều này vào [bảng tham số](../../5.2-prerequisites/5.2.3-parameter-table/).
{{% /notice %}}

![](/images/5-Workshop/5.4/30.png)

#### Bước 2 — Tham số bàn giao cho Hoa

| Tham số | Giá trị |
| --- | --- |
| Mantle base URL | `https://bedrock-mantle.us-east-1.api.aws` |
| Chat Completions path | `/v1/chat/completions` |
| Embeddings path | `/v1/embeddings` |
| Model ID (chat) | `openai.gpt-oss-120b` |
| Model ID (embedding) | `cohere.embed-multilingual-v3` |
| Kích thước Embedding | `1024` *(Cohere multilingual v3)* |
| Region của Mantle | `us-east-1` |
| Region của Lambda | `ap-southeast-1` |
| Bảng Vector (RDS) | `code_embeddings` *(pgvector — xem [5.4.4](../5.4.4-schema-jpa/))* |
| RAG top-k | `5` *(Khuyến nghị của workshop)* |

#### Bước 3 — RAG tinh gọn (Option A — Workshop)

Luồng xử lý AI sử dụng một **Context Builder Lambda** (`ContextBuilderLambda`) duy nhất cho tất cả các bước liên quan đến vector — **không** tách riêng Lambda embedding, **không** dùng OpenSearch.

```mermaid
flowchart LR
  S3[Nguồn S3] --> CB[ContextBuilderLambda]
  CB --> EMB["Mantle /v1/embeddings"]
  EMB --> RDS[(RDS pgvector<br/>code_embeddings)]
  CB --> RET[Truy xuất top-k]
  RET --> AI[BedrockInvokeLambda]
  AI --> CHAT["Mantle /v1/chat/completions"]