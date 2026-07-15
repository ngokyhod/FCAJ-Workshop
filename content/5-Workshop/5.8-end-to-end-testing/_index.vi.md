---
title: "Kiểm thử end-to-end"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

**Phần chung — cả nhóm** thực hiện sau khi **Toàn** (EC2 + ALB healthy) và **Hoa** (Step Functions + Lambda) hoàn tất. **Trinh** có thể dựng edge trước; một số test vẫn trả **502/401** cho đến khi backend + serverless chạy — bình thường.

#### Mục tiêu

Xác nhận luồng ZeroBug Agent chạy trọn vẹn trên **một AWS account**:

```
Client (SPA / Electron)
  → CloudFront → WAF → API Gateway (JWT)
    → ALB → EC2 (Spring Boot)
      → Step Functions
        → ProjectImportLambda → S3
        → ContextBuilderLambda (embed + pgvector retrieve)
        → BedrockInvokeLambda (chat Mantle)
        → ResultServiceLambda / HistoryServiceLambda → RDS
```

#### Điều kiện trước khi test

| Thành viên | Phải xong |
| --- | --- |
| Trí | NAT Available; route private → NAT; SG outbound EC2/Lambda |
| Kiệt | S3, RDS, Secret DB; pgvector + `code_embeddings`; Bedrock model access (chat + embedding) |
| Toàn | JAR chạy systemd; Target Group **healthy**; ALB DNS đã copy |
| Hoa | Step Fn chạy success; 6 Lambda deploy; Mantle HTTP **200** (test invoke) |
| Trinh | Cognito, API GW `prod`, CloudFront **Deployed**, WAF gắn |

Tham số lấy từ [bảng chung](../5.2-prerequisites/5.2.3-parameter-table/): CloudFront domain, Cognito client, ALB DNS, Step Functions ARN.

#### Tiêu chí “pass” workshop

- [x] Đăng nhập Cognito → JWT hợp lệ ([5.8.2](5.8.2-jwt-authentication/))
- [x] `GET /api/health` qua CloudFront → **200**
- [x] Import project → file trên S3; vector trong `code_embeddings`
- [x] **Generate Test** → Step Functions **Succeeded**; kết quả Unit Test hiển thị trên UI
- [x] `GET /api/aws/status` báo RDS, S3, Bedrock Mantle OK

#### Demo hệ thống

| | Nội dung |
| --- | --- |
| **Link truy cập web** | [ZeroBug-Agent Web](http://54.179.132.145:8080/) |
| **Video demo** | Xem bên dưới hoặc tải [Demo.mp4](/videos/Demo.mp4) |

<video controls width="100%" style="max-width: 960px; border-radius: 8px;">
  <source src="/videos/Demo.mp4" type="video/mp4">
  Trình duyệt không hỗ trợ phát video. Tải file tại <a href="/videos/Demo.mp4">/videos/Demo.mp4</a>.
</video>

#### Nội dung

1. [Checklist theo thành viên](5.8.1-checklist/)
2. [Kiểm tra xác thực JWT](5.8.2-jwt-authentication/)
3. [Kịch bản nghiệp vụ](5.8.3-business-scenarios/)
4. [Xử lý lỗi thường gặp](5.8.4-troubleshooting/)

{{% notice tip %}}
Gặp lỗi → tra [5.8.4](5.8.4-troubleshooting/) theo triệu chứng; debug sâu hơn xem [5.9](../5.9-monitoring-operations/). Sau khi pass, chuyển [5.10 — Dọn dẹp](../5.10-cleanup/).
{{% /notice %}}
