---
title: "Trí – Nền tảng IAM & VPC"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

**Thành viên phụ trách:** Trí  
**Làm đầu tiên** — hoàn thành [5.2 Chuẩn bị chung](../5.2-prerequisites/) trước.

#### Trí làm gì?

Dựng **nền tảng bảo mật và mạng** cho toàn bộ ZeroBug Agent trên **một VPC chung**: IAM users/roles, VPC hai lớp subnet, NAT Gateway, route tables và security groups.

#### Chức năng & dùng ở đâu

| Thành phần | Chức năng | Dùng ở đâu |
| --- | --- | --- |
| **IAM Users `tri`–`trinh`** | Đăng nhập Console an toàn | Cả nhóm trên 1 account |
| **IAM Roles (EC2/Lambda/StepFn)** | Quyền runtime từng dịch vụ | EC2 (Toàn), Lambda + Step Fn (Hoa) |
| **VPC + Subnet** | Public (ALB) + Private (EC2, RDS, Lambda) | Toàn bộ hạ tầng ZeroBug |
| **NAT Gateway** | Private subnet ra internet | Lambda VPC (`ContextBuilderLambda`, `BedrockInvokeLambda`) → Mantle `us-east-1`; EC2 tải JAR / SSM |
| **Security Groups** | Kiểm soát traffic | ALB ↔ EC2 ↔ RDS ↔ Lambda |

#### Vai trò trong luồng production

```
Client → CloudFront → WAF → API Gateway → ALB (Public) → EC2 / RDS / Lambda (Private)
```

#### Thiết kế mạng

| Thành phần | Giá trị |
| --- | --- |
| VPC CIDR | `10.0.0.0/16` — `zerobug-vpc` |
| Public Subnet A / B | `10.0.1.0/24` (1a), `10.0.2.0/24` (1b) |
| Private Subnet A / B | `10.0.11.0/24` (1a), `10.0.12.0/24` (1b) |

{{% notice warning %}}
**NAT Gateway không free tier** (~32 USD/tháng). Chỉ tạo **1 NAT**; xóa khi xong (mục 5.10).
{{% /notice %}}

#### Chuẩn bị riêng Trí

- Hoàn thành checklist [5.2.4](../5.2-prerequisites/5.2.4-conventions-and-order/).
- **Lối tắt (tùy chọn):** VPC wizard **"VPC and more"** — nếu dùng, đối chiếu tên/CIDR bảng trên. Các bước bên dưới hướng dẫn **cách thủ công** để kiểm soát tên khi bàn giao.

#### Nội dung triển khai

1. [Tạo IAM Users/Groups](5.3.1-iam-users-groups/)
2. [Tạo IAM Roles dùng chung](5.3.2-iam-roles/)
3. [Tạo VPC](5.3.3-vpc/)
4. [Tạo Public & Private Subnet](5.3.4-subnets/)
5. [Internet Gateway & NAT Gateway](5.3.5-igw-nat/)
6. [Cấu hình Route Tables](5.3.6-route-tables/)
7. [Tạo Security Groups](5.3.7-security-groups/)
8. [Kiểm tra & bàn giao](5.3.8-verification-handoff/)
