---
title: "Amazon Cognito"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

#### Tạo User Pool (Create user directory)

| Trường | Điền |
| --- | --- |
| **Application type** | **Single-page application (SPA)** |
| **Name** | `zerobug-spa-client` |
| **Sign-in identifiers** | Tích **Email** *(không đổi sau khi tạo)* |
| **Self-registration** | Bật (hoặc tắt tùy đồ án) |
| **Return URL** | `http://localhost:5173` |

→ **Create user directory**. Đổi tên pool: **`zerobug-user-pool`** *(nếu cần)*.

![](/images/5-Workshop/5.7/1.png)
![](/images/5-Workshop/5.7/2.png)
![](/images/5-Workshop/5.7/3.png)
![](/images/5-Workshop/5.7/4.png)

**Các tùy chọn wizard (nếu hiện thêm bước):**

| Trường | Giá trị |
| --- | --- |
| MFA | **No MFA** (workshop) |
| Password policy | Mặc định hoặc tối thiểu 8 ký tự |
| Email delivery | **Send email with Cognito** |
| App client | **Public client** — **không** tạo client secret |

#### Cognito domain & Hosted UI

1. User Pool → **App integration** → **Domain**.
2. Chọn **Cognito domain** → prefix: `zerobug-<suffix-duy-nhat>` (ví dụ `zerobug-hutech01`).
3. **App client** → `zerobug-spa-client` → bật **Hosted UI** (nếu chưa bật).
4. **Allowed callback URLs:** `http://localhost:5173`
5. **Allowed sign-out URLs:** `http://localhost:5173`
6. **OAuth 2.0 grant types:** Authorization code grant **hoặc** Implicit grant (SPA token).
7. **OpenID Connect scopes:** `openid`, `email`, `profile`.

#### Tạo Groups

**Create group** — lặp 2 lần:

| Group | Precedence | IAM role |
| --- | --- | --- |
| `ADMIN` | `1` | **Để trống** |
| `USER` | `10` | **Để trống** |

Description tùy chọn. IAM role trong group **không cần** (chỉ dùng cho Identity Pool).

![](/images/5-Workshop/5.7/7.png)
![](/images/5-Workshop/5.7/8.png)
![](/images/5-Workshop/5.7/9.png)

#### Tạo user test

1. **Users** → **Create user** → `admin@gmail.com`, đặt password.
2. **Add to group** → `ADMIN`.

![](/images/5-Workshop/5.7/5.png)
![](/images/5-Workshop/5.7/6.png)

#### Lưu tham số

| Tham số | Ví dụ |
| --- | --- |
| User Pool ID | `ap-southeast-1_xxxxxxxxx` |
| App Client ID | *(chuỗi alphanumeric)* |
| Cognito domain | `zerobug-hutech01.auth.ap-southeast-1.amazoncognito.com` |

#### Test Hosted UI

Mở trình duyệt (thay `<domain>`, `<client_id>`):

```
https://<domain>/login?client_id=<client_id>&response_type=token&scope=email+openid+profile&redirect_uri=http://localhost:5173
```

Trang login Cognito hiện ra = OK.

#### Callback sau CloudFront

Sau mục 5.7.3, thêm vào App client:

- **Allowed callback URLs:** `https://d....cloudfront.net`
- **Allowed sign-out URLs:** `https://d....cloudfront.net`

#### Wizard classic (fallback nếu không thấy “Create user directory”)

1. **Create user pool** → **Step through settings**.
2. Pool name: `zerobug-user-pool`; sign-in **Email**; MFA **Off**.
3. App client: `zerobug-spa-client`, **Generate client secret: No**.
4. Domain prefix + Hosted UI như mục 5.7.1.2.
