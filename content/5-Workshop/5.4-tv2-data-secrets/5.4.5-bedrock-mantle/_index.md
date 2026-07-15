---
title: "Bedrock Mantle"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.4.5. </b> "
---

#### Step 1 — Get API Key from Model catalog (Mantle region)

1. Console → **Amazon Bedrock** → switch region to **`us-east-1` (N. Virginia)** *(Mantle endpoint region)*.
2. Go to **Model catalog** → locate the models you are integrating:
   - **`openai.gpt-oss-120b`** — Unit Test generation (Chat Completions)
   - **`cohere.embed-multilingual-v3`** — **required** for RAG pipeline (Embeddings)
3. Directly generate and copy the **API Key** from the model's interface. This key will be used as the Bearer token in your Lambda functions.

{{% notice info %}}
The workshop deploys VPC/Lambda in **`ap-southeast-1`**, but calls inference via **`bedrock-mantle.us-east-1.api.aws`** — **cross-region**. Record this clearly in the [parameter table](../../5.2-prerequisites/5.2.3-parameter-table/).
{{% /notice %}}

![](/images/5-Workshop/5.4/30.png)

#### Step 2 — Handoff parameters for Hoa

| Parameter | Value |
| --- | --- |
| Mantle base URL | `https://bedrock-mantle.us-east-1.api.aws` |
| Chat Completions path | `/v1/chat/completions` |
| Embeddings path | `/v1/embeddings` |
| Model ID (chat) | `openai.gpt-oss-120b` |
| Model ID (embedding) | `cohere.embed-multilingual-v3` |
| Embedding dimension | `1024` *(Cohere multilingual v3)* |
| Mantle region | `us-east-1` |
| Lambda region | `ap-southeast-1` |
| Vector table (RDS) | `code_embeddings` *(pgvector — see [5.4.4](../5.4.4-schema-jpa/))* |
| RAG top-k | `5` *(workshop suggestion)* |

#### Step 3 — Lightweight RAG (option A — workshop)

The AI pipeline uses a single **Context Builder Lambda** (`ContextBuilderLambda`) for all vector steps — **no** separate embedding Lambda, **no** OpenSearch.

```mermaid
flowchart LR
  S3[S3 source] --> CB[ContextBuilderLambda]
  CB --> EMB["Mantle /v1/embeddings"]
  EMB --> RDS[(RDS pgvector<br/>code_embeddings)]
  CB --> RET[Retrieve top-k]
  RET --> AI[BedrockInvokeLambda]
  AI --> CHAT["Mantle /v1/chat/completions"]