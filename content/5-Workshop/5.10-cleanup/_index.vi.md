---
title: "Dọn dẹp tài nguyên"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

**Bắt buộc sau workshop** — xóa tài nguyên theo thứ tự **ngược** triển khai để tránh dependency error và **tiết kiệm chi phí** (NAT, WAF, RDS, EC2 chạy theo giờ).

#### Thứ tự xóa (cả nhóm)

```
Trinh (CloudFront, WAF, API GW, Cognito)
  → Hoa (Step Functions → 6 Lambda → log groups)
    → Toàn (ALB, Target Group, EC2)
      → Kiệt (RDS → subnet group → Secrets → S3 empty + delete bucket)
        → Trí (NAT → EIP → IGW → SG → subnet → VPC → IAM roles)
          → Chung: IAM users/group, Budgets, revoke Bedrock API key
```

| Bước | Thành viên | Tài nguyên chính | Lưu ý |
| --- | --- | --- | --- |
| 1 | Trinh | CloudFront, WAF, API Gateway, Cognito | WAF Global — đợi disassociate |
| 2 | Hoa | State machine, 6 Lambda PascalCase | Xóa Step Fn **trước** Lambda |
| 3 | Toàn | ALB, Target Group, EC2 | Terminate EC2 trước delete ALB |
| 4 | Kiệt | RDS, DB subnet group, Secret, S3 | S3 phải **empty** mới delete bucket |
| 5 | Trí | NAT, EIP, IGW, 4 SG, VPC | NAT + EIP tốn phí — xóa sớm nhất có thể |
| 6 | Chung | IAM group/users/keys, Budgets, Bedrock key | Revoke **một** API key dùng chung 2 Lambda Mantle |

#### Checklist trước khi kết thúc

- [x] Không còn EC2 **running**, RDS **Available**, NAT **Available**
- [x] S3 bucket ZeroBug đã empty và deleted
- [x] Không còn Lambda / Step Functions ZeroBug
- [x] CloudFront distribution **Disabled** rồi deleted
- [x] Bedrock API key workshop đã revoke (`us-east-1`)
- [x] Billing tháng hiện tại: không còn dòng NAT, EIP, RDS, EC2 *(có thể prorate vài giờ)*

#### Nội dung

1. [Trinh — Edge & Auth](5.10.1-trinh/)
2. [Hoa — Serverless](5.10.2-hoa/)
3. [Toàn — EC2 & ALB](5.10.3-toan/)
4. [Kiệt — Data](5.10.4-kiet/)
5. [Trí — VPC & NAT](5.10.5-tri/)
6. [IAM & xác nhận billing](5.10.6-iam-billing/)

{{% notice warning %}}
**NAT Gateway + Elastic IP** và **WAF** tốn phí nhất — nếu chỉ tạm dừng một đêm, vẫn nên xóa NAT hoặc terminate EC2/RDS. Không để NAT chạy qua tuần sau khi hết workshop.
{{% /notice %}}

{{% notice tip %}}
Tên Lambda cleanup khớp [bảng tham số](../5.2-prerequisites/5.2.3-parameter-table/): `ProjectImportLambda`, `SourceFileServiceLambda`, `ContextBuilderLambda`, `BedrockInvokeLambda`, `ResultServiceLambda`, `HistoryServiceLambda`.
{{% /notice %}}
