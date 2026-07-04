---
title: "AWS WAF"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.7.4. </b> "
---

{{% notice warning %}}
WAF **không free tier** (~5 USD/tháng/Web ACL + phí rule).
{{% /notice %}}

Áp dụng khi **chưa bật** WAF ở bước CloudFront (mục 5.7.3). Luồng production: **Client → CloudFront → WAF → API Gateway → ALB → EC2**.

{{% notice info %}}
Để gắn WAF cho **CloudFront**, phải đổi region góc phải Console sang **ap-southeast-1 (CloudFront)** khi tạo Web ACL.
{{% /notice %}}

#### Bước 1 — Tạo Web ACL

1. Search **WAF** → **Web ACLs**.
2. Đổi region → **ap-southeast-1 (CloudFront)**.
3. **Create web ACL**.
4. **App category:** `API & intergration services`.
5. **Resources to select:** `zerobug-api-prod`.
6. **Initial protections:** `Essentials`.
7. **Name:** `zerobug-waf`.
8. → **Create**.

![](/images/5-Workshop/5.7/34.png)
![](/images/5-Workshop/5.7/35.png)
![](/images/5-Workshop/5.7/36.png)
![](/images/5-Workshop/5.7/37.png)
![](/images/5-Workshop/5.7/38.png)

#### Bước 2 — Add managed rule groups

1. **Add rules** → **Add managed rule groups**.
2. Mở **AWS managed rule groups**, bật:
   - **Core rule set (CRS)**
   - **Known bad inputs**
3. → **Add rules**.

![](/images/5-Workshop/5.7/39.png)
![](/images/5-Workshop/5.7/40.png)
![](/images/5-Workshop/5.7/41.png)
![](/images/5-Workshop/5.7/42.png)

#### Bước 3 — Add rate limit rule

1. **Add rules** → **Add my own rules** → Rule type **Rate-based rule**.
2. **Name:** `rate-limit`.
3. **Rate limit:** ví dụ **1000 request / 5 phút / IP**.
4. **Action:** **Block**.
5. → **Add rule**.

![](/images/5-Workshop/5.7/43.png)
![](/images/5-Workshop/5.7/44.png)
![](/images/5-Workshop/5.7/45.png)

#### Kiểm tra

- Distribution CloudFront đã **associate** Web ACL `zerobug-waf`.
- Khi backend sẵn sàng: gửi nhiều request vượt ngưỡng rate limit → WAF **Block** (xem **Sampled requests** trong WAF Console).
