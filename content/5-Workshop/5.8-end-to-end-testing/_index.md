---
title: "End-to-end Testing"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

**Shared section — entire team** runs this after **Toàn** (EC2 + ALB healthy) and **Hoa** (Step Functions + Lambda) are complete. **Trinh** can build the edge layer first; some tests may still return **502/401** until backend + serverless are running — that is normal.

#### Goals

Verify the ZeroBug Agent flow runs end-to-end on **one AWS account**:

```
Client (SPA / Electron)
  → CloudFront → WAF → API Gateway (JWT)
    → ALB → EC2 (Spring Boot)
      → Step Functions
        → ProjectImportLambda → S3
        → ContextBuilderLambda (embed + pgvector retrieve)
        → BedrockInvokeLambda (chat Mantle)
        → ResultServiceLambda / HistoryServiceLambda → RDS
```

#### Prerequisites before testing

| Member | Must be complete |
| --- | --- |
| Trí | NAT Available; private route → NAT; EC2/Lambda SG outbound |
| Kiệt | S3, RDS, DB Secret; pgvector + `code_embeddings`; Bedrock model access (chat + embedding) |
| Toàn | JAR running via systemd; Target Group **healthy**; ALB DNS copied |
| Hoa | Step Fn runs success; 6 Lambda deployed; Mantle HTTP **200** (test invoke) |
| Trinh | Cognito, API GW `prod`, CloudFront **Deployed**, WAF attached |

Parameters from the [shared table](../5.2-prerequisites/5.2.3-parameter-table/): CloudFront domain, Cognito client, ALB DNS, Step Functions ARN.

#### Workshop “pass” criteria

- [x] Sign in Cognito → valid JWT ([5.8.2](5.8.2-jwt-authentication/))
- [x] `GET /api/health` via CloudFront → **200**
- [x] Import project → file on S3; vectors in `code_embeddings`
- [x] **Generate Test** → Step Functions **Succeeded**; Unit Test result shown in UI
- [x] `GET /api/aws/status` reports RDS, S3, Bedrock Mantle OK

#### Content

1. [Checklist by member](5.8.1-checklist/)
2. [JWT authentication testing](5.8.2-jwt-authentication/)
3. [Business scenarios](5.8.3-business-scenarios/)
4. [Common troubleshooting](5.8.4-troubleshooting/)

{{% notice tip %}}
On error → see [5.8.4](5.8.4-troubleshooting/) by symptom; deeper debugging in [5.9](../5.9-monitoring-operations/). After pass, proceed to [5.10 — Cleanup](../5.10-cleanup/).
{{% /notice %}}
