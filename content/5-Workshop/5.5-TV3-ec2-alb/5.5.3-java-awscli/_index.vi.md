---
title: "Java & AWS CLI"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

#### Cài Java 17

```bash
sudo apt update
sudo apt install -y openjdk-17-jre-headless unzip curl
java -version
```

#### Cài AWS CLI v2

Ubuntu 24.04 **không có** gói `apt install awscli`:

```bash
cd /tmp
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
aws sts get-caller-identity
```

| Lỗi | Sửa |
| --- | --- |
| `Package 'awscli' has no installation candidate` | Dùng installer v2 ở trên |
| `curl: Permission denied` | Chạy trong **`/tmp`**, không thư mục home SSM |
| Arm instance | URL `awscli-exe-linux-aarch64.zip` |

<!-- Hình: /images/5-Workshop/5.5-Toàn/awscli.png -->
