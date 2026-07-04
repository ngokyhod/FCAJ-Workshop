---
title: "IGW & NAT"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.3.5. </b> "
---

#### Internet Gateway

1. **Internet gateways** → **Create internet gateway** → Name: `zerobug-igw` → **Create**.
2. Chọn IGW → **Actions** → **Attach to VPC** → `zerobug-vpc` → **Attach**.

![](/images/5-Workshop/5.3/31.jpg)
![](/images/5-Workshop/5.3/32.png)
![](/images/5-Workshop/5.3/33.png)

#### NAT Gateway

NAT cho phép subnet **private** ra internet: EC2/SSM, tải artifact S3, và Lambda VPC (**`ContextBuilderLambda`**, **`BedrockInvokeLambda`**) gọi **`bedrock-mantle.us-east-1.api.aws`** — embed + chat cross-region từ `ap-southeast-1`.

1. **NAT gateways** → **Create NAT gateway**.
2. Name: `zerobug-nat`.
3. **Subnet:** `zerobug-public-a` *(NAT phải ở public subnet)*.
4. **Connectivity:** Public.
5. **Elastic IP:** **Allocate Elastic IP**.
6. → **Create**. Đợi **Available** (vài phút).
7. Copy **NAT Gateway ID**.

![](/images/5-Workshop/5.3/34.png)
![](/images/5-Workshop/5.3/35.png)

{{% notice warning %}}
**NAT không free tier** (~0.045 USD/giờ). Chỉ tạo **1 NAT** (không mỗi AZ một cái). Xóa + release Elastic IP khi xong (mục 5.10).
{{% /notice %}}

<!-- Hình: /images/5-Workshop/5.3-Trí/nat-gateway.png -->
