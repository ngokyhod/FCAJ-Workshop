---
title: "Dọn tài nguyên - Trinh"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.10.1. </b> "
---

Xóa **theo thứ tự bên dưới** — Trinh làm **đầu tiên** trong quy trình dọn dẹp (ngược với triển khai).

{{% notice warning %}}
**CloudFront** phải **Disable** và đợi **Deployed** trước khi Delete. **WAF** gỡ association trước khi xóa Web ACL.
{{% /notice %}}

#### Bước 1 — CloudFront Distribution

1. Console → **CloudFront** → **Distributions**.
2. Chọn `zerobug-cloudfront` (hoặc distribution có origin API Gateway `...execute-api...`).
3. **Disable** → đợi trạng thái **Deployed** (~5–15 phút).
4. **Delete** distribution.

![](/images/5-Workshop/5.10/1.png)

#### Bước 2 — API Gateway

1. **API Gateway** → **REST APIs** → `zerobug-api`.
2. **Stages** → stage **`prod`** → **Delete stage** (nếu bắt buộc trước khi xóa API).
3. **Resources** → xóa `/{proxy+}` và `/api/health` (nếu tạo).
4. **Authorizers** → xóa `zerobug-cognito-auth`.
5. **Actions** → **Delete API** → xác nhận `zerobug-api`.

![](/images/5-Workshop/5.10/2.png)
![](/images/5-Workshop/5.10/3.png)
![](/images/5-Workshop/5.10/4.png)
![](/images/5-Workshop/5.10/5.png)
![](/images/5-Workshop/5.10/6.png)


#### Bước 3 — Amazon Cognito

1. **Cognito** → **User pools** → `zerobug-user-pool`.
2. **App clients** → xóa `zerobug-spa-client`.
3. **Groups** → xóa `ADMIN`, `USER`.
4. **Users** → xóa user test (`admin@gmail.com`, …).
5. **Delete user pool** `zerobug-user-pool`.

![](/images/5-Workshop/5.10/7.png)
![](/images/5-Workshop/5.10/8.png)
![](/images/5-Workshop/5.10/9.png)
![](/images/5-Workshop/5.10/10.png)

#### Bước 4 — CloudWatch Logs (tùy chọn)

1. **CloudWatch** → **Log groups**.
2. Xóa các log group prefix `/aws/apigateway/zerobug-api`, `/aws/waf/...` (nếu có).

#### Checklist xác nhận

- [x] Không còn CloudFront distribution ZeroBug
- [x] Không còn Web ACL `zerobug-waf` đang associate
- [x] Không còn REST API `zerobug-api`
- [x] Không còn User Pool `zerobug-user-pool`

→ Tiếp: [Hoa — Serverless](5.10.2-hoa/)
