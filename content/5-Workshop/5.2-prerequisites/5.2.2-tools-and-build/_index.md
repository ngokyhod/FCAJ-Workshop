---
title: "Tools & build"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---

#### Tools to install

| Tool | Purpose | Who needs it |
| --- | --- | --- |
| **AWS CLI** | Test and deploy on dev machine | Everyone |
| **Postman / curl** | Test API, JWT | Trinh, section 5.8 |
| **Amazon Bedrock Console** | Enable Mantle model access (`us-east-1`) | Kiệt |
| **Bedrock API keys** | Bearer token for `ContextBuilderLambda` + `BedrockInvokeLambda` | Hoa |

#### AWS CLI configuration (dev machine)

```powershell
aws configure
# Default region name: ap-southeast-1
aws sts get-caller-identity
```

{{% notice warning %}}
Do not share Secret access keys. Delete keys when done (section 5.10).
{{% /notice %}}

#### Build artifacts

On the development machine:

```bat
build-all.bat
```

Produces:

- `zerobug-agent-app-1.0.0.jar` → Toàn uploads to S3, deploys on EC2.
- Vite SPA build → inside the JAR.
- Lambda packages → Hoa *(when code is available)*.

#### Tag conventions

- `Project=zerobug`
- `Owner=Tri` … `trinh`

<!-- Image: /images/5-Workshop/5.2-Prerequisite/cli.png -->
