---
title: "Verification & Handoff"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.7.5. </b> "
---

#### Trinh checklist (can do immediately)

- [x] Cognito Hosted UI opens login page (URL in section 5.7.1.6).
- [x] API Gateway stage **`prod`** deployed; Invoke URL copied.
- [x] CloudFront **Deployed**; domain `d....cloudfront.net` copied.
- [x] WAF **Essentials** attached to **`zerobug-api - prod`** (and/or CloudFront if using Option B).
- [x] `https://d....cloudfront.net/api/health` may return **502** if EC2 is not running — **normal**.

#### For section 5.8 (when backend + Hoa are done)

- [x] Sign in Cognito → get JWT → call API via CloudFront.
- [x] No token → **401**; valid token → **200**.
- [x] Refresh Token works.
- [x] Generate Test end-to-end via Step Functions.

#### Handoff to Frontend / Electron

| Parameter | Description | Example |
| --- | --- | --- |
| **CloudFront domain** | Production SPA URL | `https://dxxxx.cloudfront.net` |
| **API Gateway Invoke URL** | Base API (if calling directly) | `https://xxxx.execute-api.ap-southeast-1.amazonaws.com/prod` |
| **User Pool ID** | Cognito pool | `ap-southeast-1_...` |
| **App Client ID** | SPA public client, **no secret** | `...` |
| **Cognito domain** | Hosted UI | `zerobug-xxx.auth.ap-southeast-1.amazoncognito.com` |

#### Sample Hosted UI URL

```
https://<cognito-domain>/login?client_id=<App-Client-ID>&response_type=token&scope=email+openid+profile&redirect_uri=https://d....cloudfront.net
```

Update the [parameter table](../../5.2-prerequisites/5.2.3-parameter-table/) — Trinh column.
