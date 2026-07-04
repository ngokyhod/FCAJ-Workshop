---
title: "Resource Cleanup – Kiệt"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.10.4. </b> "
---

Delete **RDS after** Toàn/Hoa have terminated (no active connections). **S3** must **empty bucket** before delete.

#### Step 1 — RDS PostgreSQL

1. **RDS** → **Databases** → select `zerobug-db`.
2. **Actions** → **Delete**.
3. Uncheck **Create final snapshot** *(workshop — save cost)* or create snapshot if you need to keep data.
4. Type `delete me` → **Delete**.

Wait until status disappears (~ a few minutes).

![](/images/5-Workshop/5.10/21.png)

#### Step 2 — DB Subnet Group

1. **RDS** → **Subnet groups** → `zerobug-db-subnet-group`.
2. **Delete** *(only when no DB instance remains)*.

![](/images/5-Workshop/5.10/22.png)

#### Step 3 — AWS Secrets Manager

1. **Secrets Manager** → secret **`zerobug/rds/credentials`** → **Delete secret**.

![](/images/5-Workshop/5.10/24.png)

#### Step 4 — Amazon S3

1. **S3** → bucket `zerobug-projects-<suffix>` (e.g. `zerobug-projects-hutech01`).
2. Tab **Objects** — select **Empty** (delete all objects, including prefix `deploy/zerobug-agent-app-1.0.0.jar` and project source).
3. **Delete bucket** → type bucket name to confirm.

![](/images/5-Workshop/5.10/23.png)

{{% notice warning %}}
Bucket **not empty** cannot be deleted. Enable **Show versions** if bucket has versioning and delete delete markers too.
{{% /notice %}}

#### Step 5 — CloudWatch (optional)

Delete RDS alarm/log (if created in section 5.9).

#### Confirmation checklist

- [x] No DB `zerobug-db` remaining
- [x] No subnet group `zerobug-db-subnet-group` remaining
- [x] Secret `zerobug/rds/credentials` deleted
- [x] ZeroBug S3 bucket empty + deleted

→ Next: [Trí — VPC & NAT](5.10.5-tri/)
