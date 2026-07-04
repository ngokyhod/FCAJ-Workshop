---
title: "RDS PostgreSQL"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

#### Create DB Subnet Group

1. **RDS** → **Subnet groups** → **Create DB subnet group**.
2. Name: `zerobug-db-subnet-group`; VPC: `zerobug-vpc`.
3. Add subnets: AZ `1a` & `1b` → select **private-a**, **private-b**.
4. → **Create**.

![](/images/5-Workshop/5.4/7.png)
![](/images/5-Workshop/5.4/8.png)
![](/images/5-Workshop/5.4/9.png)
![](/images/5-Workshop/5.4/10.png)

#### Create RDS Instance

1. **Databases** → **Create database** → **Standard create**.
2. Engine: **PostgreSQL 15.x**, Template: **Free tier**.
3. **DB instance identifier:** `zerobug-db`.
4. **Master username:** `zerobugadmin`; strong password *(record it)*.
5. **Instance:** `db.t3.micro` or `db.t4g.micro`.
6. **Storage:** 20 GB, disable autoscaling.
7. **Connectivity:**
   - VPC: `zerobug-vpc`
   - DB subnet group: `zerobug-db-subnet-group`
   - **Public access: No**
   - Security group: **`zerobug-rds-sg`** (remove default)
8. **Initial database name:** `zerobug`.
9. → **Create database** → wait for **Available** (~5–10 minutes).
10. Copy **Endpoint** (Connectivity & security tab).

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
Engine **PostgreSQL 15.x** on RDS supports the **`vector`** extension (pgvector) — Kiệt enables it in [5.4.4 — RAG pgvector](5.4.4-schema-jpa/). Use version **15.2+** if the Console allows selecting a minor version.
{{% /notice %}}
