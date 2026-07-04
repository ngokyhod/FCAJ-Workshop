---
title: "Dọn tài nguyên - Toàn"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.10.3. </b> "
---

Xóa **ALB trước EC2** — Target Group chỉ xóa được sau khi không còn listener/load balancer tham chiếu.

#### Bước 1 — Application Load Balancer

1. **EC2** → **Load Balancers** → chọn `zerobug-alb`.
2. **Listeners** — xóa listener HTTP `:80` (nếu cần tách bước).
3. **Actions** → **Delete load balancer** → xác nhận.

![](/images/5-Workshop/5.10/11.png)
![](/images/5-Workshop/5.10/12.png)
![](/images/5-Workshop/5.10/13.png)
![](/images/5-Workshop/5.10/14.png)

#### Bước 2 — Target Group

1. **Target Groups** → `zerobug-tg`.
2. Tab **Targets** — **Deregister** instance `zerobug-ec2` (nếu còn).
3. **Actions** → **Delete** target group.

{{% notice tip %}}
Target Group **Unhealthy** vẫn xóa bình thường sau khi ALB đã xóa.
{{% /notice %}}

![](/images/5-Workshop/5.10/15.png)
![](/images/5-Workshop/5.10/16.png)

#### Bước 3 — EC2 Instance

1. **Instances** → chọn `zerobug-ec2`.
2. **Instance state** → **Stop instance** *(tùy chọn, dễ kiểm tra)* → **Terminate instance**.
3. Xác nhận **Terminate**.

![](/images/5-Workshop/5.10/17.png)
![](/images/5-Workshop/5.10/18.png)

#### Bước 4 — Key Pair (tùy chọn)

1. **Key Pairs** → `zerobug-key` → **Delete** *(chỉ khi không còn instance nào dùng)*.

![](/images/5-Workshop/5.10/19.png)
![](/images/5-Workshop/5.10/20.png)

#### Bước 5 — CloudWatch Logs EC2 (tùy chọn)

Xóa log group `/aws/ec2/zerobug` hoặc agent log (nếu cấu hình).

#### Checklist xác nhận

- [x] Không còn ALB `zerobug-alb`
- [x] Không còn Target Group `zerobug-tg`
- [x] Instance `zerobug-ec2` = **terminated**
- [x] Không còn ENI orphan trong VPC

→ Tiếp: [Kiệt — Data](5.10.4-kiet/)
