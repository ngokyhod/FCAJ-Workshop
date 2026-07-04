---
title: "Kiểm tra & bàn giao"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.7.5. </b> "
---

#### Checklist Trinh (làm được ngay)

- [x] Cognito Hosted UI mở trang login (URL mục 5.7.1.6).
- [x] API Gateway stage **`prod`** deployed; Invoke URL đã copy.
- [x] CloudFront **Deployed**; domain `d....cloudfront.net` đã copy.
- [x] WAF **Essentials** gắn **`zerobug-api - prod`** (và/hoặc CloudFront nếu dùng Cách B).
- [x] `https://d....cloudfront.net/api/health` có thể **502** nếu EC2 chưa chạy — **bình thường**.

#### Để mục 5.8 (khi backend + Hoa xong)

- [x] Đăng nhập Cognito → lấy JWT → gọi API qua CloudFront.
- [x] Không token → **401**; có token hợp lệ → **200**.
- [x] Refresh Token hoạt động.
- [x] Generate Test end-to-end qua Step Functions.

#### Bàn giao Frontend / Electron

| Tham số | Mô tả | Ví dụ |
| --- | --- | --- |
| **CloudFront domain** | URL production SPA | `https://dxxxx.cloudfront.net` |
| **API Gateway Invoke URL** | Base API (nếu gọi trực tiếp) | `https://xxxx.execute-api.ap-southeast-1.amazonaws.com/prod` |
| **User Pool ID** | Cognito pool | `ap-southeast-1_...` |
| **App Client ID** | SPA public client, **no secret** | `...` |
| **Cognito domain** | Hosted UI | `zerobug-xxx.auth.ap-southeast-1.amazoncognito.com` |

#### Hosted UI URL mẫu

```
https://<cognito-domain>/login?client_id=<App-Client-ID>&response_type=token&scope=email+openid+profile&redirect_uri=https://d....cloudfront.net
```

Cập nhật [bảng tham số](../../5.2-prerequisites/5.2.3-parameter-table/) — cột Trinh.
