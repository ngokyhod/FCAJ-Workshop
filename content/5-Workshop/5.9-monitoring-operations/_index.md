---
title: "Monitoring & Operations"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

**Shared section** — apply **during deployment** (5.3–5.7) and **when debugging** section [5.8](../5.8-end-to-end-testing/). Each member monitors their own resources; you do not need to wait for E2E tests before enabling basic logs/alarms.

#### Monitoring by block

| Block | Member | CloudWatch / tools | Notes |
| --- | --- | --- | --- |
| VPC / NAT | Trí | VPC Reachability Analyzer; NAT status | Mantle Lambda timeout → check NAT + route |
| S3 / RDS / Secrets | Kiệt | RDS metrics; Secrets get-value test | pgvector: `\d code_embeddings` on psql |
| EC2 / ALB | Toàn | Target Group health; `journalctl -u zerobug` | Unhealthy usually means JAR not started |
| Lambda / Step Fn | Hoa | `/aws/lambda/*`; Step Fn execution history | Log embed + chat Mantle; HTTP 401 → API key |
| Edge / Auth | Trinh | API GW access logs; WAF sampled requests | 401 JWT vs 502 backend |

#### Bedrock Mantle & cost

- **Chat** inference (`openai.gpt-oss-120b`) and **embedding** (`cohere.embed-multilingual-v3`) appear on **AWS Billing / Cost Explorer** — same bill as NAT, EC2, RDS.
- Mantle API key TTL **~30 days** — env `BEDROCK_MANTLE_API_KEY` on **`ContextBuilderLambda`** and **`BedrockInvokeLambda`**; rotate before expiry ([5.9.2](5.9.2-budgets-sns/)).
- **NAT Gateway** ~32 USD/month — highest cost after running the workshop; delete early when not in use ([5.10](../5.10-cleanup/)).

#### Suggested alarms & budget (workshop)

| Alarm | Suggested threshold | Who handles |
| --- | --- | --- |
| ALB `UnHealthyHostCount` | ≥ 1 for 5 minutes | Toàn |
| Lambda `Errors` | ≥ 1 | Hoa |
| RDS `CPUUtilization` | > 80% *(db.t3.micro)* | Kiệt |
| AWS Budgets | Email when team threshold exceeded | Entire team |

Frontend (when app is running): `GET /api/aws/status` — RDS, S3, **Bedrock Mantle** status.

#### Content

1. [CloudWatch Logs & Alarms](5.9.1-cloudwatch/)
2. [AWS Budgets & SNS](5.9.2-budgets-sns/)
3. [Debug by member](5.9.3-debug-by-member/)

{{% notice info %}}
Section 5.9 does **not require** all alarms before demo — prioritize logs + Step Fn history when debugging. Budgets/SNS should be enabled early to avoid leaving NAT/WAF running overnight.
{{% /notice %}}
