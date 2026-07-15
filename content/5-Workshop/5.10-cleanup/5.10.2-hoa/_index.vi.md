---
title: "Dọn tài nguyên - Hoa"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.10.2. </b> "
---

Xóa **Step Functions** trước **Lambda** (state machine tham chiếu Lambda). Xóa **CloudWatch Log groups** của Lambda sau cùng trong mục này.

Tên function theo [bảng tham số](../../5.2-prerequisites/5.2.3-parameter-table/) — **PascalCase**, không dùng prefix `zerobug-` cho Lambda.

#### Bước 1 — Step Functions State Machine

1. **Step Functions** → **State machines**.
2. Chọn state machine ZeroBug (ví dụ `zerobug-workflow` hoặc tên nhóm đã đặt).
3. **Stop** mọi execution đang **Running** (tab **Executions**).
4. **Delete state machine**.

#### Bước 2 — Lambda Functions (6 function)

**Lambda** → **Functions** — xóa lần lượt:

| Function | Ghi chú |
| --- | --- |
| `ProjectImportLambda` | Project Import |
| `SourceFileServiceLambda` | Source File Service |
| `ContextBuilderLambda` | Context Builder + RAG embed/retrieve |
| `BedrockInvokeLambda` | AI Invoke (Bedrock Mantle chat) |
| `ResultServiceLambda` | Result Service |
| `HistoryServiceLambda` | History Service |

Với từng function:

1. **Configuration** → **Triggers** → gỡ mọi trigger (nếu có).
2. **Actions** → **Delete** → gõ `delete` xác nhận.

{{% notice tip %}}
Nếu **Delete** báo lỗi do Step Functions: quay lại Bước 1, đảm bảo state machine đã xóa.
{{% /notice %}}

#### Bước 3 — Lambda Layers / Versions (nếu có)

1. **Lambda** → **Layers** — xóa layer custom ZeroBug (nếu tạo).
2. Tab **Versions** trên từng function — thường tự xóa khi delete function.

#### Bước 4 — CloudWatch Log groups

**CloudWatch** → **Log groups** → xóa:

- `/aws/lambda/ProjectImportLambda`
- `/aws/lambda/SourceFileServiceLambda`
- `/aws/lambda/ContextBuilderLambda`
- `/aws/lambda/BedrockInvokeLambda`
- `/aws/lambda/ResultServiceLambda`
- `/aws/lambda/HistoryServiceLambda`
- `/aws/states/zerobug-workflow` *(hoặc prefix state machine)*

#### Bước 5 — EventBridge / SNS (nếu Hoa tạo thêm)

Xóa rule/schedule test invoke Lambda (nếu có trong workshop mở rộng).

#### Checklist xác nhận

- [ ] Không còn state machine ZeroBug
- [ ] Không còn 6 Lambda function
- [ ] Log groups Lambda/Step Functions đã xóa

→ Tiếp: [Toàn — EC2 & ALB](5.10.3-toan/)
