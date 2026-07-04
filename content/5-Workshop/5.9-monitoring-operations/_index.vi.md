---
title: "Giám sát & vận hành"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

**Phần chung** — áp dụng **trong lúc triển khai** (5.3–5.7) và **khi debug** mục [5.8](../5.8-end-to-end-testing/). Mỗi thành viên theo dõi tài nguyên của mình; không cần chờ test E2E xong mới bật log/alarm cơ bản.

#### Giám sát theo khối

| Khối | Thành viên | CloudWatch / công cụ | Ghi chú |
| --- | --- | --- | --- |
| VPC / NAT | Trí | VPC Reachability Analyzer; NAT status | Lambda timeout Mantle → kiểm tra NAT + route |
| S3 / RDS / Secrets | Kiệt | RDS metrics; Secrets get-value test | pgvector: `\d code_embeddings` trên psql |
| EC2 / ALB | Toàn | Target Group health; `journalctl -u zerobug` | Unhealthy thường do JAR chưa start |
| Lambda / Step Fn | Hoa | `/aws/lambda/*`; Step Fn execution history | Log embed + chat Mantle; HTTP 401 → API key |
| Edge / Auth | Trinh | API GW access logs; WAF sampled requests | 401 JWT vs 502 backend |

#### Bedrock Mantle & chi phí

- Inference **chat** (`openai.gpt-oss-120b`) và **embedding** (`cohere.embed-multilingual-v3`) hiện trên **AWS Billing / Cost Explorer** — cùng bill với NAT, EC2, RDS.
- API key Mantle TTL **~30 ngày** — env `BEDROCK_MANTLE_API_KEY` trên **`ContextBuilderLambda`** và **`BedrockInvokeLambda`**; rotate trước khi hết hạn ([5.9.2](5.9.2-budgets-sns/)).
- **NAT Gateway** ~32 USD/tháng — tài nguyên tốn phí nhất sau khi chạy workshop; xóa sớm khi không dùng ([5.10](../5.10-cleanup/)).

#### Alarm & budget gợi ý (workshop)

| Alarm | Ngưỡng gợi ý | Ai xử lý |
| --- | --- | --- |
| ALB `UnHealthyHostCount` | ≥ 1 trong 5 phút | Toàn |
| Lambda `Errors` | ≥ 1 | Hoa |
| RDS `CPUUtilization` | > 80% *(db.t3.micro)* | Kiệt |
| AWS Budgets | Email khi vượt ngưỡng nhóm đặt | Cả nhóm |

Frontend (khi app chạy): `GET /api/aws/status` — trạng thái RDS, S3, **Bedrock Mantle**.

#### Nội dung

1. [CloudWatch Logs & Alarms](5.9.1-cloudwatch/)
2. [AWS Budgets & SNS](5.9.2-budgets-sns/)
3. [Debug theo thành viên](5.9.3-debug-by-member/)

{{% notice info %}}
Mục 5.9 **không bắt buộc** tạo đủ alarm trước demo — ưu tiên log + Step Fn history khi debug. Budgets/SNS nên bật sớm để tránh quên NAT/WAF chạy qua đêm.
{{% /notice %}}
