---
title: "IAM Users/Groups"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

#### Mục tiêu

Tạo IAM Group và User (`tri`, `kiet`, `toan`, `hoa`, `trinh`) cho cả nhóm; cấp quyền cơ bản + PassRole.

#### Bước 1 — Vào IAM

1. Thanh search Console → gõ `IAM` → mở **IAM**.

![](/images/5-Workshop/5.3/1.jpg)

#### Bước 2 — Tạo Group

1. Menu trái **User groups** → **Create group**.
2. Group name: `zerobug-team`.
3. **Attach permissions policies** → tích **`PowerUserAccess`**.
4. → **Create group**.

![](/images/5-Workshop/5.3/2.jpg)
![](/images/5-Workshop/5.3/3.jpg)

#### Bước 3 — Inline policy PassRole

`PowerUserAccess` **chặn** `iam:ListInstanceProfiles` và `iam:PassRole` — Toàn/Hoa cần khi gắn Role vào EC2/Lambda.

1. Mở group `zerobug-team` → **Permissions** → **Add permissions** → **Create inline policy**.
2. Tab **JSON** → dán (thay `<ACCOUNT_ID>`):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ListForEc2RoleAttach",
      "Effect": "Allow",
      "Action": [
        "iam:ListInstanceProfiles",
        "iam:ListInstanceProfilesForRole",
        "iam:GetInstanceProfile",
        "iam:ListRoles"
      ],
      "Resource": "*"
    },
    {
      "Sid": "PassZeroBugRoles",
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "arn:aws:iam::<ACCOUNT_ID>:role/zerobug-*"
    }
  ]
}
```

3. **Next** → tên `zerobug-ec2-passrole` → **Create policy**.

4. **Đăng xuất Console và đăng nhập lại** bằng IAM User (`tri`…) để policy có hiệu lực trước khi Toàn/Hoa gắn Role.

Policy này cần cho **Toàn** (gắn `zerobug-ec2-role` vào EC2) và **Hoa** (gắn `zerobug-lambda-role` vào Lambda). Khi dropdown Role trống, chọn **Specify a custom value** → gõ `zerobug-ec2-role`.

#### Xử lý lỗi thường gặp (PassRole / ListInstanceProfiles)

| Lỗi | Cách sửa |
| --- | --- |
| `not authorized to perform iam:ListInstanceProfiles` | Thêm policy JSON ở Bước 3; **đăng xuất/đăng nhập lại** IAM User |
| Dropdown instance profile trống | Chọn **Specify a custom value** → `zerobug-ec2-role` |
| Đã chọn role nhưng chưa lưu | EC2 → **Modify IAM role** → **Update IAM role** (bấm nút cam) |
| Launch fail `iam:PassRole` | Kiểm tra policy `zerobug-ec2-passrole` đã gắn group; hoặc launch tạm bằng Root |

#### Bước 4 — Tạo 5 IAM User

Lặp lại cho `tri`, `kiet`, `toan`, `hoa`, `trinh` (tương ứng Trí, Kiệt, Toàn, Hoa, Trinh):

1. **Users** → **Create user**.
2. User name: `tri` (v.v. — `kiet`, `toan`, `hoa`, `trinh`).
3. Tích **Provide user access to the AWS Management Console** → **I want to create an IAM user** → đặt password.
4. **Add user to group** → tích `zerobug-team`.
5. → **Create user**.

![](/images/5-Workshop/5.3/4.jpg)
![](/images/5-Workshop/5.3/5.jpg)
![](/images/5-Workshop/5.3/6.jpg)
![](/images/5-Workshop/5.3/7.jpg)

#### Bước 5 — Lưu Console sign-in URL

Copy URL dạng `https://<account-id>.signin.aws.amazon.com/console` — gửi cho cả nhóm.

{{% notice warning %}}
**Nếu đã làm mọi thứ bằng Root:** tài nguyên vẫn hợp lệ. Bật **MFA cho Root**, tạo IAM User, **không dùng Root hàng ngày**. Không tạo Access Key cho Root.
{{% /notice %}}

<!-- Hình: /images/5-Workshop/5.3-Trí/iam-group.png -->
