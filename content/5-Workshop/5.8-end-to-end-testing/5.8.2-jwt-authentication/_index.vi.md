---
title: "Xác thực JWT"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.8.2. </b> "
---

#### Đăng nhập Cognito → JWT

Đăng nhập `admin@gmail.com` → lưu **IdToken**, AccessToken, RefreshToken.

#### Gọi API không token → 401

```powershell
curl.exe https://d....cloudfront.net/api/projects
```

#### Gọi API có token → 200

```powershell
curl.exe -H "Authorization: <IdToken>" https://d....cloudfront.net/api/projects
```

{{% notice note %}}
REST API + Cognito Authorizer: token có thể gắn trực tiếp vào `Authorization` (không bắt buộc `Bearer`).
{{% /notice %}}

#### Decode token

[jwt.io](https://jwt.io) — kiểm tra `cognito:groups` = `ADMIN`.

#### Refresh Token

Dùng Refresh Token lấy IdToken mới khi hết hạn.

<!-- Hình: /images/5-Workshop/5.8-Integration/jwt.png -->
