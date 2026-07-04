---
title: "Schema (JPA)"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---

#### Cách 1 — Khuyến nghị (Kiệt không tạo SQL tay)

**Không tạo bảng ứng dụng ở Kiệt.** Toàn deploy Spring Boot với:

```properties
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:postgresql://<RDS-ENDPOINT>:5432/zerobug
DB_SECRET_NAME=zerobug/rds/credentials
```

Khi EC2 khởi động lần đầu, JPA tự tạo `users`, `projects`, `generation_records`.

{{% notice note %}}
**Kiệt xong** phần S3 + RDS + Secrets + pgvector (mục dưới) → **chuyển Toàn** làm tiếp. Không cần đợi schema ứng dụng có sẵn.
{{% /notice %}}

#### RAG — pgvector trên RDS

Workshop dùng **RAG gọn (A)**: vector lưu trên **cùng RDS PostgreSQL**, bảng **`code_embeddings`**. Kiệt chạy SQL **một lần** sau khi RDS **Available** (tạm mở public hoặc dùng Session Manager bastion nếu nhóm có — tương tự Cách 2 bên dưới).

1. Kết nối database **`zerobug`** bằng master user hoặc user từ secret.
2. Chạy:

```sql
CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE IF NOT EXISTS code_embeddings (
  id           BIGSERIAL PRIMARY KEY,
  project_id   BIGINT NOT NULL,
  chunk_index  INT NOT NULL,
  content      TEXT NOT NULL,
  embedding    vector(1024),
  created_at   TIMESTAMP DEFAULT NOW()
);

CREATE INDEX IF NOT EXISTS idx_code_embeddings_project
  ON code_embeddings (project_id);

-- Tùy chọn: index vector cho retrieve nhanh (workshop nhỏ có thể bỏ qua)
-- CREATE INDEX ON code_embeddings USING ivfflat (embedding vector_cosine_ops) WITH (lists = 32);
```

3. Ghi **`code_embeddings`** và dimension **`1024`** vào [bảng tham số](../../5.2-prerequisites/5.2.3-parameter-table/).
4. Khóa lại public access RDS nếu đã mở tạm để chạy SQL.

**Hoa** (`ContextBuilderLambda`) sẽ **INSERT** chunk + embedding và **SELECT** top-k — không cần Kiệt viết thêm SQL sau bước này.

{{% notice warning %}}
Cột `embedding vector(1024)` khớp model **`cohere.embed-multilingual-v3`**. Đổi model embedding → phải đổi dimension và migrate bảng.
{{% /notice %}}

#### Cách 2 — Tạo SQL tay toàn bộ *(không khuyến nghị)*

1. Modify RDS → Public access Yes + rule My IP trên RDS-SG.
2. pgAdmin/psql → chạy SQL tạo bảng ứng dụng *(và pgvector như trên nếu chưa chạy)*.
3. Khóa lại: Public access No, xóa rule My IP.

#### Sau khi ổn định

Đổi `ddl-auto` → `validate` ở production.
