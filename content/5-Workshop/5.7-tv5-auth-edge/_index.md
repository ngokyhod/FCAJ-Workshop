---
title: "Trinh – Auth & Edge"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

**Owner:** Trinh  
**Depends on Toàn:** **ALB DNS name** (required before API Gateway).

#### What does Trinh do?

Deploy the **authentication and edge layer**: Cognito, API Gateway, CloudFront, WAF — **no Route 53**.

#### Components & where they are used

| Component | Function | Used where |
| --- | --- | --- |
| **Cognito** | Sign-in, JWT | SPA/Electron; API GW Authorizer |
| **API Gateway** | REST + `{proxy+}` → ALB | Client API entry point |
| **CloudFront** | CDN HTTPS `*.cloudfront.net` | Production Frontend URL |
| **WAF** | Core rules, rate limit | Protect API GW / CloudFront |

```
Client → CloudFront → WAF → API Gateway (JWT) → ALB → EC2
```

#### Trinh-specific preparation

- **ALB DNS** from Toàn: EC2 → Load Balancers → `zerobug-alb` → DNS name.
- Postman/curl for JWT testing (section 5.8).
- WAF for CloudFront uses the **Global** region if you choose Option B (section 5.7.4).

{{% notice info %}}
You can **build the full Trinh stack** before the backend is ready. `https://d....cloudfront.net/api/health` returning **502** is **normal**.
{{% /notice %}}

#### Deployment content

1. [Amazon Cognito](5.7.1-cognito/)
2. [Amazon API Gateway](5.7.2-api-gateway/)
3. [CloudFront Distribution](5.7.3-cloudfront/)
4. [AWS WAF](5.7.4-waf/)
5. [Verification & Handoff](5.7.5-verification-handoff/)
