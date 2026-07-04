---
title: "Route Tables"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.3.6. </b> "
---

#### Public route table — `zerobug-rt-public`

1. **Route tables** → **Create route table** → Name: `zerobug-rt-public`, VPC: `zerobug-vpc`.
2. **Routes** tab → **Edit routes** → **Add route**:
   - Destination: `0.0.0.0/0`
   - Target: **Internet Gateway** → `zerobug-igw`
   - → **Save**
3. **Subnet associations** tab → **Edit** → select `zerobug-public-a`, `zerobug-public-b` → **Save**.

![](/images/5-Workshop/5.3/37.png)
![](/images/5-Workshop/5.3/38.png)
![](/images/5-Workshop/5.3/39.png)
![](/images/5-Workshop/5.3/40.png)
![](/images/5-Workshop/5.3/41.png)

#### Private route table — `zerobug-rt-private`

1. **Create route table** → Name: `zerobug-rt-private`, VPC: `zerobug-vpc`.
2. **Routes** tab → **Add route**:
   - Destination: `0.0.0.0/0`
   - Target: **NAT Gateway** → `zerobug-nat`
   - → **Save**
3. **Subnet associations** tab → select `zerobug-private-a`, `zerobug-private-b` → **Save**.

![](/images/5-Workshop/5.3/47.png)
![](/images/5-Workshop/5.3/48.png)
![](/images/5-Workshop/5.3/49.png)
![](/images/5-Workshop/5.3/50.png)
