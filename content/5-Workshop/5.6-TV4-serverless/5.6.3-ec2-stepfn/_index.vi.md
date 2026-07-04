---
title: "Cấu hình EC2 & Kích hoạt Workflow"
date: 2026-07-03
weight: 3
chapter: false
pre: " <b> 5.6.3. </b> "
---

Để ứng dụng Spring Boot chạy trên máy chủ EC2 có thể gửi lệnh kích hoạt (Start Execution) tới quy trình Step Functions đã tạo ở bước trước, máy chủ này bắt buộc phải có một định danh an toàn (IAM Role) được cấp quyền hợp lệ.

#### 1. Khởi tạo IAM Role cho EC2
Đầu tiên, ta cần tạo một Role để cấp quyền cho máy chủ EC2 giao tiếp với các dịch vụ AWS.

1. Truy cập dịch vụ **IAM** trên AWS Console → Chọn **Roles** ở thực đơn bên trái → Nhấn **Create role**.
2. Tại mục **Trusted entity type**, chọn **AWS service**. Tại phần **Use case**, tìm và chọn **EC2** rồi nhấn **Next**.
3. Tại giao diện **Add permissions**, tìm và tích chọn chính sách `AmazonSSMManagedInstanceCore` (Quyền cơ bản giúp quản trị viên kết nối từ xa vào EC2 một cách bảo mật).
4. Tiến tới bước cuối cùng, đặt tên Role là `ZeroBug-App-Role` rồi nhấn **Create role** để hoàn tất.

![](/images/5-Workshop/5.6/18.png)
![](/images/5-Workshop/5.6/19.png)
![](/images/5-Workshop/5.6/20.png)
![](/images/5-Workshop/5.6/21.png)

#### 2. Gắn quyền kích hoạt Step Functions (Inline Policy)
Mặc định `ZeroBug-App-Role` chưa thể can thiệp vào quy trình tự động. Ta cần nhúng thêm một chính sách riêng biệt (Inline Policy) để mở quyền này:

1. Nhấp vào tên Role `ZeroBug-App-Role` vừa tạo trong danh sách.
2. Tại tab **Permissions**, mở danh mục **Add permissions** → Chọn **Create inline policy**.

21![](/images/5-Workshop/5.6/22.png)

3. Chuyển trình soạn thảo sang tab **JSON** và dán đoạn mã phân quyền sau (Hãy nhớ thay thế chuỗi ARN bằng ARN State Machine thực tế của dự án):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowBackendToStartStepFunctions",
            "Effect": "Allow",
            "Action": "states:StartExecution",
            "Resource": "arn:aws:states:ap-southeast-1:123456789012:stateMachine:ZeroBug-Workflow"
        }
    ]
}
```
Nhấn nút lưu chính sách để áp dụng quyền lực mới cho Role.

![](/images/5-Workshop/5.6/23.png)

> ⚠️ **HÀNH ĐỘNG BẮT BUỘC:** Sau khi thiết lập xong Role trên IAM, bạn phải quay lại danh sách **EC2 Instances** → Tích chọn máy chủ của dự án → Chọn **Actions** → **Security** → **Modify IAM role** → Chọn đúng `ZeroBug-App-Role` vừa tạo và nhấn **Update** để máy chủ chính thức nhận quyền.

#### 3. Viết mã nguồn kích hoạt trong Spring Boot
Khi máy chủ EC2 đã có đủ thẩm quyền, bước cuối cùng là sử dụng thư viện `AWS SDK` trong mã nguồn Java của Backend để gọi Step Functions hoạt động thông qua một Request duy nhất:

```java
import software.amazon.awssdk.services.sfn.SfnClient;
import software.amazon.awssdk.services.sfn.model.StartExecutionRequest;
import software.amazon.awssdk.services.sfn.model.StartExecutionResponse;

// Khởi tạo đối tượng kết nối SfnClient nội bộ của AWS
SfnClient sfnClient = SfnClient.builder().region(Region.AP_SOUTHEAST_1).build();

// Đóng gói tham số đầu vào dưới dạng chuỗi JSON chuẩn
String jsonPayload = "{ \"projectId\": \"1783058\", \"sourceUrl\": \"https://github/...\" }";

// Thiết lập yêu cầu kích hoạt State Machine
StartExecutionRequest executionRequest = StartExecutionRequest.builder()
        .stateMachineArn("arn:aws:states:ap-southeast-1:123456789012:stateMachine:ZeroBug-Workflow")
        .input(jsonPayload)
        .build();

// Gửi tín hiệu thực thi toàn trình
StartExecutionResponse response = sfnClient.startExecution(executionRequest);
System.out.println("Workflow đã được kích hoạt thành công. Execution ARN: " + response.executionArn());
```