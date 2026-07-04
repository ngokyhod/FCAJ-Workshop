---
title: "CloudWatch"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.9.1. </b> "
---

| Thành phần | Thành viên | Ghi chú |
| --- | --- | --- |
| EC2 Spring Boot | Toàn | `journalctl -u zerobug` |
| Lambda | Hoa | `/aws/lambda/...` |
| Step Functions | Hoa | Execution history |
| API Gateway | Trinh | Access logs (tùy chọn) |

**Alarms gợi ý:** Lambda errors, ALB UnHealthyHostCount, RDS CPU/connections.

<!-- Hình: /images/5-Workshop/5.9-Monitoring/cloudwatch.png -->
