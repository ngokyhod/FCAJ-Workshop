---
title: "Budgets & SNS"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.9.2. </b> "
---

#### AWS Budgets & SNS

- **AWS Budgets:** cảnh báo chi phí vượt ngưỡng (NAT, WAF, EC2, RDS).
- **SNS:** email khi CloudWatch Alarm kích hoạt.
- Theo dõi **Amazon Bedrock** (Mantle inference — chat + embedding) trong **AWS Billing / Cost Explorer** — cùng bill với NAT, EC2, RDS.

#### Bedrock API key (Hoa)

- Key tạo trên Bedrock Console → **API keys**, TTL mặc định **~30 ngày**.
- Cả **`ContextBuilderLambda`** và **`BedrockInvokeLambda`** dùng biến **`BEDROCK_MANTLE_API_KEY`** — hết hạn → **401** trên mọi gọi Mantle.
- **Trước khi hết hạn:** tạo key mới → cập nhật env **cả hai** Lambda → test invoke → vô hiệu hóa key cũ.
- Ghi lịch rotate trong nhóm (calendar / bảng tham số) — workshop không lưu key trong Secrets Manager.

#### Gợi ý theo dõi chi phí Mantle

| Hạng mục | Ghi chú |
| --- | --- |
| Chat (`openai.gpt-oss-120b`) | Mỗi lần sinh Unit Test qua Step Functions |
| Embeddings (`cohere.embed-multilingual-v3`) | Mỗi chunk khi import/re-index source |
| NAT | Egress cross-region `ap-southeast-1` → `us-east-1` |

<!-- Hình: /images/5-Workshop/5.9-Monitoring/budgets.png -->
