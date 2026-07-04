---
title: "Shared prerequisites"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

Prepare the environment and conventions **before Trí → Kiệt → Toàn → Hoa → Trinh** deploy. All **shared** preparation steps live here — each member only adds **member-specific dependencies** on their own overview page.

#### Quick checklist

- [x] 1 shared AWS account + IAM Users `tri`, `kiet`, `toan`, `hoa`, `trinh`
- [x] Region **`ap-southeast-1`**
- [x] [Parameter table](5.2.3-parameter-table/) ready to fill in
- [x] PassRole policy attached to group `zerobug-team`
- [x] `build-all.bat` runs successfully *(Toàn/Hoa)*

#### Contents

1. [AWS account & Region](5.2.1-aws-account/)
2. [Tools & build artifacts](5.2.2-tools-and-build/)
3. [Shared parameter table](5.2.3-parameter-table/)
4. [Conventions & deployment order](5.2.4-conventions-and-order/)
