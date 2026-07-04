---
title: "Secrets Manager"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

#### Store DB Credentials

1. **Secrets Manager** → **Store a new secret**.
2. Type: **Credentials for Amazon RDS database**.
3. User: `zerobugadmin`, Password: *(as set)*, Database: `zerobug-db`.
4. Secret name: **`zerobug/rds/credentials`** → **Store**.
5. Copy **Secret ARN**.

![](/images/5-Workshop/5.4/23.png)
![](/images/5-Workshop/5.4/24.png)
![](/images/5-Workshop/5.4/25.png)
![](/images/5-Workshop/5.4/26.png)
![](/images/5-Workshop/5.4/27.png)

#### Verify read permissions

EC2 & Lambda roles (Trí) have `SecretsManagerReadWrite` — read DB secret at runtime, do not hardcode password.

#### Bedrock Mantle

→ Continue to [5.4.5 — Enable model access](5.4.5-bedrock-mantle/) in Bedrock Console (`us-east-1`).
