---
title: "Subnets"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

1. **Subnets** → **Create subnet** → VPC = `zerobug-vpc`.
2. **Add new subnet** multiple times in one form:

| Subnet name | AZ | CIDR |
| --- | --- | --- |
| `zerobug-public-a` | ap-southeast-1a | `10.0.1.0/24` |
| `zerobug-public-b` | ap-southeast-1b | `10.0.2.0/24` |
| `zerobug-private-a` | ap-southeast-1a | `10.0.11.0/24` |
| `zerobug-private-b` | ap-southeast-1b | `10.0.12.0/24` |

3. → **Create subnet**. Copy 4 **Subnet ID**s.

![](/images/5-Workshop/5.3/22.jpg)
![](/images/5-Workshop/5.3/23.jpg)
![](/images/5-Workshop/5.3/24.jpg)
![](/images/5-Workshop/5.3/25.jpg)
![](/images/5-Workshop/5.3/26.jpg)
![](/images/5-Workshop/5.3/27.jpg)

#### Enable auto-assign public IP (public subnets)

- Select `zerobug-public-a` → **Actions** → **Edit subnet settings** → **Enable auto-assign public IPv4 address** → **Save**.
- Repeat for `zerobug-public-b`.

![](/images/5-Workshop/5.3/28.jpg)
![](/images/5-Workshop/5.3/29.jpg)
