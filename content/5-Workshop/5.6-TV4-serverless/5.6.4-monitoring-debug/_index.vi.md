---
title: "Giám sát & Gỡ lỗi (Monitoring & Debugging)"
date: 2026-07-03
weight: 4
chapter: false
pre: " <b> 5.6.4. </b> "
---

Trong kiến trúc Serverless, mã nguồn của chúng ta chạy trên các môi trường máy chủ tạm thời (Ephemeral Compute). Không có một máy chủ vật lý cố định để chúng ta mở Terminal lên xem lỗi. Do đó, **Amazon CloudWatch** chính là "mắt và tai" của toàn bộ hệ thống. Mọi luồng thực thi, từ thành công đến thất bại, đều được ghi nhận chi tiết tại đây.

Dưới đây là quy trình 4 bước tiêu chuẩn để truy vết và gỡ lỗi một hàm Lambda (lấy `ProjectImportLambda` làm ví dụ):

#### 1. Chọn hàm Lambda cần kiểm tra
Đầu tiên, bạn truy cập vào dịch vụ **Lambda** trên AWS Console. Tại giao diện **Functions**, bạn sẽ thấy danh sách toàn bộ các hàm trong dự án. Nhấp vào tên hàm mà bạn nghi ngờ đang gặp sự cố (ở đây ta chọn `ProjectImportLambda`).

![](/images/5-Workshop/5.6/24.png)

#### 2. Mở cổng giám sát CloudWatch
Bên trong giao diện chi tiết của hàm Lambda, bạn thực hiện các thao tác sau:
1. Chuyển sang tab **Monitor** (Giám sát).
2. Nhấp vào nút **View CloudWatch logs** ở góc phải màn hình. Thao tác này sẽ tự động chuyển hướng bạn đến kho lưu trữ log riêng biệt của hàm Lambda này trên dịch vụ CloudWatch.

![](/images/5-Workshop/5.6/25.png)

#### 3. Chọn luồng Log (Log Stream) mới nhất
Mỗi khi Lambda được kích hoạt, AWS sẽ tạo ra một **Log stream** (Luồng nhật ký) mới.
Tại giao diện CloudWatch, dưới mục **Log streams**, hãy tìm và nhấp vào luồng log trên cùng (đây là luồng ghi lại lần chạy gần nhất của hàm).

![](/images/5-Workshop/5.6/26.png)

#### 4. Đọc Log và "Bắt bệnh" (Troubleshooting)
Giao diện **Log events** sẽ hiển thị chi tiết từng mili-giây quá trình mã nguồn của bạn thực thi. Hãy chú ý đến các dòng có chứa từ khóa `LỖI`, `ERROR`, `Exception` hoặc `Task timed out`.

**Ví dụ thực tế trong ảnh dưới:** Hệ thống báo lỗi `Authentication is required but no CredentialsProvider has been registered`. 
* **Nguyên nhân:** Mã nguồn JGit đang cố gắng clone một Repository ở chế độ Private (Riêng tư) từ GitHub, nhưng chưa được cung cấp Username/Token.
* **Cách khắc phục:** Đổi Repository trên GitHub sang chế độ Public, hoặc bổ sung CredentialsProvider vào đoạn code JGit.

![](/images/5-Workshop/5.6/27.png)

Nhờ vào những dòng Log chi tiết này, việc tìm ra nguyên nhân và khắc phục lỗi trong kiến trúc Serverless trở nên minh bạch và dễ dàng hơn rất nhiều!