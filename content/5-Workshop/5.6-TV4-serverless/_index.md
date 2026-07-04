---
title: "Hoa – Serverless: Lambda & Step Functions"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

**Owner:** Hoa  
**Depends on Trí + Kiệt:** NAT Gateway, `zerobug-lambda-sg`, IAM Roles; S3 bucket, RDS endpoint, DB Secret, and Bedrock Mantle parameters.

#### What does Hoa do?

Deploys the core serverless AI pipeline for ZeroBug Agent. This involves creating six independent Lambda functions, orchestrating them with an AWS Step Functions state machine, and integrating with Amazon Bedrock Mantle for RAG and code generation.

#### Components & where they are used

| Component | Function | Used by |
| --- | --- | --- |
| **`ProjectImportLambda`** | Processes project source (Git/Zip) and uploads to S3. | Start of the Step Functions workflow. |
| **`ContextBuilderLambda`** | Builds RAG context: chunks source, creates embeddings via Mantle, stores/retrieves vectors from RDS pgvector. | Step Functions, before AI invocation. |
| **`SourceFileServiceLambda`** | Provides file tree and file content for the IDE. | Step Functions and IDE view. |
| **`BedrockInvokeLambda`** | Calls Bedrock Mantle's chat model (`openai.gpt-oss-120b`) with context to generate the unit test. | The core AI generation step in the workflow. |
| **`ResultServiceLambda`** | Saves the generated test result to RDS. | End of the workflow. |
| **`HistoryServiceLambda`** | Logs the generation event to the history table in RDS. | End of the workflow. |
| **Step Functions** | The "orchestrator" that chains all Lambda functions into an automated, fault-tolerant workflow. | Triggered by the EC2 backend (Toàn). |

#### AI Design

The entire AI process is offloaded from the EC2 instance to a serverless pipeline orchestrated by Step Functions.

- **Workflow:** `EC2 (Toàn) → Step Functions → ProjectImportLambda → S3 → ContextBuilderLambda (embed + pgvector) → BedrockInvokeLambda (chat) → Result/History Lambdas → RDS`
- **Chat Model:** `openai.gpt-oss-120b` via Mantle (`/v1/chat/completions`).
- **Embedding Model:** `cohere.embed-multilingual-v3` via Mantle (`/v1/embeddings`).
- **Authentication:** A Bearer token (`BEDROCK_MANTLE_API_KEY`) is stored as a Lambda environment variable for both `ContextBuilderLambda` and `BedrockInvokeLambda`.
- **Networking:** Lambdas run in a private subnet and make cross-region calls (`ap-southeast-1` → `us-east-1`) to the Bedrock Mantle endpoint via the NAT Gateway (Trí).

#### Hoa-specific preparation

- Collect all necessary parameters (VPC, Subnets, SGs, Roles, S3, RDS, Mantle URLs/models) from the shared parameter table.
- Run `build-all.bat` locally to generate the deployment artifacts for each Lambda function.
- Create a Bedrock API key in the **`us-east-1`** console and have it ready to be configured as the `BEDROCK_MANTLE_API_KEY` environment variable on the Lambdas.

#### Deployment content

1. Lambda Setup
2. AWS Step Functions
3. EC2 Configuration & Workflow Trigger
4. Monitoring & Debugging
5. End-to-End Verification (Verifying Handoff)