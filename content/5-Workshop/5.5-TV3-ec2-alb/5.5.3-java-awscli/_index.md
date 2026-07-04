---
title: "Java & AWS CLI"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

#### Install Java 17

```bash
sudo apt update
sudo apt install -y openjdk-17-jre-headless unzip curl
java -version
```

#### Install AWS CLI v2

Ubuntu 24.04 **does not have** the `apt install awscli` package:

```bash
cd /tmp
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
aws sts get-caller-identity
```

| Error | Fix |
| --- | --- |
| `Package 'awscli' has no installation candidate` | Use v2 installer above |
| `curl: Permission denied` | Run in **`/tmp`**, not SSM home directory |
| Arm instance | URL `awscli-exe-linux-aarch64.zip` |

<!-- Image: /images/5-Workshop/5.5-Toàn/awscli.png -->
