---
title: "Dọn tài nguyên - Kiệt"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.10.4. </b> "
---

Xóa **RDS sau** khi Toàn/Hoa đã terminate (không còn kết nối). **S3** phải **empty bucket** trước khi delete.

#### Bước 1 — RDS PostgreSQL

1. **RDS** → **Databases** → chọn `zerobug-db`.
2. **Actions** → **Delete**.
3. Bỏ tick **Create final snapshot** *(workshop — tiết kiệm)* hoặc tạo snapshot nếu cần giữ dữ liệu.
4. Gõ `delete me` → **Delete**.

Đợi status biến mất (~ vài phút).

![](/images/5-Workshop/5.10/21.png)

#### Bước 2 — DB Subnet Group

1. **RDS** → **Subnet groups** → `zerobug-db-subnet-group`.
2. **Delete** *(chỉ khi không còn DB instance)*.

![](/images/5-Workshop/5.10/22.png)

#### Bước 3 — AWS Secrets Manager

1. **Secrets Manager** → secret **`zerobug/rds/credentials`** → **Delete secret**.

![](/images/5-Workshop/5.10/24.png)

#### Bước 4 — Amazon S3

1. **S3** → bucket `zerobug-projects-<suffix>` (ví dụ `zerobug-projects-hutech01`).
2. Tab **Objects** — chọn **Empty** (xóa mọi object, gồm prefix `deploy/zerobug-agent-app-1.0.0.jar` và source project).
3. **Delete bucket** → gõ tên bucket xác nhận.

![](/images/5-Workshop/5.10/23.png)

{{% notice warning %}}
Bucket **không empty** sẽ không xóa được. Bật **Show versions** nếu bucket có versioning và xóa cả delete markers.
{{% /notice %}}

#### Bước 5 — CloudWatch (tùy chọn)

Xóa alarm/log RDS (nếu tạo ở mục 5.9).

#### Checklist xác nhận

- [x] Không còn DB `zerobug-db`
- [x] Không còn subnet group `zerobug-db-subnet-group`
- [x] Secret `zerobug/rds/credentials` đã xóa
- [x] S3 bucket ZeroBug đã empty + deleted

→ Tiếp: [Trí — VPC & NAT](5.10.5-tri/)
