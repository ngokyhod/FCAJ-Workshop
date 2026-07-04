---
title: "Business Scenarios"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.8.3. </b> "
---

1. Sign in Cognito.
2. Import project (Git/Zip) → Step Functions → **`ProjectImportLambda`** → S3; **`ContextBuilderLambda`** chunk + embed → pgvector **`code_embeddings`** on RDS.
3. Open IDE — browse folder tree.
4. Generate Test → **`ContextBuilderLambda`** retrieve top-k → **`BedrockInvokeLambda`** (chat Mantle) → **`ResultServiceLambda`** saves result.
5. View history on RDS (**`HistoryServiceLambda`** / EC2 API).

| Endpoint | Function |
| --- | --- |
| `GET /api/health` | Health |
| `GET /api/projects` | Project list |
| `POST /api/projects/{id}/generate` | Generate test |
| `GET /api/generations/recent` | History |
| `GET /api/aws/status` | RDS/S3/Bedrock Mantle status |
