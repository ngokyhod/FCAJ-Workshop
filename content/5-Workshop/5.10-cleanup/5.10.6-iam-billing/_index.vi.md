---
title: "IAM & Billing"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.10.6. </b> "
---

Bước cuối — dọn **IAM**, **Budgets/SNS** (nếu tạo ở mục 5.9) và xác nhận **không còn phí theo giờ**.

#### Bước 1 — IAM Access Keys

1. **IAM** → **Users** → từng user `tri`, `kiet`, `toan`, `hoa`, `trinh`.
2. Tab **Security credentials** → **Access keys** → **Deactivate** / **Delete** (nếu từng tạo CLI key).

#### Bước 2 — IAM Group & Inline Policy

1. **User groups** → `zerobug-team`.
2. **Permissions** → detach **`PowerUserAccess`**.
3. Xóa inline policy **`zerobug-ec2-passrole`**.
4. **Remove users** khỏi group → **Delete group** `zerobug-team`.

#### Bước 3 — IAM Users (tùy chọn)

Nếu account chỉ dùng cho workshop:

1. **Users** → xóa lần lượt `tri`, `kiet`, `toan`, `hoa`, `trinh`.
2. Xác nhận không còn MFA device / login profile cần gỡ trước.

{{% notice note %}}
Giữ lại IAM User nếu account dùng cho mục đích khác — chỉ xóa Access Key và group ZeroBug.
{{% /notice %}}

#### Bước 4 — AWS Budgets & SNS (mục 5.9)

1. **AWS Budgets** → xóa budget cảnh báo ZeroBug.
2. **SNS** → **Subscriptions** — unsubscribe email test.
3. **SNS** → **Topics** — xóa topic alarm (nếu tạo).
4. **CloudWatch** → **Alarms** — xóa alarm ZeroBug.

#### Bước 5 — Kiểm tra billing

1. **Billing** → **Bills** — tháng hiện tại: không còn dòng **NAT Gateway**, **Elastic IP**, **RDS**, **EC2 running**, **WAF**, **CloudFront** *(có thể còn vài giờ prorate)*.
2. **Cost Explorer** → filter service — xác nhận $0 running sau 24–48h.
3. **Bedrock** → **API keys** (region `us-east-1`) → revoke/delete **key workshop** *(một key dùng chung env `BEDROCK_MANTLE_API_KEY` trên **`ContextBuilderLambda`** và **`BedrockInvokeLambda`*)*.

#### Checklist tổng thể (cả nhóm)

| Thành viên | Đã xóa |
| --- | --- |
| Trinh | CloudFront, WAF, API GW, Cognito |
| Hoa | Step Functions, 6 Lambda, log groups |
| Toàn | ALB, Target Group, EC2, key pair |
| Kiệt | RDS, subnet group, Secrets, S3 |
| Trí | NAT, EIP, IGW, SG, subnet, VPC, roles |
| Chung | IAM group/users/keys, Budgets, Alarms |

{{% notice success %}}
Khi checklist đủ, workshop ZeroBug Agent trên AWS đã được dọn sạch.
{{% /notice %}}
