---
title: "Schema (JPA)"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---

#### Option 1 — Recommended (Kiệt does not write SQL by hand)

**Do not create application tables at Kiệt's step.** Toàn deploys Spring Boot with:

```properties
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:postgresql://<RDS-ENDPOINT>:5432/zerobug
DB_SECRET_NAME=zerobug/rds/credentials
```

On first EC2 startup, JPA automatically creates `users`, `projects`, `generation_records`.

{{% notice note %}}
When **Kiệt finishes** S3 + RDS + Secrets + pgvector (section below) → **hand off to Toàn** to continue. No need to wait for application schema to exist first.
{{% /notice %}}

#### RAG — pgvector on RDS

The workshop uses **lightweight RAG (A)**: vectors stored on the **same RDS PostgreSQL** instance, table **`code_embeddings`**. Kiệt runs SQL **once** after RDS is **Available** (temporarily enable public access or use a Session Manager bastion if the team has one — similar to Option 2 below).

1. Connect to database **`zerobug`** using the master user or user from the secret.
2. Run:

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

-- Optional: vector index for faster retrieval (can skip for small workshop)
-- CREATE INDEX ON code_embeddings USING ivfflat (embedding vector_cosine_ops) WITH (lists = 32);
```

3. Record **`code_embeddings`** and dimension **`1024`** in the [parameter table](../../5.2-prerequisites/5.2.3-parameter-table/).
4. Lock down RDS public access again if it was temporarily opened to run SQL.

**Hoa** (`ContextBuilderLambda`) will **INSERT** chunks + embeddings and **SELECT** top-k — Kiệt does not need to write additional SQL after this step.

{{% notice warning %}}
Column `embedding vector(1024)` matches model **`cohere.embed-multilingual-v3`**. Changing the embedding model requires changing the dimension and migrating the table.
{{% /notice %}}

#### Option 2 — Create all SQL by hand *(not recommended)*

1. Modify RDS → Public access Yes + My IP rule on RDS-SG.
2. pgAdmin/psql → run SQL to create application tables *(and pgvector as above if not yet run)*.
3. Lock down again: Public access No, remove My IP rule.

#### After stabilization

Change `ddl-auto` → `validate` in production.
