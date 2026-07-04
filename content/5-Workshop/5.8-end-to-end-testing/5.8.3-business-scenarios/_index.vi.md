---
title: "Kịch bản nghiệp vụ"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.8.3. </b> "
---

1. Đăng nhập Cognito.
2. Import project (Git/Zip) → Step Functions → **`ProjectImportLambda`** → S3; **`ContextBuilderLambda`** chunk + embed → pgvector **`code_embeddings`** trên RDS.
3. Mở IDE — duyệt cây thư mục.
4. Generate Test → **`ContextBuilderLambda`** retrieve top-k → **`BedrockInvokeLambda`** (chat Mantle) → **`ResultServiceLambda`** lưu kết quả.
5. Xem lịch sử trên RDS (**`HistoryServiceLambda`** / API EC2).

| Endpoint | Chức năng |
| --- | --- |
| `GET /api/health` | Health |
| `GET /api/projects` | Danh sách project |
| `POST /api/projects/{id}/generate` | Sinh test |
| `GET /api/generations/recent` | Lịch sử |
| `GET /api/aws/status` | Trạng thái RDS/S3/Bedrock Mantle |
