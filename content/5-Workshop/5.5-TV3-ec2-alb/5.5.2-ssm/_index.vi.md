---
title: "SSM Session Manager"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

1. EC2 → `zerobug-ec2` → **Connect** → **Session Manager** → **Connect**.

![](/images/5-Workshop/5.5/13.jpg)

#### Lỗi: Ping Offline / `unable to acquire credentials`

| Bước | Hành động |
| --- | --- |
| 1 | **Actions → Security → Modify IAM role** → chọn `zerobug-ec2-role` → **Update IAM role** *(phải bấm Update, không chỉ chọn trong list)* |
| 2 | IAM → Roles → `zerobug-ec2-role` → tab Permissions → phải có **`AmazonSSMManagedInstanceCore`** |
| 3 | **Reboot instance** → đợi **3–5 phút** |
| 4 | Kiểm tra NAT/route (Trí) — xem bảng bên dưới |

![](/images/5-Workshop/5.5/4.jpg)

#### Lỗi `send request failed`

EC2 private **không ra internet** → SSM Agent không lấy được credential.

**Checklist NAT/route (Trí):**

| Kiểm tra | Kỳ vọng |
| --- | --- |
| NAT Gateway `zerobug-nat` | Status **Available** |
| Route table `zerobug-rt-private` | Route `0.0.0.0/0` → NAT |
| Subnet association | `zerobug-private-a` gắn `zerobug-rt-private` |
| EC2-SG outbound | **All traffic** → `0.0.0.0/0` (mục 5.3.7) |

Sau khi sửa → reboot EC2 → thử Connect lại.

**Test nhanh trong Session Manager** (khi đã Online):

```bash
curl -I https://aws.amazon.com
```

HTTP 200/301 = outbound OK.

#### VPC Endpoints (dự phòng — SSM không cần NAT)

Nếu không muốn EC2 ra internet qua NAT chỉ để SSM, tạo **Interface Endpoint** trong VPC `zerobug-vpc`:

| Service | Endpoint name |
| --- | --- |
| `com.amazonaws.ap-southeast-1.ssm` | `ssm` |
| `com.amazonaws.ap-southeast-1.ssmmessages` | `ssmmessages` |
| `com.amazonaws.ap-southeast-1.ec2messages` | `ec2messages` |

Cấu hình chung: Subnet **private-a/b**, SG cho phép **HTTPS 443** từ EC2-SG, bật **Enable DNS name**.

{{% notice info %}}
VPC Endpoint giúp SSM Online **không cần NAT**; Lambda **`BedrockInvokeLambda`** **vẫn cần NAT** để HTTPS tới `bedrock-mantle.us-east-1.api.aws` (cross-region).
{{% /notice %}}

#### Fallback tạm thời (chỉ debug)

Chuyển EC2 sang **public subnet** + bật public IP tạm → Connect SSM → sửa xong chuyển lại private.

<!-- Hình: /images/5-Workshop/5.5-Toàn/ssm.png -->
