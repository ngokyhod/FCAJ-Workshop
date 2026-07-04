---
title: "CloudFront"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.7.3. </b> "
---

#### Step 1 — Get started

| Field | Value |
| --- | --- |
| **Distribution name** | `zerobug-cloudfront` *(required — blank shows red error)* |
| **Distribution type** | Single website or app |
| **Route 53 managed domain** | **Leave BLANK** |

→ **Next**.

![](/images/5-Workshop/5.7/27.png)
![](/images/5-Workshop/5.7/28.png)
![](/images/5-Workshop/5.7/29.png)

#### Step 2 — Origin

| Field | Value |
| --- | --- |
| Origin domain | `xxxx.execute-api.ap-southeast-1.amazonaws.com` |
| Origin path | `/prod` |
| Protocol | HTTPS only |

![](/images/5-Workshop/5.7/30.png)

#### Step 3 — Settings

- Redirect HTTP → HTTPS.
- Methods: GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE.
- **Cache policy:** CachingDisabled.
- **Origin request policy:** AllViewerExceptHostHeader.

![](/images/5-Workshop/5.7/31.png)
![](/images/5-Workshop/5.7/32.png)

#### Step 4 — Review

- **Alternate domain (CNAME):** blank.
- WAF: Enable or configure separately in section 5.7.4.
- → **Create distribution** → wait for **Deployed**.
- Copy `d....cloudfront.net` → update Cognito callback.

![](/images/5-Workshop/5.7/33.png)
