---
title: "JWT Authentication"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.8.2. </b> "
---

#### Sign in Cognito → JWT

Sign in as `admin@gmail.com` → save **IdToken**, AccessToken, RefreshToken.

#### Call API without token → 401

```powershell
curl.exe https://d....cloudfront.net/api/projects
```

#### Call API with token → 200

```powershell
curl.exe -H "Authorization: <IdToken>" https://d....cloudfront.net/api/projects
```

{{% notice note %}}
REST API + Cognito Authorizer: token can be sent directly in `Authorization` (`Bearer` prefix not required).
{{% /notice %}}

#### Decode token

[jwt.io](https://jwt.io) — verify `cognito:groups` = `ADMIN`.

#### Refresh Token

Use Refresh Token to obtain a new IdToken when expired.

<!-- Image: /images/5-Workshop/5.8-Integration/jwt.png -->
