---
title: "Khởi tạo AWS Lambda"
date: 2026-07-03
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---

#### Tạo các hàm Lambda

1. Truy cập dịch vụ `Lambda` trên AWS Console → Nhấn **Create function**.
2. Chọn **Author from scratch**.
3. **Function name:** `ProjectImportLambda` (Tạo hàm đầu tiên).
4. **Runtime:** Chọn `Java 17`.
5. Nhấn **Create function**.

![](/images/5-Workshop/5.6/1.png)
![](/images/5-Workshop/5.6/2.png)

Lặp lại các bước trên để tạo tổng cộng 5 hàm Lambda cho toàn bộ hệ thống bao gồm:
* `FileTreeLambda`
* `BedrockInvokeLambda`
* `ResultAndHistoryLambda`
* `ProjectImportLambda`
* `RagContextLambda`

![](/images/5-Workshop/5.6/3.png)

#### Cấu hình ProjectImportLambda

1. Nhấn vào hàm `ProjectImportLambda` vừa tạo.
2. Chuyển sang tab **Configuration**.
3. Chọn mục **Environment variables** ở menu bên trái → Nhấn **Edit** để thêm các biến môi trường kết nối cơ sở dữ liệu và S3:
   * `DB_PASS`
   * `DB_URL`
   * `DB_USER`
   * `SOURCE_CODE_BUCKET`

![](/images/5-Workshop/5.6/4.png)
![](/images/5-Workshop/5.6/5.png)

4. Tiếp tục ở tab **Configuration**, chọn mục **VPC** ở menu bên trái → Nhấn **Edit**.
5. Chọn **VPC** của dự án.
6. Chọn các **Subnets** (Nên đưa vào Private Subnets để bảo mật kết nối với RDS).
7. Chọn **Security groups** mặc định của VPC.
8. Nhấn **Save**.

![](/images/5-Workshop/5.6/6.png)
![](/images/5-Workshop/5.6/7.png)

#### Cấu hình RagContextLambda

Tương tự như `ProjectImportLambda`, hàm `RagContextLambda` cũng cần cấu hình môi trường và mạng để có thể băm mã nguồn và lưu vector vào RDS.

1. Quay lại danh sách Functions, chọn hàm `RagContextLambda`.
2. Chuyển sang tab **Configuration** → **Environment variables** → Nhấn **Edit** để thêm các biến:
   * `DB_PASS`
   * `DB_URL`
   * `DB_USER`
   * `SOURCE_CODE_BUCKET`
   * `GEMINI_API_KEY` *(Lưu ý: Biến này dùng cho cấu hình cũ trong ảnh, nếu đã chuyển sang Bedrock Mantle thì tùy chỉnh lại cho phù hợp).*

![](/images/5-Workshop/5.6/8.png)

3. Chuyển sang mục **VPC** → Nhấn **Edit**.
4. Thiết lập **VPC**, **Subnets**, và **Security groups** tương tự như hàm import.
5. Nhấn **Save**.

![](/images/5-Workshop/5.6/9.png)
![](/images/5-Workshop/5.6/10.png)
#### Cấu hình ResultAndHistoryLambda

Hàm này chịu trách nhiệm lưu kết quả và lịch sử tạo Unit Test vào cơ sở dữ liệu, do đó cần cấu hình kết nối tương tự như các hàm trước.

1. Tại danh sách Functions, chọn hàm `ResultAndHistoryLambda`.
2. Chuyển sang tab **Configuration**.
3. Chọn mục **Environment variables** ở menu bên trái → Nhấn **Edit** để thêm các biến môi trường kết nối RDS:
   * `DB_PASS`
   * `DB_URL`
   * `DB_USER`

![](/images/5-Workshop/5.6/11.png)

4. Tiếp tục ở tab **Configuration**, chọn mục **VPC** ở menu bên trái → Nhấn **Edit**.
5. Chọn **VPC**, **Subnets** (Private Subnets) và **Security groups** tương tự như các bước trước để đảm bảo hàm có thể giao tiếp nội bộ với cơ sở dữ liệu.
6. Nhấn **Save** để lưu cấu hình.

![](/images/5-Workshop/5.6/12.png)
![](/images/5-Workshop/5.6/13.png)

#### Cấu hình BedrockInvokeLambda

Hàm này đóng vai trò giao tiếp trực tiếp với AI (Bedrock Mantle) để sinh mã nguồn, nên chỉ cần cung cấp khóa API (không cần gắn VPC nếu không có nhu cầu kết nối RDS nội bộ).

1. Mở hàm `BedrockInvokeLambda`.
2. Chuyển sang tab **Configuration** → **Environment variables** → Nhấn **Edit**.
3. Thêm biến môi trường chứa khóa API để xác thực (trong ảnh minh họa là `OpenAi`, bạn có thể đổi thành tên biến tương ứng nếu dùng hệ thống khác).

![](/images/5-Workshop/5.6/14.png)

#### Cấu hình FileTreeLambda

Hàm này có nhiệm vụ đọc và phân tích cấu trúc cây thư mục (File Tree) của dự án đã được upload lên S3.

1. Mở hàm `FileTreeLambda`.
2. Chuyển sang tab **Configuration** → **Environment variables** → Nhấn **Edit**.
3. Thêm biến môi trường trỏ tới kho lưu trữ mã nguồn:
   * `S3_BUCKET_NAME` (Ví dụ: `zerobug-source-code-hutech-2026`)

![](/images/5-Workshop/5.6/15.png)