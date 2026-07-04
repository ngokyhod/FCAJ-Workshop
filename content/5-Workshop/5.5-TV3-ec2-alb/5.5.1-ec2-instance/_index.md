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

| Type | Notes |
| --- | --- |
| **`t3.medium`** | Recommended design — enough RAM for Spring Boot + JVM |
| **`t3.small`** | Balance cost/performance |
| **`t3.micro`** | Free tier — **weak for JVM**, demo only |

#### Launch error handling

| Error | Cause | Fix |
| --- | --- | --- |
| Ubuntu + t3 incompatible | Selected **Arm** AMI with `t3.*` | Choose x86, or Arm AMI + **`t4g.*`** |
| SQL/Marketplace AMI | Selected paid AMI by mistake | Quick Start → Ubuntu Free tier |

![](/images/5-Workshop/5.5/5.jpg)
![](/images/5-Workshop/5.5/6.jpg)
![](/images/5-Workshop/5.5/7.jpg)

5. Key pair: `zerobug-key`.
6. **Network:** VPC `zerobug-vpc`, Subnet **`zerobug-private-a`**, Public IP **Disable**, SG **`zerobug-ec2-sg`**.
7. **IAM instance profile:** **`zerobug-ec2-role`**. If dropdown is empty → **Specify a custom value** → `zerobug-ec2-role`.

![](/images/5-Workshop/5.5/8.jpg)
![](/images/5-Workshop/5.5/9.jpg)
![](/images/5-Workshop/5.5/10.jpg)
![](/images/5-Workshop/5.5/11.jpg)
![](/images/5-Workshop/5.5/12.jpg)

{{% notice tip %}}
`iam:ListInstanceProfiles` error: see [5.3.1 — PassRole policy](../../5.3-tv1-iam-vpc/5.3.1-iam-users-groups/) — sign out/in again after Trí adds PassRole policy. You can attach the role later: **Actions → Security → Modify IAM role** → **Update IAM role**.
{{% /notice %}}

8. **Launch instance**.
9. Wait for **Status checks: 2/2 passed** before connecting via SSM.

![](/images/5-Workshop/5.5/13.jpg)
