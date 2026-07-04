---
title: "Resource Cleanup – Toàn"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.10.3. </b> "
---

Delete **ALB before EC2** — Target Group can only be deleted after no listener/load balancer references it.

#### Step 1 — Application Load Balancer

1. **EC2** → **Load Balancers** → select `zerobug-alb`.
2. **Listeners** — delete HTTP `:80` listener (if you need to split steps).
3. **Actions** → **Delete load balancer** → confirm.

![](/images/5-Workshop/5.10/11.png)
![](/images/5-Workshop/5.10/12.png)
![](/images/5-Workshop/5.10/13.png)
![](/images/5-Workshop/5.10/14.png)

#### Step 2 — Target Group

1. **Target Groups** → `zerobug-tg`.
2. Tab **Targets** — **Deregister** instance `zerobug-ec2` (if still registered).
3. **Actions** → **Delete** target group.

{{% notice tip %}}
Target Group **Unhealthy** can still be deleted normally after ALB is deleted.
{{% /notice %}}

![](/images/5-Workshop/5.10/15.png)
![](/images/5-Workshop/5.10/16.png)

#### Step 3 — EC2 Instance

1. **Instances** → select `zerobug-ec2`.
2. **Instance state** → **Stop instance** *(optional, easier to verify)* → **Terminate instance**.
3. Confirm **Terminate**.

![](/images/5-Workshop/5.10/17.png)
![](/images/5-Workshop/5.10/18.png)

#### Step 4 — Key Pair (optional)

1. **Key Pairs** → `zerobug-key` → **Delete** *(only when no instance uses it)*.

![](/images/5-Workshop/5.10/19.png)
![](/images/5-Workshop/5.10/20.png)

#### Step 5 — CloudWatch Logs EC2 (optional)

Delete log group `/aws/ec2/zerobug` or agent log (if configured).

#### Confirmation checklist

- [x] No ALB `zerobug-alb` remaining
- [x] No Target Group `zerobug-tg` remaining
- [x] Instance `zerobug-ec2` = **terminated**
- [x] No orphan ENI in VPC

→ Next: [Kiệt — Data](5.10.4-kiet/)
