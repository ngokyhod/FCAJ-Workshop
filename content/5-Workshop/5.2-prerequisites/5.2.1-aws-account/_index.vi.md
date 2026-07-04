---
title: "Tài khoản AWS"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.2.1. </b> "
---

#### Dùng chung 1 tài khoản AWS

Cả 5 thành viên **bắt buộc** dùng **cùng 1 AWS account** vì các phần tham chiếu trực tiếp lẫn nhau:

- VPC / Subnet / Security Group — RDS, EC2, Lambda phải cùng VPC.
- IAM Role — không dùng chéo account (trừ khi cấu hình cross-account phức tạp).
- Secrets Manager, S3 — ARN nội bộ account.
- ALB DNS, Step Functions ARN — integration nội bộ.

| Vấn đề | Giải pháp |
| --- | --- |
| Phân quyền | IAM User riêng `tri`, `kiet`, `toan`, `hoa`, `trinh`, group `zerobug-team` |
| Trùng tên | Prefix `zerobug-*` |
| Chi phí | 1 AWS Budgets alert |
| Bàn giao | 1 bảng tham số chung (mục 5.2.3) |

#### Nếu đã làm bằng Root

- **Không hỏng**, **không làm lại** tài nguyên.
- Bật **MFA cho Root** ngay.
- **Không** tạo Access Key cho Root.
- Tạo IAM User → từ đó chỉ đăng nhập bằng IAM User.

#### Chọn Region

1. [AWS Console](https://console.aws.amazon.com/) → góc trên phải.
2. Chọn **Asia Pacific (Singapore) `ap-southeast-1`**.

{{% notice note %}}
WAF gắn CloudFront dùng **ap-southeast-1**. IAM là global.
{{% /notice %}}

<!-- Hình: /images/5-Workshop/5.2-Prerequisite/region.png -->
