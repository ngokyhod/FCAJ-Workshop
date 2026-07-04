---
title: "IAM & Billing"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.10.6. </b> "
---

Final step — clean up **IAM**, **Budgets/SNS** (if created in section 5.9) and confirm **no hourly charges remain**.

#### Step 1 — IAM Access Keys

1. **IAM** → **Users** → each user `tri`, `kiet`, `toan`, `hoa`, `trinh`.
2. Tab **Security credentials** → **Access keys** → **Deactivate** / **Delete** (if CLI keys were created).

#### Step 2 — IAM Group & Inline Policy

1. **User groups** → `zerobug-team`.
2. **Permissions** → detach **`PowerUserAccess`**.
3. Delete inline policy **`zerobug-ec2-passrole`**.
4. **Remove users** from group → **Delete group** `zerobug-team`.

#### Step 3 — IAM Users (optional)

If account is workshop-only:

1. **Users** → delete one by one `tri`, `kiet`, `toan`, `hoa`, `trinh`.
2. Confirm no MFA device / login profile needs removal first.

{{% notice note %}}
Keep IAM Users if account is used for other purposes — only delete Access Keys and ZeroBug group.
{{% /notice %}}

#### Step 4 — AWS Budgets & SNS (section 5.9)

1. **AWS Budgets** → delete ZeroBug alert budget.
2. **SNS** → **Subscriptions** — unsubscribe test email.
3. **SNS** → **Topics** — delete alarm topic (if created).
4. **CloudWatch** → **Alarms** — delete ZeroBug alarms.

#### Step 5 — Billing check

1. **Billing** → **Bills** — current month: no **NAT Gateway**, **Elastic IP**, **RDS**, **EC2 running**, **WAF**, **CloudFront** line items *(may prorate a few hours)*.
2. **Cost Explorer** → filter by service — confirm $0 running after 24–48h.
3. **Bedrock** → **API keys** (region `us-east-1`) → revoke/delete **workshop key** *(one key shared via env `BEDROCK_MANTLE_API_KEY` on **`ContextBuilderLambda`** and **`BedrockInvokeLambda`*)*.

#### Overall checklist (entire team)

| Member | Deleted |
| --- | --- |
| Trinh | CloudFront, WAF, API GW, Cognito |
| Hoa | Step Functions, 6 Lambda, log groups |
| Toàn | ALB, Target Group, EC2, key pair |
| Kiệt | RDS, subnet group, Secrets, S3 |
| Trí | NAT, EIP, IGW, SG, subnet, VPC, roles |
| Shared | IAM group/users/keys, Budgets, Alarms |

{{% notice success %}}
When the checklist is complete, the ZeroBug Agent workshop on AWS has been fully cleaned up.
{{% /notice %}}
