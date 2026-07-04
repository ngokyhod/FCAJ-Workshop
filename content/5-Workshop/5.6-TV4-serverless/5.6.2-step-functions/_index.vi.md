---
title: "Tích hợp AWS Step Functions"
date: 2026-07-03
weight: 2
chapter: false
pre: " <b> 5.6.2 </b> "
---

Sau khi đã xây dựng xong 5 hàm Lambda độc lập, chúng ta cần một "nhạc trưởng" để xâu chuỗi chúng thành một quy trình tự động. Đó chính là vai trò của **AWS Step Functions**.

#### 1. Xây dựng Workflow (State Machine)
Thay vì để ứng dụng Spring Boot (trên EC2) phải gọi tuần tự từng hàm Lambda và tự quản lý trạng thái, chúng ta nhường lại toàn bộ khối lượng công việc này cho Step Functions. 

Workflow được thiết kế chạy theo luồng tuyến tính:
`Import Project` → `Build Context (RAG)` → `File Tree` → `AI Invoke` → `Result And History`.

![](/images/5-Workshop/5.6/16.png)

#### 2. Cơ chế chịu lỗi (Error Handling & Retry)
Giao tiếp với các mô hình AI (LLMs) thường tiềm ẩn độ trễ cao hoặc lỗi mạng tạm thời (Timeout/Rate Limit). Trong Step Functions, chúng ta cấu hình một cơ chế **Retrier** đặc biệt cho bước `AI Invoke`:

* **Errors:** Bắt các lỗi `States.Timeout` và `States.TaskFailed`.
* **Interval:** Chờ 2 giây trước khi thử lại lần đầu tiên.
* **Max attempts:** Cho phép thử lại tối đa 3 lần.
* **Backoff rate (2.0):** Tăng gấp đôi thời gian chờ sau mỗi lần thất bại (2s → 4s → 8s) để giảm tải cho hệ thống AI.

![](/images/5-Workshop/5.6/17.png)

