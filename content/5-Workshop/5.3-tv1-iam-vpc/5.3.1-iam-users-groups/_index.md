---
title: "IAM Users/Groups"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

#### Objective

Create IAM Group and Users (`tri`, `kiet`, `toan`, `hoa`, `trinh`) for the whole team; grant basic permissions + PassRole.

#### Step 1 — Open IAM

1. Console search bar → type `IAM` → open **IAM**.

![](/images/5-Workshop/5.3/1.jpg)

#### Step 2 — Create Group

1. Left menu **User groups** → **Create group**.
2. Group name: `zerobug-team`.
3. **Attach permissions policies** → select **`PowerUserAccess`**.
4. → **Create group**.

![](/images/5-Workshop/5.3/2.jpg)
![](/images/5-Workshop/5.3/3.jpg)

#### Step 3 — Inline PassRole policy

`PowerUserAccess` **blocks** `iam:ListInstanceProfiles` and `iam:PassRole` — Toàn/Hoa need these when attaching Roles to EC2/Lambda.

1. Open group `zerobug-team` → **Permissions** → **Add permissions** → **Create inline policy**.
2. **JSON** tab → paste (replace `<ACCOUNT_ID>`):

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

3. **Next** → name `zerobug-ec2-passrole` → **Create policy**.

4. **Sign out of the Console and sign in again** with the IAM User (`tri`…) so the policy takes effect before Toàn/Hoa attach Roles.

This policy is required for **Toàn** (attach `zerobug-ec2-role` to EC2) and **Hoa** (attach `zerobug-lambda-role` to Lambda). When the Role dropdown is empty, choose **Specify a custom value** → enter `zerobug-ec2-role`.

#### Common errors (PassRole / ListInstanceProfiles)

| Error | Fix |
| --- | --- |
| `not authorized to perform iam:ListInstanceProfiles` | Add the JSON policy in Step 3; **sign out/sign in again** as IAM User |
| Instance profile dropdown empty | Choose **Specify a custom value** → `zerobug-ec2-role` |
| Role selected but not saved | EC2 → **Modify IAM role** → **Update IAM role** (click the orange button) |
| Launch fail `iam:PassRole` | Verify policy `zerobug-ec2-passrole` is attached to the group; or launch temporarily as Root |

#### Step 4 — Create 5 IAM Users

Repeat for `tri`, `kiet`, `toan`, `hoa`, `trinh` (Trí, Kiệt, Toàn, Hoa, Trinh respectively):

1. **Users** → **Create user**.
2. User name: `tri` (etc. — `kiet`, `toan`, `hoa`, `trinh`).
3. Check **Provide user access to the AWS Management Console** → **I want to create an IAM user** → set password.
4. **Add user to group** → select `zerobug-team`.
5. → **Create user**.

![](/images/5-Workshop/5.3/4.jpg)
![](/images/5-Workshop/5.3/5.jpg)
![](/images/5-Workshop/5.3/6.jpg)
![](/images/5-Workshop/5.3/7.jpg)

#### Step 5 — Save Console sign-in URL

Copy the URL in the form `https://<account-id>.signin.aws.amazon.com/console` — share with the whole team.

{{% notice warning %}}
**If you already did everything as Root:** resources are still valid. Enable **MFA for Root**, create IAM Users, **do not use Root for daily work**. Do not create Access Keys for Root.
{{% /notice %}}

<!-- Image: /images/5-Workshop/5.3-Trí/iam-group.png -->
