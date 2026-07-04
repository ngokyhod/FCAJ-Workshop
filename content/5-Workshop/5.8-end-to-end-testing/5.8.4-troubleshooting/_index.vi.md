---
title: "Xử lý lỗi"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.8.4. </b> "
---

| Triệu chứng | Nguyên nhân | Thành viên |
| --- | --- | --- |
| CloudFront 502 | EC2 chưa chạy / unhealthy | Toàn |
| 401 mọi request | Thiếu/sai JWT | Trinh |
| SSM Offline | Thiếu role / NAT | Trí, Toàn |
| Target Unhealthy | App chưa start | Toàn |
| Bedrock Mantle timeout / 401 | NAT; API key hết hạn; model access | Trí, Hoa, Kiệt |
| Embedding 400 / dimension mismatch | Sai model hoặc vector ≠ 1024; thiếu `input_type` Cohere | Hoa, Kiệt |
| RAG trả context rỗng | Chưa embed (`code_embeddings` trống); sai `project_id`; chưa chạy Context Builder | Hoa |
| `ERROR: extension "vector" does not exist` | Kiệt chưa `CREATE EXTENSION vector` trên RDS | Kiệt |
| Chat chất lượng kém (thiếu ngữ cảnh) | Retrieve top-k quá nhỏ; chunk quá lớn; chưa index vector | Hoa |
| DB fail | SG / secret / endpoint | Kiệt, Toàn |
