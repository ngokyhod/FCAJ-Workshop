---
title: "CloudFront"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.7.3. </b> "
---

#### Bước 1 — Get started

| Trường | Giá trị |
| --- | --- |
| **Distribution name** | `zerobug-cloudfront` *(bắt buộc — trống sẽ báo đỏ)* |
| **Distribution type** | Single website or app |
| **Route 53 managed domain** | **Để TRỐNG** |

→ **Next**.

![](/images/5-Workshop/5.7/27.png)
![](/images/5-Workshop/5.7/28.png)
![](/images/5-Workshop/5.7/29.png)

#### Bước 2 — Origin

| Trường | Giá trị |
| --- | --- |
| Origin domain | `xxxx.execute-api.ap-southeast-1.amazonaws.com` |
| Origin path | `/prod` |
| Protocol | HTTPS only |

![](/images/5-Workshop/5.7/30.png)

#### Bước 3 — Settings

- Redirect HTTP → HTTPS.
- Methods: GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE.
- **Cache policy:** CachingDisabled.
- **Origin request policy:** AllViewerExceptHostHeader.

![](/images/5-Workshop/5.7/31.png)
![](/images/5-Workshop/5.7/32.png)

#### Bước 4 — Review

- **Alternate domain (CNAME):** trống.
- WAF: Enable hoặc cấu hình riêng mục 5.7.4.
- → **Create distribution** → đợi **Deployed**.
- Copy `d....cloudfront.net` → cập nhật Cognito callback.

![](/images/5-Workshop/5.7/33.png)
