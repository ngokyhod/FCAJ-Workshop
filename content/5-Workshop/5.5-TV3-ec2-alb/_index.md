---
title: "Toàn – EC2 & ALB"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

**Owner:** Toàn  
**Depends on Trí + Kiệt:** Subnet/SG/`zerobug-ec2-role`; S3 bucket, RDS endpoint, DB Secret.

#### What does Toàn do?

Deploy **backend compute**: EC2 Spring Boot (Private Subnet) + **ALB** (Public Subnet). EC2 invokes Step Functions when users generate tests — **no SQS**.

#### Components & where they are used

| Component | Function | Used by |
| --- | --- | --- |
| **EC2 (Private)** | REST API `/api/*`, invoke Step Functions | After CloudFront → API GW → ALB |
| **ALB (Public)** | HTTP:80 → EC2:8080 | API Gateway integration (Trinh) |
| **SSM Session Manager** | Access EC2 without public IP | Install Java, deploy JAR |
| **systemd** | Run JAR 24/7 | Production runtime |

#### Handoff output

**ALB DNS name** (`*.elb.amazonaws.com`) → Trinh configures `{proxy+}`.

#### Toàn-specific preparation

1. Run `build-all.bat` → `zerobug-agent-app-1.0.0.jar` ([5.2.2](../5.2-prerequisites/5.2.2-tools-and-build/)).
2. Upload to S3: `s3://<bucket>/deploy/zerobug-agent-app-1.0.0.jar`.
3. Confirm `zerobug-ec2-role` has **`AmazonSSMManagedInstanceCore`**.

{{% notice info %}}
You may **pause** after the Java/SSM install step — do Trinh's section first; test in section 5.8.
{{% /notice %}}

#### Deployment content

1. [Launch EC2 Instance](5.5.1-ec2-instance/)
2. [Connect via SSM Session Manager](5.5.2-ssm/)
3. [Install Java 17 & AWS CLI v2](5.5.3-java-awscli/)
4. [Deploy JAR & systemd](5.5.4-deploy-systemd/)
5. [ALB & Target Group](5.5.5-alb/)
6. [Verification & handoff](5.5.6-verification-handoff/)
