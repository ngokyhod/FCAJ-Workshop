---
title: "Chuẩn bị chung"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

Chuẩn bị môi trường và quy ước **trước khi Trí → Kiệt → Toàn → Hoa → Trinh** triển khai. Mọi bước chuẩn bị **dùng chung** nằm ở đây — từng thành viên chỉ ghi thêm phần **phụ thuộc riêng** trên trang overview của mình.

#### Checklist nhanh

- [x] 1 account AWS chung + IAM User `tri`, `kiet`, `toan`, `hoa`, `trinh`
- [x] Region **`ap-southeast-1`**
- [x] [Bảng tham số](5.2.3-parameter-table/) sẵn sàng điền
- [x] PassRole policy đã gắn group `zerobug-team`
- [x] `build-all.bat` chạy được *(Toàn/Hoa)*

#### Nội dung

1. [Tài khoản AWS & Region](5.2.1-aws-account/)
2. [Công cụ & Build artifact](5.2.2-tools-and-build/)
3. [Bảng tham số dùng chung](5.2.3-parameter-table/)
4. [Quy ước & thứ tự triển khai](5.2.4-conventions-and-order/)
