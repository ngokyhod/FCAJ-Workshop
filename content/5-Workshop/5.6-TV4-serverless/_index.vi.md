---
title: "Hoa – Serverless: Lambda & Step Functions"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

**Thành viên phụ trách:** Hoa  
**Phụ thuộc Trí + Kiệt:** NAT Gateway, `zerobug-lambda-sg`, IAM Roles; S3 bucket, RDS endpoint, Secret DB, và các tham số Bedrock Mantle.

#### Hoa làm gì?

Triển khai pipeline AI serverless lõi cho ZeroBug Agent. Công việc bao gồm tạo 6 hàm Lambda độc lập, điều phối chúng bằng một State Machine của AWS Step Functions, và tích hợp với Amazon Bedrock Mantle để thực hiện RAG và sinh mã.

#### Chức năng & dùng ở đâu

| Thành phần | Chức năng | Dùng ở đâu |
| --- | --- | --- |
| **`ProjectImportLambda`** | Xử lý mã nguồn dự án (Git/Zip) và tải lên S3. | Bước đầu của workflow Step Functions. |
| **`ContextBuilderLambda`** | Xây dựng ngữ cảnh RAG: chunk source, tạo embedding qua Mantle, lưu/truy vấn vector trên RDS pgvector. | Step Functions, trước khi gọi AI. |
| **`SourceFileServiceLambda`** | Cung cấp cây thư mục và nội dung file cho IDE. | Step Functions và giao diện IDE. |
| **`BedrockInvokeLambda`** | Gọi model chat của Bedrock Mantle (`openai.gpt-oss-120b`) với ngữ cảnh để sinh unit test. | Bước sinh AI lõi trong workflow. |
| **`ResultServiceLambda`** | Lưu kết quả unit test đã sinh vào RDS. | Cuối workflow. |
| **`HistoryServiceLambda`** | Ghi lại lịch sử lần sinh test vào bảng trong RDS. | Cuối workflow. |
| **Step Functions** | "Nhạc trưởng" xâu chuỗi các hàm Lambda thành một quy trình tự động, có khả năng chịu lỗi. | Được kích hoạt bởi backend EC2 (Toàn). |

#### Thiết kế AI

Toàn bộ quy trình AI được chuyển từ EC2 sang một pipeline serverless do Step Functions điều phối.

- **Workflow:** `EC2 (Toàn) → Step Functions → ProjectImportLambda → S3 → ContextBuilderLambda (embed + pgvector) → BedrockInvokeLambda (chat) → Result/History Lambdas → RDS`
- **Model Chat:** `openai.gpt-oss-120b` qua Mantle (`/v1/chat/completions`).
- **Model Embedding:** `cohere.embed-multilingual-v3` qua Mantle (`/v1/embeddings`).
- **Xác thực:** Dùng Bearer token (`BEDROCK_MANTLE_API_KEY`) lưu trên biến môi trường của `ContextBuilderLambda` và `BedrockInvokeLambda`.
- **Mạng:** Các Lambda chạy trong private subnet và gọi API cross-region (`ap-southeast-1` → `us-east-1`) đến endpoint của Bedrock Mantle thông qua NAT Gateway (Trí).

#### Chuẩn bị riêng Hoa

- Thu thập các tham số cần thiết (VPC, Subnet, SG, Role, S3, RDS, Mantle URL/model) từ bảng tham số chung.
- Chạy `build-all.bat` trên máy cá nhân để tạo các gói artifact deploy cho từng hàm Lambda.
- Tạo một Bedrock API key trên Console ở region **`us-east-1`** và chuẩn bị sẵn để cấu hình biến môi trường `BEDROCK_MANTLE_API_KEY` trên Lambda.

#### Nội dung triển khai

1. Khởi tạo AWS Lambda
2. Tích hợp AWS Step Functions
3. Cấu hình EC2 & Kích hoạt Workflow
4. Giám sát & Gỡ lỗi
5. Kiểm thử toàn trình (Verifying Handoff)