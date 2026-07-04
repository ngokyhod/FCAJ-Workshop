---
title: "Dọn dẹp tài nguyên – Hoa"
date: 2026-07-03
weight: 2
chapter: false
pre: " <b> 5.10.2. </b> "
---

Sau khi đã hoàn thành nghiệm thu hệ thống, chúng ta cần tiến hành dọn dẹp (Cleanup) các tài nguyên Serverless đã tạo để tránh phát sinh chi phí lưu trữ và vận hành ngoài ý muốn. Hãy thực hiện tuần tự theo các bước dưới đây.

#### Bước 1 — Xóa các hàm Lambda (5 hàm)

Chúng ta sẽ tiến hành xóa toàn bộ 5 hàm Lambda đã tạo trong bài 5.6. Danh sách các hàm cần xóa bao gồm:

* `ProjectImportLambda`
* `FileTreeLambda`
* `RagContextLambda`
* `BedrockInvokeLambda`
* `ResultAndHistoryLambda`

**Thao tác thực hiện:**
1. Truy cập dịch vụ **Lambda** → chọn **Functions**.
2. Tại danh sách hàm, tích chọn hàm cần xóa (ví dụ `alllambda` hoặc các hàm của dự án).
3. Nhấn vào nút **Actions** (Hành động) ở góc trên bên phải → chọn **Delete** (Xóa).

![](/images/5-Workshop/5.6/33.png)

4. Một bảng cảnh báo sẽ hiện ra. Bạn cần gõ chữ `confirm` vào ô trống để xác nhận việc xóa vĩnh viễn mã nguồn và cấu hình của hàm này. Nhấn **Delete** để hoàn tất.

![](/images/5-Workshop/5.6/34.png)

*(Lặp lại thao tác trên cho đến khi xóa sạch cả 5 hàm Lambda của dự án).*

#### Bước 2 — Dọn dẹp nhật ký CloudWatch Logs

Khi bạn xóa hàm Lambda, các file nhật ký (Log) của nó vẫn tiếp tục nằm lại trên hệ thống và tính phí lưu trữ. Do đó, việc dọn dẹp CloudWatch là bắt buộc.

1. Truy cập dịch vụ **CloudWatch** trên AWS Console.
2. Ở thanh menu bên trái, tìm mục **Logs** → Chọn **Log Management** (hoặc Log groups).

![](/images/5-Workshop/5.6/35.png)

3. Tại ô tìm kiếm, gõ tên hàm Lambda (ví dụ: `/aws/lambda/BedrockInvokeLambda`) để lọc ra nhóm log tương ứng. Nhấn vào tên của Log group đó.

![](/images/5-Workshop/5.6/36.png)

4. Trong giao diện chi tiết, chuyển xuống tab **Log streams**. Tích chọn tất cả các luồng nhật ký (Log streams) đang có mặt.
5. Nhấn nút **Delete** ở thanh công cụ phía trên danh sách.

![](/images/5-Workshop/5.6/37.png)

6. Xác nhận xóa toàn bộ các luồng log này bằng cách nhấn **Delete** ở bảng thông báo hiện ra. 

![](/images/5-Workshop/5.6/38.png)

> 💡 **Mẹo:** Thay vì xóa từng Log stream, bạn cũng có thể chọn trực tiếp các **Log groups** ở màn hình ngoài (hình 36), chọn **Actions** → **Delete log group(s)** để dọn dẹp nhanh và triệt để hơn. Hãy làm tương tự cho tất cả 5 nhóm log của 5 hàm Lambda.

#### Danh sách kiểm tra nghiệm thu (Checklist)

- [ ] Đã xóa sạch 5 hàm Lambda.
- [ ] Đã xóa/làm sạch toàn bộ Log groups trong CloudWatch.
- [ ] (Tùy chọn) Truy cập Step Functions và xóa State Machine `ZeroBug-Workflow` nếu bạn là người khởi tạo.

→ Tiếp theo: [Toàn — Dọn dẹp EC2 & RDS](5.10.3-toan/)