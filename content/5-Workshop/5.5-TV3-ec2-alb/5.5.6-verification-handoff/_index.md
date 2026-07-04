---
title: "Verification & Handoff"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.5.6. </b> "
---

#### Checklist (when backend is ready)

- [x] `systemctl status zerobug` = active.
- [x] `curl localhost:8080/api/health` OK.
- [x] Target **healthy**.
- [x] `http://<ALB-DNS>/api/health` OK.

#### Handoff to Trinh

**ALB DNS name** — used in API Gateway:

```
http://<ALB-DNS>/{proxy}
```

This is the ALB DNS (`*.elb.amazonaws.com`), **not** a subnet.

→ Next: [Trinh — Auth & Edge](../../5.7-tv5-auth-edge/).
