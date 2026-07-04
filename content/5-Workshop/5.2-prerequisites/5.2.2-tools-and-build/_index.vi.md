---
title: "Công cụ & Build"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2.2. </b> "
---

#### Công cụ cần cài

| Công cụ | Mục đích | Ai cần |
| --- | --- | --- |
| **AWS CLI** | Test, deploy trên máy dev | Tất cả |
| **Postman / curl** | Test API, JWT | Trinh, mục 5.8 |
| **Amazon Bedrock Console** | Bật model access Mantle (`us-east-1`) | Kiệt |
| **Bedrock API keys** | Token Bearer cho `ContextBuilderLambda` + `BedrockInvokeLambda` | Hoa |

#### Cấu hình AWS CLI (máy dev)

```powershell
aws configure
# Default region name: ap-southeast-1
aws sts get-caller-identity
```

{{% notice warning %}}
Không chia sẻ Secret access key. Xóa key khi xong (mục 5.10).
{{% /notice %}}

#### Build artifact

Trên máy phát triển:

```bat
build-all.bat
```

Sinh ra:

- `zerobug-agent-app-1.0.0.jar` → Toàn upload S3, deploy EC2.
- Vite SPA build → trong JAR.
- Lambda packages → Hoa *(khi có code)*.

#### Quy ước tag

- `Project=zerobug`
- `Owner=Tri` … `trinh`

<!-- Hình: /images/5-Workshop/5.2-Prerequisite/cli.png -->
