---
title: "AWS WAF"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.7.4. </b> "
---

{{% notice warning %}}
WAF is **not free tier** (~5 USD/month/Web ACL + rule fees).
{{% /notice %}}

Apply when WAF was **not enabled** in the CloudFront step (section 5.7.3). Production flow: **Client → CloudFront → WAF → API Gateway → ALB → EC2**.

{{% notice info %}}
To attach WAF to **CloudFront**, switch the Console region (top right) to **ap-southeast-1 (CloudFront)** when creating the Web ACL.
{{% /notice %}}

#### Step 1 — Create Web ACL

1. Search **WAF** → **Web ACLs**.
2. Change region → **ap-southeast-1 (CloudFront)**.
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

#### Step 2 — Add managed rule groups

1. **Add rules** → **Add managed rule groups**.
2. Expand **AWS managed rule groups**, enable:
   - **Core rule set (CRS)**
   - **Known bad inputs**
3. → **Add rules**.

![](/images/5-Workshop/5.7/39.png)
![](/images/5-Workshop/5.7/40.png)
![](/images/5-Workshop/5.7/41.png)
![](/images/5-Workshop/5.7/42.png)

#### Step 3 — Add rate limit rule

1. **Add rules** → **Add my own rules** → Rule type **Rate-based rule**.
2. **Name:** `rate-limit`.
3. **Rate limit:** e.g. **1000 requests / 5 minutes / IP**.
4. **Action:** **Block**.
5. → **Add rule**.

![](/images/5-Workshop/5.7/43.png)
![](/images/5-Workshop/5.7/44.png)
![](/images/5-Workshop/5.7/45.png)

#### Verification

- CloudFront distribution is **associated** with Web ACL `zerobug-waf`.
- When backend is ready: send requests exceeding the rate limit → WAF **Block** (see **Sampled requests** in WAF Console).
