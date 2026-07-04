---
title: "Secrets Manager"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

#### Lưu DB Credentials

1. **Secrets Manager** → **Store a new secret**.
2. Type: **Credentials for Amazon RDS database**.
3. User: `zerobugadmin`, Password: *(đã đặt)*, Database: `zerobug-db`.
4. Secret name: **`zerobug/rds/credentials`** → **Store**.
5. Copy **Secret ARN**.

![](/images/5-Workshop/5.4/23.png)
![](/images/5-Workshop/5.4/24.png)
![](/images/5-Workshop/5.4/25.png)
![](/images/5-Workshop/5.4/26.png)
![](/images/5-Workshop/5.4/27.png)

#### Kiểm tra quyền đọc

Role EC2 & Lambda (Trí) có `SecretsManagerReadWrite` — đọc secret DB runtime, không hardcode password.

#### Bedrock Mantle

→ Tiếp [5.4.5 — Bật model access](5.4.5-bedrock-mantle/) trên Bedrock Console (`us-east-1`).
