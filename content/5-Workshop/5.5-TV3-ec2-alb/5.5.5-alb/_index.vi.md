---
title: "ALB & Target Group"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5.5. </b> "
---

#### Target Group — `zerobug-tg`

1. **Target Groups** → **Create**.
2. Instances, HTTP **8080**, VPC `zerobug-vpc`.
3. Health check path: **`/api/health`**.
4. Register `zerobug-ec2` → chọn instance → **Include as pending below** → **Create pending** → **Save**.

![](/images/5-Workshop/5.5/18.png)
![](/images/5-Workshop/5.5/19.png)
![](/images/5-Workshop/5.5/20.png)
![](/images/5-Workshop/5.5/21.png)
![](/images/5-Workshop/5.5/22.png)
![](/images/5-Workshop/5.5/23.png)
![](/images/5-Workshop/5.5/24.png)

#### ALB — `zerobug-alb`

1. **Load Balancers** → **Application Load Balancer**.
2. **Scheme:** Internet-facing | **IP address type:** IPv4.
3. VPC `zerobug-vpc`, chọn **2 AZ**, map **public subnet-a** và **public subnet-b**.
4. SG: **`zerobug-alb-sg`**.
5. Listener HTTP **:80** → `zerobug-tg`.
6. Copy **DNS name** → bảng tham số → **bàn giao Trinh**.

#### Target Unhealthy

| Tình huống | Xử lý |
| --- | --- |
| Backend chưa chạy | **Bình thường** — ghi báo cáo, test sau |
| App đã chạy mà vẫn unhealthy | `systemctl status`; EC2-SG 8080 từ ALB-SG; outbound All traffic |
| Outbound SG = 0 | Thêm All traffic → 0.0.0.0/0 (Trí mục 5.3.7) |

Test: `http://<ALB-DNS>/api/health`

![](/images/5-Workshop/5.5/25.png)
![](/images/5-Workshop/5.5/26.png)
![](/images/5-Workshop/5.5/27.png)
![](/images/5-Workshop/5.5/28.png)
![](/images/5-Workshop/5.5/29.png)
