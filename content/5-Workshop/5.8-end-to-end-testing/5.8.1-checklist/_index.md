---
title: "Checklist"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.8.1. </b> "
---

| Member | Confirm |
| --- | --- |
| Trí | NAT Available; private route; EC2-SG outbound |
| Kiệt | S3; RDS; DB Secret; pgvector + `code_embeddings`; Bedrock model access (chat + embedding) |
| Toàn | `/api/health` via ALB; Target healthy |
| Hoa | Step Fn success; Context Builder embed/retrieve; Bedrock Mantle chat HTTP 200 |
| Trinh | Cognito + CloudFront + WAF |

Full flow:

```
Client → CloudFront → WAF → API GW → ALB → EC2 → Step Fn
  → ContextBuilderLambda (embed + pgvector retrieve)
  → BedrockInvokeLambda (chat) → RDS/S3
```
