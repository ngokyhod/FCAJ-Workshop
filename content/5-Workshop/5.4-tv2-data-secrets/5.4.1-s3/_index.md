---
title: "Amazon S3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

#### Create S3 Bucket

1. Search `S3` → **Create bucket**.
2. **Bucket name:** `zerobug-projects-<unique-suffix>` *(global unique)*.
3. **Region:** ap-southeast-1.
4. **Block Public Access:** enable **all**.
5. → **Create bucket**.

![](/images/5-Workshop/5.4/1.png)
![](/images/5-Workshop/5.4/2.png)
![](/images/5-Workshop/5.4/3.png)
![](/images/5-Workshop/5.4/4.png)
![](/images/5-Workshop/5.4/5.png)
![](/images/5-Workshop/5.4/6.png)

#### Bucket Policy (optional)

Restrict access to `zerobug-ec2-role` and `zerobug-lambda-role` only. Not required if roles already have `AmazonS3FullAccess`.

#### Verify upload/download

Upload a test file → delete. OK.

Toàn uploads JAR: `s3://<bucket>/deploy/zerobug-agent-app-1.0.0.jar`.

<!-- Image: /images/5-Workshop/5.4-Kiệt/s3.png -->
