---
title: "Quy ước & thứ tự triển khai"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.2.4. </b> "
---

#### Checklist chung — trước khi bất kỳ thành viên nào làm việc

1. **Region:** Console → **Asia Pacific (Singapore) `ap-southeast-1`** *(IAM global; WAF CloudFront = Global)*.
2. **Tài khoản:** cả nhóm dùng **chung 1 AWS account**, đăng nhập bằng IAM User `tri`, `kiet`, `toan`, `hoa`, `trinh` — không dùng Root hàng ngày.
3. **Bảng tham số:** mở [5.2.3](5.2.3-parameter-table/) — điền ID/ARN/DNS ngay khi tạo xong, chia sẻ cả nhóm.
4. **PassRole:** group `zerobug-team` đã có policy `zerobug-ec2-passrole` ([5.3.1](../5.3-tv1-iam-vpc/5.3.1-iam-users-groups/)) — sau khi thêm policy, **đăng xuất/đăng nhập lại**.

#### Quy ước đặt tên

- Prefix tài nguyên hạ tầng: **`zerobug-*`** (VPC, subnet, SG, bucket, RDS…)
- **Lambda function:** tên cố định PascalCase — `ProjectImportLambda`, `SourceFileServiceLambda`, `ContextBuilderLambda`, `BedrockInvokeLambda`, `ResultServiceLambda`, `HistoryServiceLambda` *(ghi vào [bảng tham số](5.2.3-parameter-table/))*
- Tag gợi ý: `Project=zerobug`, `Owner=Tri` … `trinh`

#### Thứ tự triển khai

```
Trí (IAM + VPC) → Kiệt (S3 + RDS + Secrets + pgvector + Bedrock model access)
  → Toàn (EC2 + ALB) → Hoa (Lambda + Step Functions)
    → Trinh (Cognito + API GW + CloudFront + WAF)
      → 5.8 Kiểm thử end-to-end
```

| Thành viên | Phụ thuộc tối thiểu |
| --- | --- |
| Trí | Hoàn thành [5.2.1](5.2.1-aws-account/) |
| Kiệt | Trí xong (Private Subnet, `zerobug-rds-sg`, Role ARN) |
| Toàn | Trí + Kiệt (Subnet/SG/Role; S3, RDS, Secret DB) |
| Hoa | Trí + Kiệt (NAT, Lambda-SG, Roles; S3, RDS, Secret DB, pgvector, Mantle params) |
| Trinh | **ALB DNS** từ Toàn *(có thể làm song song Hoa)* |

#### Chiến lược tạm dừng (viết báo cáo)

- Có thể **dựng hết hạ tầng Trí → Kiệt → Toàn → Hoa → Trinh** trước khi backend/Lambda chạy.
- **Tạm dừng Toàn** sau bước cài Java/SSM — làm **Trinh** trước; test end-to-end để mục **5.8**.
- CloudFront `/api/health` trả **502** hoặc ALB Target **Unhealthy** khi app chưa chạy — **bình thường**.

#### Build artifact (Toàn & Hoa)

Trên máy dev chạy `build-all.bat` (chi tiết [5.2.2](5.2.2-tools-and-build/)):

- JAR → Toàn upload `s3://<bucket>/deploy/zerobug-agent-app-1.0.0.jar`
- Lambda packages → Hoa deploy từng function
