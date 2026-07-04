---
title: "Kiểm tra & bàn giao"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.5.6. </b> "
---

#### Checklist (khi backend sẵn sàng)

- [x] `systemctl status zerobug` = active.
- [x] `curl localhost:8080/api/health` OK.
- [x] Target **healthy**.
- [x] `http://<ALB-DNS>/api/health` OK.

#### Bàn giao Trinh

**ALB DNS name** — dùng trong API Gateway:

```
http://<ALB-DNS>/{proxy}
```

Đây là DNS ALB (`*.elb.amazonaws.com`), **không phải** subnet.

→ Tiếp: [Trinh — Xác thực & Edge](../../5.7-tv5-auth-edge/).
