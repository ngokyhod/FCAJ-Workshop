---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# ZeroBug Agent
## AI-Powered Automated Unit Test Generation Platform on AWS

---

### 1. Project Overview

**ZeroBug Agent** is an AI system built on Amazon Web Services (AWS) that automatically generates Unit Tests from project source code. The system supports three core languages: **Java** (JUnit 5 + Mockito), **Python** (pytest), and **C# / .NET** (xUnit).

Users import an entire project (via public Git or Zip upload), browse the file tree in an integrated IDE interface, and describe test requirements in natural language. The backend collects relevant context through a **lightweight RAG pipeline** (embedding + vector retrieval on RDS), then calls **Amazon Bedrock Mantle** (`openai.gpt-oss-120b`) to generate test code.

**Main objectives:**

- Reduce Unit Test writing time for developers and QA engineers.
- Improve test suggestions with AI using multi-file context and RAG.
- Provide an IDE-like experience: view source, generate tests, copy results, view history.
- Deploy to production on AWS with security, monitoring, and cost control.
- Offer a desktop client (Electron) — users only need WiFi and the app.

**Project scope:**

- Generate Unit Tests (does not automatically run test suites on CI yet).
- Languages: Java, Python, .NET (C#).
- Import projects via public Git or Zip upload.
- Architecture: Frontend (Vite SPA / Electron) + Backend (Spring Boot on EC2 in VPC) + AWS Step Functions + 6 Lambda functions + AWS services (CloudFront, WAF, API Gateway, ALB, Cognito, S3, RDS, pgvector, Bedrock Mantle, Secrets Manager, CloudWatch, SNS, Budgets, IAM).
- **5-member team** deployment on **one shared AWS account**, split into TV1–TV5 blocks per Workshop.

**Target users:**

- Developers / students who need a quick test baseline.
- Admins managing the system and monitoring AWS service status.

---

### 2. Business Description

#### 2.1. Overall Business Flow

```
Sign up / Sign in (Amazon Cognito)
  → Create project (choose language: Java / Python / .NET; Import Git or Zip)
  → ProjectImportLambda → S3; ContextBuilderLambda chunk + embed → pgvector on RDS
  → Open IDE (Explorer + Monaco Editor)
  → Enter test requirements
  → Generate Test (Step Functions → ContextBuilder retrieve → BedrockInvokeLambda Mantle chat)
  → View / Copy results; history stored on RDS
```

**Infrastructure access flow:**

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
[Amazon Bedrock Mantle — us-east-1: chat + embedding (cross-region via NAT)]
      ↓
[Amazon RDS PostgreSQL — Metadata / Results / History / code_embeddings (pgvector)]
```

> **Note:** Route 53 is not used; the production URL is the default CloudFront domain (`d....cloudfront.net`).

#### 2.2. User Management (Amazon Cognito)

- Sign up / sign in via Amazon Cognito User Pool.
- JWT tokens issued by Cognito; API Gateway validates via Cognito Authorizer.
- Forgot password / email confirmation: Cognito Forgot Password flow.
- Authorization: Cognito Groups USER and ADMIN (ADMIN can view all projects).
- Passwords are not stored in plaintext in the application database.

#### 2.3. Project Management

- **Git Import:** clone public repository (JGit shallow clone), remove `.git`, validate source files; **`ProjectImportLambda`** processes and uploads to S3.
- **Zip Upload:** extract archive, detect root folder; **`ProjectImportLambda`** uploads to S3.
- After import: **`ContextBuilderLambda`** chunks source, calls Mantle embedding, stores vectors in **`code_embeddings`** on RDS.
- Metadata (name, language type, sourceType, userId) stored in Amazon RDS PostgreSQL (JPA creates application tables when EC2 starts).
- `projectLanguage`: JAVA | PYTHON | DOTNET.
- Delete project: remove RDS metadata and S3 objects.

#### 2.4. IDE Workspace

- **Explorer:** file tree via **`SourceFileServiceLambda`**; filters noisy directories (`.git`, `node_modules`, `target`, …).
- **Editor:** Monaco Editor (read-only), syntax highlighting by file language.
- **AI Test Agent:** requirement textarea + Generate Test button.
- **Results:** display test code by framework (JUnit / pytest / xUnit); copy to clipboard.

#### 2.5. AI Test Generation (Step Functions + Lambda + Bedrock Mantle)

1. User enters a test requirement.
2. Spring Boot on EC2 invokes AWS Step Functions workflow via AWS SDK.
3. **`ContextBuilderLambda`**: reads source from S3; chunks text; calls **`POST /v1/embeddings`** (model `cohere.embed-multilingual-v3`); stores vectors in **`code_embeddings`**; retrieves top-k relevant chunks by `project_id`.
4. **`BedrockInvokeLambda`**: receives `context` from Step Functions; calls **`POST /v1/chat/completions`** (model `openai.gpt-oss-120b`) with language-specific prompt templates (JUnit 5 / pytest / xUnit).
5. **`ResultServiceLambda`**: saves generated test results to RDS.
6. **`HistoryServiceLambda`**: writes `generation_records` to RDS; returns results to the client.

Mantle authentication: Bearer token **`BEDROCK_MANTLE_API_KEY`** in Lambda environment variables (Hoa creates API key from Bedrock Console `us-east-1`). Lambda runs in VPC private subnet and calls Mantle cross-region via NAT Gateway.

> Local dev mode: `AWS_ENABLED=false` returns mock templates — for development only.

#### 2.6. Monitoring and Operations

- Home / IDE: RDS, S3, Bedrock Mantle status panel (`/api/aws/status`).
- Amazon CloudWatch: centralized logs from EC2, Lambda, API Gateway, Step Functions.
- CloudWatch Alarms: Lambda errors, RDS CPU, ALB unhealthy hosts → SNS.
- Amazon SNS: email when alarms trigger.
- AWS Budgets: cost threshold alerts.

#### 2.7. Desktop Client

- Electron thin client: loads production URL via CloudFront (HTTPS).
- End users: install `ZeroBugAgent-Setup.exe`, connect WiFi, sign in via Cognito.
- No Java, Docker, or AWS CLI required on the client machine.

---

### 3. Infrastructure

#### 3.1. Architecture Diagram

![ZeroBug Agent Architecture](/images/2-Proposal/architecture.png)

#### 3.2. AWS Services (Production)

| Service | Region | Role |
| --- | --- | --- |
| Amazon CloudFront | Global | HTTPS CDN; production URL `*.cloudfront.net` |
| AWS WAF | Global / Regional | Web ACL; rate limit, core rules |
| Amazon API Gateway | ap-southeast-1 | Public REST API; JWT Authorizer |
| Amazon Cognito | ap-southeast-1 | User Pool; Sign-in / JWT Token |
| Application Load Balancer | ap-southeast-1 | ALB Public Subnet; forwards to EC2 Private Subnet |
| Amazon EC2 | ap-southeast-1 | Spring Boot JAR Private Subnet; invokes Step Functions |
| AWS Step Functions | ap-southeast-1 | Test generation workflow orchestration |
| AWS Lambda (×6) | ap-southeast-1 | ProjectImport, SourceFileService, ContextBuilder, BedrockInvoke, Result, History |
| Amazon S3 | ap-southeast-1 | Source code + `deploy/` JAR prefix |
| Amazon RDS (PostgreSQL 15) | ap-southeast-1 | Metadata, results, history; pgvector `code_embeddings` |
| Amazon Bedrock Mantle | us-east-1 | Chat `openai.gpt-oss-120b` + embedding `cohere.embed-multilingual-v3` |
| AWS Secrets Manager | ap-southeast-1 | RDS credentials (`zerobug/rds/credentials`) |
| NAT Gateway | ap-southeast-1 | Lambda private subnet to internet → cross-region Mantle |
| Amazon CloudWatch | ap-southeast-1 | Logs for EC2, Lambda, Step Fn, API GW |
| Amazon SNS | ap-southeast-1 | Receives CloudWatch Alarms |
| AWS Budgets | Global | Cost threshold alerts |
| AWS IAM | — | Team users; Roles for EC2, Lambda, Step Functions |

Production URL: `https://d....cloudfront.net` → WAF → API Gateway → ALB → EC2 (Private Subnet).

#### 3.3. Team Deployment Assignments (Workshop)

| Member | Block | Main scope |
| --- | --- | --- |
| Tri | TV1 | IAM, VPC, Subnet, NAT, Security Groups |
| Kiet | TV2 | S3, RDS, Secrets Manager, pgvector, Bedrock model access |
| Toan | TV3 | EC2 Spring Boot, ALB, Target Group |
| Hoa | TV4 | 6 Lambda functions, Step Functions, Bedrock Mantle API key |
| Trinh | TV5 | Cognito, API Gateway, CloudFront, WAF |

Order: **Tri → Kiet → Toan → Hoa → Trinh → E2E testing**.

#### 3.4. Application Components

| Layer | Technology | Description |
| --- | --- | --- |
| Frontend | Vite, JavaScript SPA | Web UI; Cognito SDK / JWT |
| Backend EC2 | Spring Boot 3, Java 17 | REST API; invoke Step Functions SDK |
| Orchestration | AWS Step Functions | Import → Context → Chat → Result → History workflow |
| Backend λ | AWS Lambda (Java) | 6 serverless functions |
| Desktop | Electron 28 | Thin client, NSIS `.exe` installer |
| Git Import | Eclipse JGit | Clone public repo → upload S3 |
| Load balancer | ALB | Public Subnet; forwards to EC2 Private |
| Vector store | pgvector on RDS | `code_embeddings` table, dimension 1024 |

#### 3.5. Deployment and Operations

- Build: `build-all.bat` → Vite build FE + Maven package JAR + Lambda artifacts.
- VPC: Public Subnet (ALB) + Private Subnet (EC2, RDS, Lambda).
- EC2: JAR + systemd in Private Subnet; IAM Role to invoke Step Functions.
- Lambda: VPC private subnet + NAT; env `BEDROCK_MANTLE_API_KEY`; read DB Secret at runtime.
- RDS: PostgreSQL 15, Private Subnet; `vector` extension; application tables via JPA (`ddl-auto=update`).
- CloudFront + WAF + API Gateway + Cognito: edge and authentication layer.
- CloudWatch + SNS + Budgets: monitoring and cost alerts.

#### 3.6. Operating Cost Estimate (Reference)

| Item | Suggested config | Cost/month (USD) |
| --- | --- | --- |
| EC2 | t3.micro + EBS 30 GB gp3 | ~10 |
| RDS PostgreSQL | db.t3/t4g.micro, Single-AZ, 20 GB | ~17–20 |
| Lambda | ~300,000 requests/month | ~0 |
| API Gateway | ~300,000 requests/month | ~0 |
| Cognito | ~20 users | ~0 |
| S3 | ~5 GB Standard | ~0.12 |
| Secrets Manager | 1 DB secret | ~0.40 |
| WAF | 1 Web ACL + rules | ~7 |
| ALB | + low LCU traffic | ~17 |
| **Total (estimate)** | | **~54 USD/month** |

With **AWS Free Tier** (EC2, RDS, etc.), remaining non–free-tier items are mainly **WAF (~7 USD) + ALB (~17 USD) + Secrets (~0.8 USD) ≈ 25 USD/month**.

{{% notice warning %}}
**NAT Gateway** is not Free Tier (~32 USD/month if left running). The Workshop recommends deleting it after completion. Bedrock Mantle inference (chat + embedding) adds cost based on actual usage.
{{% /notice %}}

---

### 4. Languages, Tools, and Models

#### 4.1. Programming Languages and Frameworks

| Component | Technology |
| --- | --- |
| Backend EC2 | Java 17, Spring Boot 3.2, Spring Data JPA |
| Backend Lambda | Java 17 |
| Frontend | JavaScript (ES modules), Vite 5 |
| Desktop | Electron, electron-builder |
| Database | PostgreSQL 15 (RDS + pgvector), H2 (dev) |
| Build | Maven, npm, AWS CLI |
| Git Import | Eclipse JGit |
| Auth | Amazon Cognito User Pool |
| API edge | Amazon API Gateway, AWS WAF, CloudFront |

#### 4.2. AI Models (Bedrock Mantle)

| Model | Purpose | Endpoint |
| --- | --- | --- |
| `openai.gpt-oss-120b` | Analyze context + generate Unit Tests (chat) | `POST /v1/chat/completions` |
| `cohere.embed-multilingual-v3` | Embed source chunks for RAG (1024-dim vectors) | `POST /v1/embeddings` |

- Mantle base URL: `https://bedrock-mantle.us-east-1.api.aws`
- Lambda region: `ap-southeast-1`; Mantle inference: `us-east-1` (cross-region).
- Suggested RAG top-k: `5`.

#### 4.3. Lightweight RAG Pipeline (Context Builder)

| Step | Action |
| --- | --- |
| 1 | Read source from S3, chunk text |
| 2 | Call Mantle embedding (`input_type: search_document`) |
| 3 | Store vectors in RDS `code_embeddings` table |
| 4 | On generate: embed query (`search_query`), retrieve top-k by `project_id` |
| 5 | Pass `context` to BedrockInvokeLambda for test generation |

| Language | File filter | Output framework |
| --- | --- | --- |
| JAVA | `*.java`; exclude test dirs | JUnit 5 + Mockito |
| PYTHON | `*.py`; exclude `tests/`, `__pycache__` | pytest |
| DOTNET | `*.cs`; exclude `*Test*`, `obj/`, `bin/` | xUnit |

#### 4.4. Security

| Area | Solution |
| --- | --- |
| User authentication | Amazon Cognito + JWT; API Gateway Authorizer |
| DB password | AWS Secrets Manager; EC2/Lambda read at runtime |
| Mantle API key | Lambda environment variable; not committed to Git |
| AWS credentials | IAM Roles (EC2, Lambda); no hardcoded access keys |
| Network | WAF; VPC Private Subnet; RDS not public |
| HTTPS | CloudFront + ACM |
| Authorization | Cognito Groups USER / ADMIN |

#### 4.5. Main API Endpoints

| Method | Endpoint | Function |
| --- | --- | --- |
| GET | `/api/health` | Health check (public) |
| — | Cognito Hosted UI / SDK | Sign up / sign in |
| GET | `/api/auth/me` | User from JWT claims |
| GET | `/api/projects` | List projects |
| POST | `/api/projects/import/git` | Import Git → Step Fn → S3 |
| POST | `/api/projects/import/zip` | Upload Zip → Step Fn → S3 |
| GET | `/api/projects/{id}/files` | Directory tree (SourceFileService) |
| GET | `/api/projects/{id}/file` | File content |
| POST | `/api/projects/{id}/generate` | Generate → Step Fn → Mantle chat |
| GET | `/api/generations/recent` | Generation history |
| GET | `/api/aws/status` | RDS / S3 / Bedrock Mantle status |
| DELETE | `/api/projects/{id}` | Delete project |

---

### 5. Direction and Challenge Mitigation

#### 5.1. Challenges and Solutions

| Challenge | Mitigation |
| --- | --- |
| AWS operating costs | Estimate ~54 USD/month; Budgets + SNS; clean up NAT/RDS after workshop |
| Missing / wrong AI context | RAG pgvector on RDS; top-k retrieval; filter files by language |
| Multi-language (Java/Python/.NET) | `projectLanguage` + separate prompt templates; Context Builder filter extensions |
| Public API security | WAF + Cognito JWT + API Gateway Authorizer; Secrets Manager |
| Credential exposure on server | Secrets Manager; IAM Roles; no hardcoded passwords/API keys |
| Lambda calling Mantle cross-region | NAT Gateway + env `BEDROCK_MANTLE_API_KEY`; model access in `us-east-1` |
| 5-member team coordination | Shared parameter table; deploy TV1→TV5 in order; handoff checklists |
| Undetected production errors | CloudWatch Logs + Alarms → SNS email |
| Difficult end-user installation | Electron thin client; no local Java required |

#### 5.2. Overall Direction

1. Production-ready on AWS per Workshop architecture: CloudFront edge, private VPC, serverless AI pipeline.
2. EC2 (Spring Boot) orchestrates APIs; Step Functions orchestrates workflow; 6 Lambda functions handle import, RAG, chat, and result storage.
3. Lightweight RAG on RDS pgvector instead of OpenSearch — suited to project scale and cost.
4. Bedrock Mantle replaces Claude Haiku — unified chat + embedding via Mantle API.
5. No Route 53; use default CloudFront domain for workshop demo/production.

#### 5.3. Expected Outcomes

- Web + desktop platform generating Unit Tests for Java, Python, and .NET.
- End-to-end AWS deployment per Workshop: CloudFront, WAF, API Gateway, ALB, Cognito, EC2, Step Functions, Lambda, S3, RDS, pgvector, Bedrock Mantle, Secrets Manager, CloudWatch, SNS, Budgets.
- AI integrated into the QA workflow with RAG and cost control (~54 USD/month estimated).
- Proposal, Workshop, and report documentation aligned with the actually deployed system.
