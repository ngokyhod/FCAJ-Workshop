---
title: "Trinh – Xác thực & Edge"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

**Thành viên phụ trách:** Trinh  
**Phụ thuộc Toàn:** **ALB DNS name** (bắt buộc trước API Gateway).

#### Trinh làm gì?

Triển khai **lớp xác thực và edge**: Cognito, API Gateway, CloudFront, WAF — **không Route 53**.

#### Chức năng & dùng ở đâu

| Thành phần | Chức năng | Dùng ở đâu |
| --- | --- | --- |
| **Cognito** | Đăng nhập, JWT | SPA/Electron; API GW Authorizer |
| **API Gateway** | REST + `{proxy+}` → ALB | Điểm vào API client |
| **CloudFront** | CDN HTTPS `*.cloudfront.net` | URL production Frontend |
| **WAF** | Core rules, rate limit | Bảo vệ API GW / CloudFront |

```
Client → CloudFront → WAF → API Gateway (JWT) → ALB → EC2
```

#### Chuẩn bị riêng Trinh

- **ALB DNS** từ Toàn: EC2 → Load Balancers → `zerobug-alb` → DNS name.
- Postman/curl cho test JWT (mục 5.8).
- WAF CloudFront dùng region **Global** nếu chọn Cách B (mục 5.7.4).

{{% notice info %}}
Có thể **dựng full Trinh** khi backend chưa xong. `https://d....cloudfront.net/api/health` trả **502** — bình thường.
{{% /notice %}}

#### Nội dung triển khai

1. [Amazon Cognito](5.7.1-cognito/)
2. [Amazon API Gateway](5.7.2-api-gateway/)
3. [CloudFront Distribution](5.7.3-cloudfront/)
4. [AWS WAF](5.7.4-waf/)
5. [Kiểm tra & bàn giao](5.7.5-verification-handoff/)
