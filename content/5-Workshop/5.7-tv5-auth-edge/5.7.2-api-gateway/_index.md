---
title: "API Gateway"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---

#### Create REST API

1. **API Gateway** → **REST API** → **Build**.
2. Name: `zerobug-api`, Endpoint: **Regional**.

![](/images/5-Workshop/5.7/10.png)
![](/images/5-Workshop/5.7/11.png)
![](/images/5-Workshop/5.7/12.png)

#### Cognito Authorizer

1. **Authorizers** → **Create**.
2. Name: `zerobug-cognito-auth`, Type **Cognito**, Token source **`Authorization`**.

![](/images/5-Workshop/5.7/13.png)
![](/images/5-Workshop/5.7/14.png)

#### Resource proxy `{proxy+}`

1. **Resources** → `/` → **Create resource**.
2. **Proxy resource:** ON.
3. **Resource name:** **`{proxy+}`** — greedy path required. **Do not** type plain `proxy` → red error.
4. **Enable CORS** → **Create**.

![](/images/5-Workshop/5.7/15.png)
![](/images/5-Workshop/5.7/16.png)

#### Method ANY → HTTP proxy ALB

1. `/{proxy+}` → **ANY** → Integration **HTTP** (proxy).
2. **Endpoint URL:**

```
http://<ALB-DNS>/{proxy}
```

Example: `http://zerobug-alb-1234567890.ap-southeast-1.elb.amazonaws.com/{proxy}`

> **ALB DNS** from Toàn (`*.elb.amazonaws.com`) — **not** EC2 subnet/IP.

3. **Method request** → Authorization: `zerobug-cognito-auth`.

![](/images/5-Workshop/5.7/17.png)
![](/images/5-Workshop/5.7/18.png)
![](/images/5-Workshop/5.7/19.png)
![](/images/5-Workshop/5.7/20.png)
![](/images/5-Workshop/5.7/21.png)
![](/images/5-Workshop/5.7/22.png)

#### (Optional) Public `/api/health`

Resource `/api/health`, GET, integration `http://<ALB-DNS>/api/health`, Authorization **NONE**.

#### CORS & Deploy

- **Enable CORS** on `{proxy+}` — headers `Authorization`, `Content-Type`.
- **Deploy API** → stage **`prod`** → copy Invoke URL.

![](/images/5-Workshop/5.7/23.png)
![](/images/5-Workshop/5.7/24.png)
![](/images/5-Workshop/5.7/25.png)
![](/images/5-Workshop/5.7/26.png)
