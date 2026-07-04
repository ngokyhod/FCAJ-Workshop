---
title: "Amazon S3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

#### Tạo S3 Bucket

1. Search `S3` → **Create bucket**.
2. **Bucket name:** `zerobug-projects-<hậu-tố-duy-nhất>` *(global unique)*.
3. **Region:** ap-southeast-1.
4. **Block Public Access:** bật **tất cả**.
5. → **Create bucket**.

![](/images/5-Workshop/5.4/1.png)
![](/images/5-Workshop/5.4/2.png)
![](/images/5-Workshop/5.4/3.png)
![](/images/5-Workshop/5.4/4.png)
![](/images/5-Workshop/5.4/5.png)
![](/images/5-Workshop/5.4/6.png)

#### Bucket Policy (tùy chọn)

Siết quyền chỉ cho `zerobug-ec2-role` và `zerobug-lambda-role`. Không bắt buộc nếu role đã có `AmazonS3FullAccess`.

#### Kiểm tra upload/download

Upload thử 1 file → xóa. OK.

Toàn upload JAR: `s3://<bucket>/deploy/zerobug-agent-app-1.0.0.jar`.

<!-- Hình: /images/5-Workshop/5.4-Kiệt/s3.png -->
