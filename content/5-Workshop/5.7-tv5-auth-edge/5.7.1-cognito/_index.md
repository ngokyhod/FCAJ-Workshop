---
title: "Amazon Cognito"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

#### Create User Pool (Create user directory)

| Field | Value |
| --- | --- |
| **Application type** | **Single-page application (SPA)** |
| **Name** | `zerobug-spa-client` |
| **Sign-in identifiers** | Check **Email** *(cannot change after creation)* |
| **Self-registration** | Enable (or disable per project) |
| **Return URL** | `http://localhost:5173` |

→ **Create user directory**. Rename pool: **`zerobug-user-pool`** *(if needed)*.

![](/images/5-Workshop/5.7/1.png)
![](/images/5-Workshop/5.7/2.png)
![](/images/5-Workshop/5.7/3.png)
![](/images/5-Workshop/5.7/4.png)

**Additional wizard options (if extra steps appear):**

| Field | Value |
| --- | --- |
| MFA | **No MFA** (workshop) |
| Password policy | Default or minimum 8 characters |
| Email delivery | **Send email with Cognito** |
| App client | **Public client** — **do not** create client secret |

#### Cognito domain & Hosted UI

1. User Pool → **App integration** → **Domain**.
2. Choose **Cognito domain** → prefix: `zerobug-<unique-suffix>` (e.g. `zerobug-hutech01`).
3. **App client** → `zerobug-spa-client` → enable **Hosted UI** (if not already enabled).
4. **Allowed callback URLs:** `http://localhost:5173`
5. **Allowed sign-out URLs:** `http://localhost:5173`
6. **OAuth 2.0 grant types:** Authorization code grant **or** Implicit grant (SPA token).
7. **OpenID Connect scopes:** `openid`, `email`, `profile`.

#### Create Groups

**Create group** — repeat twice:

| Group | Precedence | IAM role |
| --- | --- | --- |
| `ADMIN` | `1` | **Leave blank** |
| `USER` | `10` | **Leave blank** |

Description optional. IAM role in group **not required** (only used for Identity Pool).

![](/images/5-Workshop/5.7/7.png)
![](/images/5-Workshop/5.7/8.png)
![](/images/5-Workshop/5.7/9.png)

#### Create test user

1. **Users** → **Create user** → `admin@gmail.com`, set password.
2. **Add to group** → `ADMIN`.

![](/images/5-Workshop/5.7/5.png)
![](/images/5-Workshop/5.7/6.png)

#### Save parameters

| Parameter | Example |
| --- | --- |
| User Pool ID | `ap-southeast-1_xxxxxxxxx` |
| App Client ID | *(alphanumeric string)* |
| Cognito domain | `zerobug-hutech01.auth.ap-southeast-1.amazoncognito.com` |

#### Test Hosted UI

Open in browser (replace `<domain>`, `<client_id>`):

```
https://<domain>/login?client_id=<client_id>&response_type=token&scope=email+openid+profile&redirect_uri=http://localhost:5173
```

Cognito login page appears = OK.

#### Callback after CloudFront

After section 5.7.3, add to App client:

- **Allowed callback URLs:** `https://d....cloudfront.net`
- **Allowed sign-out URLs:** `https://d....cloudfront.net`

#### Classic wizard (fallback if you do not see “Create user directory”)

1. **Create user pool** → **Step through settings**.
2. Pool name: `zerobug-user-pool`; sign-in **Email**; MFA **Off**.
3. App client: `zerobug-spa-client`, **Generate client secret: No**.
4. Domain prefix + Hosted UI as in section 5.7.1.2.
