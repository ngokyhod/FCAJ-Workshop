---
title: "RDS PostgreSQL"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

#### Tạo DB Subnet Group

1. **RDS** → **Subnet groups** → **Create DB subnet group**.
2. Name: `zerobug-db-subnet-group`; VPC: `zerobug-vpc`.
3. Add subnets: AZ `1a` & `1b` → chọn **private-a**, **private-b**.
4. → **Create**.

![](/images/5-Workshop/5.4/7.png)
![](/images/5-Workshop/5.4/8.png)
![](/images/5-Workshop/5.4/9.png)
![](/images/5-Workshop/5.4/10.png)

#### Khởi tạo RDS Instance

1. **Databases** → **Create database** → **Standard create**.
2. Engine: **PostgreSQL 15.x**, Template: **Free tier**.
3. **DB instance identifier:** `zerobug-db`.
4. **Master username:** `zerobugadmin`; password mạnh *(ghi lại)*.
5. **Instance:** `db.t3.micro` hoặc `db.t4g.micro`.
6. **Storage:** 20 GB, tắt autoscaling.
7. **Connectivity:**
   - VPC: `zerobug-vpc`
   - DB subnet group: `zerobug-db-subnet-group`
   - **Public access: No**
   - Security group: **`zerobug-rds-sg`** (bỏ default)
8. **Initial database name:** `zerobug`.
9. → **Create database** → đợi **Available** (~5–10 phút).
10. Copy **Endpoint** (tab Connectivity & security).

![](/images/5-Workshop/5.4/11.png)
![](/images/5-Workshop/5.4/12.png)
![](/images/5-Workshop/5.4/13.png)
![](/images/5-Workshop/5.4/14.png)
![](/images/5-Workshop/5.4/15.png)
![](/images/5-Workshop/5.4/16.png)
![](/images/5-Workshop/5.4/17.png)
![](/images/5-Workshop/5.4/18.png)
![](/images/5-Workshop/5.4/19.png)
![](/images/5-Workshop/5.4/20.png)
![](/images/5-Workshop/5.4/21.png)
![](/images/5-Workshop/5.4/22.png)

{{% notice info %}}
Engine **PostgreSQL 15.x** trên RDS hỗ trợ extension **`vector`** (pgvector) — Kiệt bật ở [5.4.4 — RAG pgvector](5.4.4-schema-jpa/). Dùng bản **15.2+** nếu Console cho phép chọn minor version.
{{% /notice %}}
