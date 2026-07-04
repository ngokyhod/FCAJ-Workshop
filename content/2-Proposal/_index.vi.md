---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# ZeroBug Agent
## Nền tảng AI sinh Unit Test tự động trên AWS

---

### 1. Tổng quan đề tài

**ZeroBug Agent** là hệ thống AI trên nền tảng Amazon Web Services (AWS), hỗ trợ tự động sinh Unit Test từ mã nguồn dự án. Hệ thống hỗ trợ ba ngôn ngữ trọng tâm: **Java** (JUnit 5 + Mockito), **Python** (pytest) và **C# / .NET** (xUnit).

Người dùng import toàn bộ dự án (Git hoặc Zip), duyệt cây thư mục trên giao diện IDE, mô tả yêu cầu kiểm thử bằng ngôn ngữ tự nhiên. Backend thu thập ngữ cảnh liên quan qua pipeline **RAG gọn** (embedding + truy vấn vector trên RDS), sau đó gọi **Amazon Bedrock Mantle** (`openai.gpt-oss-120b`) để sinh mã test.

**Mục tiêu chính:**

- Giảm thời gian viết Unit Test cho developer / QA.
- Tăng chất lượng gợi ý test case nhờ AI với ngữ cảnh đa file và RAG.
- Trải nghiệm gần IDE: xem source, sinh test, copy kết quả, xem lịch sử.
- Triển khai production trên AWS với bảo mật, giám sát và kiểm soát chi phí.
- Desktop client (Electron) — người dùng chỉ cần WiFi và cài app.

**Phạm vi đồ án:**

- Sinh Unit Test (chưa tự động chạy test suite trên CI).
- Ngôn ngữ: Java, Python, .NET (C#).
- Import project qua Git public hoặc Upload Zip.
- Kiến trúc: Frontend (Vite SPA / Electron) + Backend (Spring Boot trên EC2 trong VPC) + AWS Step Functions + 6 Lambda + các dịch vụ AWS (CloudFront, WAF, API Gateway, ALB, Cognito, S3, RDS, pgvector, Bedrock Mantle, Secrets Manager, CloudWatch, SNS, Budgets, IAM).
- Triển khai nhóm **5 thành viên** trên **một tài khoản AWS chung**, phân khối TV1–TV5 theo Workshop.

**Đối tượng sử dụng:**

- Developer / sinh viên cần bộ test khởi đầu nhanh.
- Admin quản trị hệ thống và theo dõi trạng thái dịch vụ AWS.

---

### 2. Mô tả nghiệp vụ

#### 2.1. Luồng nghiệp vụ tổng quát

```
Đăng ký / Đăng nhập (Amazon Cognito)
  → Tạo dự án (chọn ngôn ngữ: Java / Python / .NET; Import Git hoặc Zip)
  → ProjectImportLambda → S3; ContextBuilderLambda chunk + embed → pgvector trên RDS
  → Mở IDE (Explorer + Monaco Editor)
  → Nhập yêu cầu kiểm thử
  → Generate Test (Step Functions → ContextBuilder retrieve → BedrockInvokeLambda chat Mantle)
  → Xem / Copy kết quả; lịch sử lưu trên RDS
```

**Luồng truy cập hạ tầng:**

```
[Electron Desktop Client / Web Browser (Vite SPA)]
      ↓
[Amazon CloudFront — *.cloudfront.net]
      ↓
[AWS WAF — Web ACL]
      ↓
[Amazon API Gateway — REST API, JWT Authorizer (Cognito)]
      ↓
[Application Load Balancer — Public Subnet]
      ↓
[Amazon EC2 — Spring Boot Application — Private Subnet]
      ↓ (AWS SDK invoke)
[AWS Step Functions Workflow]
      ↓
[Lambda: ProjectImport | SourceFileService | ContextBuilder | BedrockInvoke | Result | History]
      ↓
[Amazon S3 — Source Code]  [AWS Secrets Manager — DB Credentials]
      ↓
[Amazon Bedrock Mantle — us-east-1: chat + embedding (cross-region qua NAT)]
      ↓
[Amazon RDS PostgreSQL — Metadata / Results / History / code_embeddings (pgvector)]
```

> **Lưu ý:** Không dùng Route 53; URL production là domain mặc định CloudFront (`d....cloudfront.net`).

#### 2.2. Quản lý người dùng (Amazon Cognito)

- Đăng ký / đăng nhập qua Amazon Cognito User Pool.
- JWT token do Cognito cấp; API Gateway xác thực qua Cognito Authorizer.
- Quên mật khẩu / xác nhận email: luồng Cognito Forgot Password.
- Phân quyền: Cognito Group USER và ADMIN (ADMIN xem toàn bộ project).
- Mật khẩu không lưu plaintext trong application database.

#### 2.3. Quản lý dự án

- **Import Git:** clone repository public (JGit shallow clone), loại bỏ `.git`, validate có file source; **`ProjectImportLambda`** xử lý và upload S3.
- **Upload Zip:** giải nén, nhận thư mục gốc; **`ProjectImportLambda`** upload S3.
- Sau import: **`ContextBuilderLambda`** chunk source, gọi Mantle embedding, lưu vector vào bảng **`code_embeddings`** trên RDS.
- Metadata (tên, loại ngôn ngữ, sourceType, userId) lưu Amazon RDS PostgreSQL (JPA tự tạo bảng ứng dụng khi EC2 khởi động).
- `projectLanguage`: JAVA | PYTHON | DOTNET.
- Xóa dự án: xóa metadata RDS và object trên S3.

#### 2.4. Không gian làm việc IDE (Workspace)

- **Explorer:** cây thư mục qua **`SourceFileServiceLambda`**; lọc thư mục nhiễu (`.git`, `node_modules`, `target`, …).
- **Editor:** Monaco Editor (read-only), syntax highlight theo ngôn ngữ file.
- **AI Test Agent:** textarea yêu cầu + nút Generate Test.
- **Kết quả:** hiển thị mã test theo framework (JUnit / pytest / xUnit); copy.

#### 2.5. Sinh test bằng AI (Step Functions + Lambda + Bedrock Mantle)

1. Người dùng nhập yêu cầu kiểm thử.
2. Spring Boot trên EC2 gọi AWS Step Functions workflow qua AWS SDK.
3. **`ContextBuilderLambda`**: đọc source từ S3; chunk văn bản; gọi **`POST /v1/embeddings`** (model `cohere.embed-multilingual-v3`); lưu vector vào **`code_embeddings`**; retrieve top-k chunk liên quan theo `project_id`.
4. **`BedrockInvokeLambda`**: nhận `context` từ Step Functions; gọi **`POST /v1/chat/completions`** (model `openai.gpt-oss-120b`) với prompt template theo ngôn ngữ (JUnit 5 / pytest / xUnit).
5. **`ResultServiceLambda`**: lưu kết quả sinh test vào RDS.
6. **`HistoryServiceLambda`**: ghi `generation_records` trên RDS; trả kết quả cho client.

Xác thực Mantle: Bearer token **`BEDROCK_MANTLE_API_KEY`** trên biến môi trường Lambda (Hoa tạo API key từ Bedrock Console `us-east-1`). Lambda chạy VPC private subnet, gọi Mantle cross-region qua NAT Gateway.

> Chế độ dev local: `AWS_ENABLED=false` trả template mock — chỉ dùng khi phát triển trên máy dev.

#### 2.6. Giám sát và vận hành

- Trang chủ / IDE: panel trạng thái RDS, S3, Bedrock Mantle (`/api/aws/status`).
- Amazon CloudWatch: log tập trung từ EC2, Lambda, API Gateway, Step Functions.
- CloudWatch Alarms: lỗi Lambda, RDS CPU, ALB unhealthy host → SNS.
- Amazon SNS: gửi email khi alarm kích hoạt.
- AWS Budgets: cảnh báo chi phí vượt ngưỡng.

#### 2.7. Desktop client

- Electron thin client: load URL production qua CloudFront (HTTPS).
- Người dùng cuối: cài `ZeroBugAgent-Setup.exe`, bật WiFi, đăng nhập Cognito.
- Không cần cài Java, Docker hay AWS CLI trên máy client.

---

### 3. Cơ sở hạ tầng

#### 3.1. Sơ đồ kiến trúc

![ZeroBug Agent Architecture](/images/2-Proposal/architecture.png)

#### 3.2. Dịch vụ AWS (production)

| Dịch vụ | Region | Vai trò |
| --- | --- | --- |
| Amazon CloudFront | Global | CDN HTTPS; URL production `*.cloudfront.net` |
| AWS WAF | Global / Regional | Web ACL; rate limit, core rules |
| Amazon API Gateway | ap-southeast-1 | REST API public; JWT Authorizer |
| Amazon Cognito | ap-southeast-1 | User Pool; Sign-in / JWT Token |
| Application Load Balancer | ap-southeast-1 | ALB Public Subnet; forward tới EC2 Private Subnet |
| Amazon EC2 | ap-southeast-1 | Spring Boot JAR Private Subnet; invoke Step Functions |
| AWS Step Functions | ap-southeast-1 | Orchestration workflow sinh test |
| AWS Lambda (×6) | ap-southeast-1 | ProjectImport, SourceFileService, ContextBuilder, BedrockInvoke, Result, History |
| Amazon S3 | ap-southeast-1 | Lưu source code + prefix `deploy/` cho JAR |
| Amazon RDS (PostgreSQL 15) | ap-southeast-1 | Metadata, results, history; pgvector `code_embeddings` |
| Amazon Bedrock Mantle | us-east-1 | Chat `openai.gpt-oss-120b` + embedding `cohere.embed-multilingual-v3` |
| AWS Secrets Manager | ap-southeast-1 | RDS credentials (`zerobug/rds/credentials`) |
| NAT Gateway | ap-southeast-1 | Lambda private subnet ra internet → Mantle cross-region |
| Amazon CloudWatch | ap-southeast-1 | Log EC2, Lambda, Step Fn, API GW |
| Amazon SNS | ap-southeast-1 | Nhận CloudWatch Alarm |
| AWS Budgets | Global | Cảnh báo chi phí vượt ngưỡng |
| AWS IAM | — | Users nhóm; Role EC2, Lambda, Step Functions |

URL production: `https://d....cloudfront.net` → WAF → API Gateway → ALB → EC2 (Private Subnet).

#### 3.3. Phân công triển khai nhóm (Workshop)

| Thành viên | Khối | Nội dung chính |
| --- | --- | --- |
| Trí | TV1 | IAM, VPC, Subnet, NAT, Security Groups |
| Kiệt | TV2 | S3, RDS, Secrets Manager, pgvector, Bedrock model access |
| Toàn | TV3 | EC2 Spring Boot, ALB, Target Group |
| Hoa | TV4 | 6 Lambda, Step Functions, Bedrock Mantle API key |
| Trinh | TV5 | Cognito, API Gateway, CloudFront, WAF |

Thứ tự: **Trí → Kiệt → Toàn → Hoa → Trinh → kiểm thử E2E**.

#### 3.4. Thành phần ứng dụng

| Lớp | Công nghệ | Mô tả |
| --- | --- | --- |
| Frontend | Vite, JavaScript SPA | Giao diện web; Cognito SDK / JWT |
| Backend EC2 | Spring Boot 3, Java 17 | REST API; invoke Step Functions SDK |
| Orchestration | AWS Step Functions | Workflow Import → Context → Chat → Result → History |
| Backend λ | AWS Lambda (Java) | 6 functions serverless |
| Desktop | Electron 28 | Thin client, NSIS installer `.exe` |
| Import Git | Eclipse JGit | Clone repo public → upload S3 |
| Load balancer | ALB | Public Subnet; forward tới EC2 Private |
| Vector store | pgvector trên RDS | Bảng `code_embeddings`, dimension 1024 |

#### 3.5. Triển khai và vận hành

- Build: `build-all.bat` → Vite build FE + Maven package JAR + artifact Lambda.
- VPC: Public Subnet (ALB) + Private Subnet (EC2, RDS, Lambda).
- EC2: JAR + systemd Private Subnet; IAM Role invoke Step Functions.
- Lambda: VPC private subnet + NAT; env `BEDROCK_MANTLE_API_KEY`; đọc Secret DB runtime.
- RDS: PostgreSQL 15, Private Subnet; extension `vector`; bảng ứng dụng qua JPA (`ddl-auto=update`).
- CloudFront + WAF + API Gateway + Cognito: lớp edge và xác thực.
- CloudWatch + SNS + Budgets: giám sát và cảnh báo chi phí.

#### 3.6. Ước tính chi phí vận hành (tham khảo)

| Hạng mục | Cấu hình gợi ý | Chi phí/tháng (USD) |
| --- | --- | --- |
| EC2 | t3.micro + EBS 30 GB gp3 | ~10 |
| RDS PostgreSQL | db.t3/t4g.micro, Single-AZ, 20 GB | ~17–20 |
| Lambda | ~300.000 request/tháng | ~0 |
| API Gateway | ~300.000 request/tháng | ~0 |
| Cognito | ~20 user | ~0 |
| S3 | ~5 GB Standard | ~0,12 |
| Secrets Manager | 1 secret DB | ~0,40 |
| WAF | 1 Web ACL + rules | ~7 |
| ALB | + LCU traffic thấp | ~17 |
| **Tổng (ước tính)** | | **~54 USD/tháng** |

Nếu còn **AWS Free Tier** (EC2, RDS…), các hạng mục không free tier còn lại chủ yếu **WAF (~7 USD) + ALB (~17 USD) + Secrets (~0,8 USD) ≈ 25 USD/tháng**.

{{% notice warning %}}
**NAT Gateway** không thuộc Free Tier (~32 USD/tháng nếu bật liên tục). Workshop khuyến nghị xóa sau khi hoàn thành. Chi phí Bedrock Mantle inference (chat + embedding) cộng thêm theo mức sử dụng thực tế.
{{% /notice %}}

---

### 4. Ngôn ngữ, công cụ và mô hình sử dụng

#### 4.1. Ngôn ngữ lập trình và framework

| Thành phần | Công nghệ |
| --- | --- |
| Backend EC2 | Java 17, Spring Boot 3.2, Spring Data JPA |
| Backend Lambda | Java 17 |
| Frontend | JavaScript (ES modules), Vite 5 |
| Desktop | Electron, electron-builder |
| Database | PostgreSQL 15 (RDS + pgvector), H2 (dev) |
| Build | Maven, npm, AWS CLI |
| Import Git | Eclipse JGit |
| Auth | Amazon Cognito User Pool |
| API edge | Amazon API Gateway, AWS WAF, CloudFront |

#### 4.2. Mô hình AI (Bedrock Mantle)

| Model | Mục đích | Endpoint |
| --- | --- | --- |
| `openai.gpt-oss-120b` | Phân tích context + sinh Unit Test (chat) | `POST /v1/chat/completions` |
| `cohere.embed-multilingual-v3` | Embedding chunk source cho RAG (vector 1024) | `POST /v1/embeddings` |

- Mantle base URL: `https://bedrock-mantle.us-east-1.api.aws`
- Lambda region: `ap-southeast-1`; inference Mantle: `us-east-1` (cross-region).
- RAG top-k gợi ý: `5`.

#### 4.3. Pipeline RAG gọn (Context Builder)

| Bước | Thực hiện |
| --- | --- |
| 1 | Đọc source từ S3, chunk văn bản |
| 2 | Gọi Mantle embedding (`input_type: search_document`) |
| 3 | Lưu vector vào RDS bảng `code_embeddings` |
| 4 | Khi generate: embed câu hỏi (`search_query`), retrieve top-k theo `project_id` |
| 5 | Truyền `context` cho BedrockInvokeLambda sinh test |

| Ngôn ngữ | File lọc | Framework output |
| --- | --- | --- |
| JAVA | `*.java`; exclude test dirs | JUnit 5 + Mockito |
| PYTHON | `*.py`; exclude `tests/`, `__pycache__` | pytest |
| DOTNET | `*.cs`; exclude `*Test*`, `obj/`, `bin/` | xUnit |

#### 4.4. Bảo mật

| Hạng mục | Giải pháp |
| --- | --- |
| Xác thực user | Amazon Cognito + JWT; API Gateway Authorizer |
| Mật khẩu DB | AWS Secrets Manager; EC2/Lambda đọc lúc runtime |
| Mantle API key | Biến môi trường Lambda; không commit vào Git |
| AWS credentials | IAM Role (EC2, Lambda); không hardcode access key |
| Network | WAF; VPC Private Subnet; RDS không public |
| HTTPS | CloudFront + ACM |
| Phân quyền | Cognito Groups USER / ADMIN |

#### 4.5. API chính

| Method | Endpoint | Chức năng |
| --- | --- | --- |
| GET | `/api/health` | Kiểm tra kết nối (public) |
| — | Cognito Hosted UI / SDK | Đăng ký / đăng nhập |
| GET | `/api/auth/me` | User từ JWT claims |
| GET | `/api/projects` | Danh sách dự án |
| POST | `/api/projects/import/git` | Import Git → Step Fn → S3 |
| POST | `/api/projects/import/zip` | Upload Zip → Step Fn → S3 |
| GET | `/api/projects/{id}/files` | Cây thư mục (SourceFileService) |
| GET | `/api/projects/{id}/file` | Nội dung file |
| POST | `/api/projects/{id}/generate` | Generate → Step Fn → Mantle chat |
| GET | `/api/generations/recent` | Lịch sử sinh test |
| GET | `/api/aws/status` | Trạng thái RDS / S3 / Bedrock Mantle |
| DELETE | `/api/projects/{id}` | Xóa dự án |

---

### 5. Hướng đi và giải pháp vượt qua thách thức

#### 5.1. Bảng thách thức — giải pháp

| Thách thức | Giải pháp triển khai |
| --- | --- |
| Chi phí vận hành AWS | Ước tính ~54 USD/tháng; Budgets + SNS; dọn NAT/RDS sau workshop |
| Ngữ cảnh AI thiếu / sai file | RAG pgvector trên RDS; top-k retrieve; lọc file theo ngôn ngữ |
| Đa ngôn ngữ (Java/Python/.NET) | `projectLanguage` + prompt template riêng; Context Builder filter extension |
| Bảo mật API public | WAF + Cognito JWT + API Gateway Authorizer; Secrets Manager |
| Credential lộ trên server | Secrets Manager; IAM Role; không hardcode password/API key |
| Lambda gọi Mantle cross-region | NAT Gateway + env `BEDROCK_MANTLE_API_KEY`; model access `us-east-1` |
| Phối hợp nhóm 5 người | Bảng tham số chung; triển khai theo thứ tự TV1→TV5; checklist bàn giao |
| Giám sát lỗi production | CloudWatch Logs + Alarms → SNS |
| Người dùng cuối cài đặt | Electron thin client; không cần Java local |

#### 5.2. Hướng đi tổng thể

1. Production-ready trên AWS theo kiến trúc Workshop: CloudFront edge, VPC private, serverless AI pipeline.
2. EC2 (Spring Boot) điều phối API; Step Functions orchestrate; 6 Lambda xử lý import, RAG, chat, lưu kết quả.
3. RAG gọn trên RDS pgvector thay OpenSearch — phù hợp quy mô đồ án và chi phí.
4. Bedrock Mantle thay Claude Haiku — chat + embedding thống nhất qua Mantle API.
5. Không Route 53; dùng CloudFront domain mặc định cho demo/production workshop.

#### 5.3. Kết quả mong đợi

- Nền tảng web + desktop sinh Unit Test cho Java, Python và .NET.
- Triển khai AWS end-to-end theo Workshop: CloudFront, WAF, API Gateway, ALB, Cognito, EC2, Step Functions, Lambda, S3, RDS, pgvector, Bedrock Mantle, Secrets Manager, CloudWatch, SNS, Budgets.
- Tích hợp AI vào quy trình QA với RAG và kiểm soát chi phí (~54 USD/tháng ước tính).
- Tài liệu Proposal, Workshop và báo cáo khớp với hệ thống triển khai thực tế.
