---
title: "Troubleshooting"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.8.4. </b> "
---

| Symptom | Cause | Member |
| --- | --- | --- |
| CloudFront 502 | EC2 not running / unhealthy | Toàn |
| 401 on every request | Missing/invalid JWT | Trinh |
| SSM Offline | Missing role / NAT | Trí, Toàn |
| Target Unhealthy | App not started | Toàn |
| Bedrock Mantle timeout / 401 | NAT; expired API key; model access | Trí, Hoa, Kiệt |
| Embedding 400 / dimension mismatch | Wrong model or vector ≠ 1024; missing Cohere `input_type` | Hoa, Kiệt |
| RAG returns empty context | Not embedded yet (`code_embeddings` empty); wrong `project_id`; Context Builder not run | Hoa |
| `ERROR: extension "vector" does not exist` | Kiệt has not run `CREATE EXTENSION vector` on RDS | Kiệt |
| Poor chat quality (missing context) | Retrieve top-k too small; chunk too large; vector index missing | Hoa |
| DB fail | SG / secret / endpoint | Kiệt, Toàn |
