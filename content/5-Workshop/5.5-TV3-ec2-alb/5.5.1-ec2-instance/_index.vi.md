---
title: "EC2 Instance"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

1. **EC2** → **Launch instance**.
2. Name: `zerobug-ec2`.
3. AMI: **Ubuntu 22.04/24.04 LTS**, **64-bit (x86)**, Free tier eligible.
4. Instance type:

| Loại | Ghi chú |
| --- | --- |
| **`t3.medium`** | Khuyến nghị thiết kế — đủ RAM cho Spring Boot + JVM |
| **`t3.small`** | Cân bằng chi phí/hiệu năng |
| **`t3.micro`** | Free tier — **yếu cho JVM**, chỉ dùng demo ngắn |

#### Xử lý lỗi Launch

| Lỗi | Nguyên nhân | Sửa |
| --- | --- | --- |
| Ubuntu + t3 incompatible | Chọn AMI **Arm** với `t3.*` | Chọn x86, hoặc AMI Arm + **`t4g.*`** |
| SQL/Marketplace AMI | Chọn nhầm AMI trả phí | Quick Start → Ubuntu Free tier |

![](/images/5-Workshop/5.5/5.jpg)
![](/images/5-Workshop/5.5/6.jpg)
![](/images/5-Workshop/5.5/7.jpg)

5. Key pair: `zerobug-key`.
6. **Network:** VPC `zerobug-vpc`, Subnet **`zerobug-private-a`**, Public IP **Disable**, SG **`zerobug-ec2-sg`**.
7. **IAM instance profile:** **`zerobug-ec2-role`**. Nếu dropdown trống → **Specify a custom value** → `zerobug-ec2-role`.

![](/images/5-Workshop/5.5/8.jpg)
![](/images/5-Workshop/5.5/9.jpg)
![](/images/5-Workshop/5.5/10.jpg)
![](/images/5-Workshop/5.5/11.jpg)
![](/images/5-Workshop/5.5/12.jpg)

{{% notice tip %}}
Lỗi `iam:ListInstanceProfiles`: xem [5.3.1 — PassRole policy](../../5.3-tv1-iam-vpc/5.3.1-iam-users-groups/) — đăng xuất/đăng nhập lại sau khi Trí thêm PassRole policy. Có thể gắn role sau: **Actions → Security → Modify IAM role** → **Update IAM role**.
{{% /notice %}}

8. **Launch instance**.
9. Đợi **Status checks: 2/2 passed** trước khi Connect SSM.

![](/images/5-Workshop/5.5/13.jpg)
