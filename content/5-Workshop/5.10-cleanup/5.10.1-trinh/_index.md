---
title: "Resource Cleanup – Trinh"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.10.1. </b> "
---

Delete **in the order below** — Trinh goes **first** in the cleanup process (reverse of deployment).

{{% notice warning %}}
**CloudFront** must be **Disabled** and wait for **Deployed** before Delete. **WAF** — remove association before deleting Web ACL.
{{% /notice %}}

#### Step 1 — CloudFront Distribution

1. Console → **CloudFront** → **Distributions**.
2. Select `zerobug-cloudfront` (or distribution with API Gateway origin `...execute-api...`).
3. **Disable** → wait for status **Deployed** (~5–15 minutes).
4. **Delete** distribution.

![](/images/5-Workshop/5.10/1.png)

#### Step 2 — API Gateway

1. **API Gateway** → **REST APIs** → `zerobug-api`.
2. **Stages** → stage **`prod`** → **Delete stage** (if required before deleting API).
3. **Resources** → delete `/{proxy+}` and `/api/health` (if created).
4. **Authorizers** → delete `zerobug-cognito-auth`.
5. **Actions** → **Delete API** → confirm `zerobug-api`.

![](/images/5-Workshop/5.10/2.png)
![](/images/5-Workshop/5.10/3.png)
![](/images/5-Workshop/5.10/4.png)
![](/images/5-Workshop/5.10/5.png)
![](/images/5-Workshop/5.10/6.png)


#### Step 3 — Amazon Cognito

1. **Cognito** → **User pools** → `zerobug-user-pool`.
2. **App clients** → delete `zerobug-spa-client`.
3. **Groups** → delete `ADMIN`, `USER`.
4. **Users** → delete test users (`admin@gmail.com`, …).
5. **Delete user pool** `zerobug-user-pool`.

![](/images/5-Workshop/5.10/7.png)
![](/images/5-Workshop/5.10/8.png)
![](/images/5-Workshop/5.10/9.png)
![](/images/5-Workshop/5.10/10.png)

#### Step 4 — CloudWatch Logs (optional)

1. **CloudWatch** → **Log groups**.
2. Delete log groups with prefix `/aws/apigateway/zerobug-api`, `/aws/waf/...` (if any).

#### Confirmation checklist

- [x] No ZeroBug CloudFront distribution remaining
- [x] No Web ACL `zerobug-waf` still associated
- [x] No REST API `zerobug-api` remaining
- [x] No User Pool `zerobug-user-pool` remaining

→ Next: [Hoa — Serverless](5.10.2-hoa/)
