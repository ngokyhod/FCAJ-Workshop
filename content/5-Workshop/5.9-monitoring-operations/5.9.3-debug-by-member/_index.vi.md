---
title: "Debug theo thành viên"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.9.3. </b> "
---

| Thành viên | Công cụ |
| --- | --- |
| Trí | VPC Reachability; NAT/route check |
| Kiệt | Secrets get-value; RDS connectivity; `\dx` / `\d code_embeddings` trên psql |
| Toàn | SSM; Target Group health; ALB access logs |
| Hoa | Step Fn history; Lambda test invoke; CloudWatch log embed + retrieve top-k |
| Trinh | API GW test; WAF sampled requests |

Frontend: `GET /api/aws/status` — trạng thái RDS, S3, **Bedrock Mantle**.
