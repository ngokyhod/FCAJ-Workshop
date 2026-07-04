---
title: "CloudWatch"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.9.1. </b> "
---

| Component | Member | Notes |
| --- | --- | --- |
| EC2 Spring Boot | Toàn | `journalctl -u zerobug` |
| Lambda | Hoa | `/aws/lambda/...` |
| Step Functions | Hoa | Execution history |
| API Gateway | Trinh | Access logs (optional) |

**Suggested alarms:** Lambda errors, ALB UnHealthyHostCount, RDS CPU/connections.

<!-- Image: /images/5-Workshop/5.9-Monitoring/cloudwatch.png -->
