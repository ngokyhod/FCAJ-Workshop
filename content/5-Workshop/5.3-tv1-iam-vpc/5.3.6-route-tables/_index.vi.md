---
title: "Route Tables"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.3.6. </b> "
---

#### Route table Public — `zerobug-rt-public`

1. **Route tables** → **Create route table** → Name: `zerobug-rt-public`, VPC: `zerobug-vpc`.
2. Tab **Routes** → **Edit routes** → **Add route**:
   - Destination: `0.0.0.0/0`
   - Target: **Internet Gateway** → `zerobug-igw`
   - → **Save**
3. Tab **Subnet associations** → **Edit** → tích `zerobug-public-a`, `zerobug-public-b` → **Save**.

![](/images/5-Workshop/5.3/37.png)
![](/images/5-Workshop/5.3/38.png)
![](/images/5-Workshop/5.3/39.png)
![](/images/5-Workshop/5.3/40.png)
![](/images/5-Workshop/5.3/41.png)

#### Route table Private — `zerobug-rt-private`

1. **Create route table** → Name: `zerobug-rt-private`, VPC: `zerobug-vpc`.
2. Tab **Routes** → **Add route**:
   - Destination: `0.0.0.0/0`
   - Target: **NAT Gateway** → `zerobug-nat`
   - → **Save**
3. Tab **Subnet associations** → tích `zerobug-private-a`, `zerobug-private-b` → **Save**.

![](/images/5-Workshop/5.3/47.png)
![](/images/5-Workshop/5.3/48.png)
![](/images/5-Workshop/5.3/49.png)
![](/images/5-Workshop/5.3/50.png)
