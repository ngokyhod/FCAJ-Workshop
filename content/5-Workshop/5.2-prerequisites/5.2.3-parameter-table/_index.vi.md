---
title: "Bảng tham số"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.2.3. </b> "
---

Mỗi thành viên điền giá trị sau khi tạo xong — **chia sẻ cho cả nhóm** (Google Sheet / file chung).

| Tham số | Thành viên | Giá trị |
| --- | --- | --- |
| Account ID | — | `...` |
| VPC ID | Trí | `vpc-...` |
| Public Subnet A / B | Trí | `subnet-...` |
| Private Subnet A / B | Trí | `subnet-...` |
| Internet Gateway ID | Trí | `igw-...` |
| NAT Gateway ID | Trí | `nat-...` |
| ALB-SG / EC2-SG / RDS-SG / Lambda-SG | Trí | `sg-...` |
| Role ARN: EC2 | Trí | `arn:aws:iam::...:role/zerobug-ec2-role` |
| Role ARN: Lambda | Trí | `arn:aws:iam::...:role/zerobug-lambda-role` |
| Role ARN: StepFn | Trí | `arn:aws:iam::...:role/zerobug-stepfn-role` |
| S3 Bucket name | Kiệt | `zerobug-projects-...` |
| RDS Endpoint | Kiệt | `....rds.amazonaws.com` |
| Secret ARN (DB) | Kiệt | `arn:aws:secretsmanager:...` |
| Mantle base URL | Kiệt | `https://bedrock-mantle.us-east-1.api.aws` |
| Model ID (chat) | Kiệt | `openai.gpt-oss-120b` |
| Model ID (embedding) | Kiệt | `cohere.embed-multilingual-v3` |
| Embedding dimension | Kiệt | `1024` |
| Vector table (pgvector) | Kiệt | `code_embeddings` |
| RAG top-k | Hoa | `5` |
| Lambda Project Import | Hoa | `ProjectImportLambda` |
| Lambda Source File Service | Hoa | `SourceFileServiceLambda` |
| Lambda Context Builder | Hoa | `ContextBuilderLambda` |
| Lambda AI Invoke | Hoa | `BedrockInvokeLambda` |
| Lambda Result Service | Hoa | `ResultServiceLambda` |
| Lambda History Service | Hoa | `HistoryServiceLambda` |
| Bedrock API key env var | Hoa | `BEDROCK_MANTLE_API_KEY` |
| EC2 Instance ID | Toàn | `i-...` |
| Target Group ARN | Toàn | `arn:...:targetgroup/...` |
| **ALB DNS name** | Toàn | `zerobug-alb-....elb.amazonaws.com` |
| Step Functions ARN | Hoa | `arn:aws:states:ap-southeast-1:...` |
| User Pool ID | Trinh | `ap-southeast-1_...` |
| App Client ID | Trinh | `...` |
| Cognito domain | Trinh | `....auth.ap-southeast-1.amazoncognito.com` |
| API Gateway Invoke URL | Trinh | `https://....execute-api.../prod` |
| CloudFront domain | Trinh | `d....cloudfront.net` |

#### Thứ tự triển khai

```
Trí → Kiệt → Toàn → Hoa → Trinh → 5.8 (test chung)
```

{{% notice info %}}
Có thể **tạm dừng Toàn** (chưa deploy backend) và làm **Trinh** trước để viết báo cáo — phần test để lại mục 5.8.
{{% /notice %}}
