---
title: "IAM Roles"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

Tạo **3 Role** trước — Toàn gắn vào EC2, Hoa gắn vào Lambda & Step Functions.

#### Role EC2 — `zerobug-ec2-role`

1. IAM → **Roles** → **Create role**.
2. Trusted entity: **EC2** → **Next**.
3. Gắn managed policies:
   - `AmazonS3FullAccess`
   - `SecretsManagerReadWrite`
   - `AWSStepFunctionsFullAccess`
   - `CloudWatchAgentServerPolicy`
   - `AmazonSSMManagedInstanceCore` *(Toàn — Session Manager)*
4. **Next** → Role name: `zerobug-ec2-role` → **Create role**.

![](/images/5-Workshop/5.3/8.jpg)
![](/images/5-Workshop/5.3/9.jpg)
![](/images/5-Workshop/5.3/10.jpg)
![](/images/5-Workshop/5.3/11.jpg)
![](/images/5-Workshop/5.3/12.jpg)
![](/images/5-Workshop/5.3/13.jpg)

#### Role Lambda — `zerobug-lambda-role`

1. **Create role** → **Lambda** → **Next**.
2. Gắn:
   - `AWSLambdaVPCAccessExecutionRole`
   - `AmazonS3FullAccess`
   - `SecretsManagerReadWrite`
3. **Next** → Role name: `zerobug-lambda-role` → **Create role**.

{{% notice info %}}
**Bedrock Mantle — xác thực Bearer API key:** Lambda **`BedrockInvokeLambda`** và **`ContextBuilderLambda`** gọi Mantle bằng header `Authorization: Bearer <BEDROCK_MANTLE_API_KEY>` (biến môi trường trên Lambda, **không** lưu trong Secrets Manager). Role **`zerobug-lambda-role`** **không cần** policy `bedrock:InvokeModel` hay quyền Bedrock IAM — chỉ cần VPC + S3 + Secrets (DB) như trên.
{{% /notice %}}

#### Role Step Functions — `zerobug-stepfn-role`

1. **Create role** → **Step Functions** → **Next**.
2. Gắn `AWSLambdaRole`.
3. **Next** → Role name: `zerobug-stepfn-role` → **Create role**.

#### Bàn giao

Mở từng role → copy **ARN** vào bảng tham số [5.2.3](../../5.2-prerequisites/5.2.3-parameter-table/).

> Đây là quyền rộng cho workshop. Bản production có thể siết least-privilege sau.

<!-- Hình: /images/5-Workshop/5.3-Trí/iam-roles.png -->
