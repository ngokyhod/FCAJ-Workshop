---
title: "IAM Roles"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

Create **3 Roles** first — Toàn attaches to EC2, Hoa attaches to Lambda & Step Functions.

#### EC2 Role — `zerobug-ec2-role`

1. IAM → **Roles** → **Create role**.
2. Trusted entity: **EC2** → **Next**.
3. Attach managed policies:
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

#### Lambda Role — `zerobug-lambda-role`

1. **Create role** → **Lambda** → **Next**.
2. Attach:
   - `AWSLambdaVPCAccessExecutionRole`
   - `AmazonS3FullAccess`
   - `SecretsManagerReadWrite`
3. **Next** → Role name: `zerobug-lambda-role` → **Create role**.

{{% notice info %}}
**Bedrock Mantle — Bearer API key authentication:** Lambda **`BedrockInvokeLambda`** and **`ContextBuilderLambda`** call Mantle with header `Authorization: Bearer <BEDROCK_MANTLE_API_KEY>` (environment variable on Lambda, **not** stored in Secrets Manager). Role **`zerobug-lambda-role`** **does not need** `bedrock:InvokeModel` policy or Bedrock IAM permissions — only VPC + S3 + Secrets (DB) as above.
{{% /notice %}}

#### Step Functions Role — `zerobug-stepfn-role`

1. **Create role** → **Step Functions** → **Next**.
2. Attach `AWSLambdaRole`.
3. **Next** → Role name: `zerobug-stepfn-role` → **Create role**.

#### Handoff

Open each role → copy **ARN** into the parameter table [5.2.3](../../5.2-prerequisites/5.2.3-parameter-table/).

> These are broad permissions for the workshop. A production deployment can tighten to least-privilege later.

<!-- Image: /images/5-Workshop/5.3-Trí/iam-roles.png -->
