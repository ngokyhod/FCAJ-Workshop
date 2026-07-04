---
title: "SSM Session Manager"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

1. EC2 → `zerobug-ec2` → **Connect** → **Session Manager** → **Connect**.

![](/images/5-Workshop/5.5/13.jpg)

#### Error: Ping Offline / `unable to acquire credentials`

| Step | Action |
| --- | --- |
| 1 | **Actions → Security → Modify IAM role** → select `zerobug-ec2-role` → **Update IAM role** *(must click Update, not just select in the list)* |
| 2 | IAM → Roles → `zerobug-ec2-role` → Permissions tab → must have **`AmazonSSMManagedInstanceCore`** |
| 3 | **Reboot instance** → wait **3–5 minutes** |
| 4 | Check NAT/route (Trí) — see table below |

![](/images/5-Workshop/5.5/4.jpg)

#### Error `send request failed`

Private EC2 **cannot reach internet** → SSM Agent cannot obtain credentials.

**NAT/route checklist (Trí):**

| Check | Expected |
| --- | --- |
| NAT Gateway `zerobug-nat` | Status **Available** |
| Route table `zerobug-rt-private` | Route `0.0.0.0/0` → NAT |
| Subnet association | `zerobug-private-a` attached to `zerobug-rt-private` |
| EC2-SG outbound | **All traffic** → `0.0.0.0/0` (section 5.3.7) |

After fixing → reboot EC2 → try Connect again.

**Quick test in Session Manager** (when Online):

```bash
curl -I https://aws.amazon.com
```

HTTP 200/301 = outbound OK.

#### VPC Endpoints (fallback — SSM without NAT)

If you do not want EC2 to reach the internet via NAT just for SSM, create **Interface Endpoints** in VPC `zerobug-vpc`:

| Service | Endpoint name |
| --- | --- |
| `com.amazonaws.ap-southeast-1.ssm` | `ssm` |
| `com.amazonaws.ap-southeast-1.ssmmessages` | `ssmmessages` |
| `com.amazonaws.ap-southeast-1.ec2messages` | `ec2messages` |

Common settings: Subnet **private-a/b**, SG allowing **HTTPS 443** from EC2-SG, enable **Enable DNS name**.

{{% notice info %}}
VPC Endpoint lets SSM go Online **without NAT**; Lambda **`BedrockInvokeLambda`** **still needs NAT** for HTTPS to `bedrock-mantle.us-east-1.api.aws` (cross-region).
{{% /notice %}}

#### Temporary fallback (debug only)

Move EC2 to **public subnet** + enable public IP temporarily → Connect SSM → fix and move back to private.

<!-- Image: /images/5-Workshop/5.5-Toàn/ssm.png -->
