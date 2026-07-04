---
title: "Toàn – EC2 & ALB"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

**Thành viên phụ trách:** Toàn  
**Phụ thuộc Trí + Kiệt:** Subnet/SG/`zerobug-ec2-role`; S3 bucket, RDS endpoint, Secret DB.

#### Toàn làm gì?

Triển khai **backend compute**: EC2 Spring Boot (Private Subnet) + **ALB** (Public Subnet). EC2 gọi Step Functions khi user sinh test — **không SQS**.

#### Chức năng & dùng ở đâu

| Thành phần | Chức năng | Dùng ở đâu |
| --- | --- | --- |
| **EC2 (Private)** | REST API `/api/*`, invoke Step Functions | Sau CloudFront → API GW → ALB |
| **ALB (Public)** | HTTP:80 → EC2:8080 | Integration API Gateway (Trinh) |
| **SSM Session Manager** | Truy cập EC2 không cần public IP | Cài Java, deploy JAR |
| **systemd** | Chạy JAR 24/7 | Production runtime |

#### Output bàn giao

**ALB DNS name** (`*.elb.amazonaws.com`) → Trinh cấu hình `{proxy+}`.

#### Chuẩn bị riêng Toàn

1. Chạy `build-all.bat` → `zerobug-agent-app-1.0.0.jar` ([5.2.2](../5.2-prerequisites/5.2.2-tools-and-build/)).
2. Upload S3: `s3://<bucket>/deploy/zerobug-agent-app-1.0.0.jar`.
3. Xác nhận `zerobug-ec2-role` có **`AmazonSSMManagedInstanceCore`**.

{{% notice info %}}
Có thể **tạm dừng** sau bước cài Java/SSM — làm Trinh trước; test ở mục 5.8.
{{% /notice %}}

#### Nội dung triển khai

1. [Khởi tạo EC2 Instance](5.5.1-ec2-instance/)
2. [Kết nối SSM Session Manager](5.5.2-ssm/)
3. [Cài Java 17 & AWS CLI v2](5.5.3-java-awscli/)
4. [Deploy JAR & systemd](5.5.4-deploy-systemd/)
5. [ALB & Target Group](5.5.5-alb/)
6. [Kiểm tra & bàn giao](5.5.6-verification-handoff/)
