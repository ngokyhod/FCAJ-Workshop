---
title: "IGW & NAT"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.3.5. </b> "
---

#### Internet Gateway

1. **Internet gateways** → **Create internet gateway** → Name: `zerobug-igw` → **Create**.
2. Select IGW → **Actions** → **Attach to VPC** → `zerobug-vpc` → **Attach**.

![](/images/5-Workshop/5.3/31.jpg)
![](/images/5-Workshop/5.3/32.png)
![](/images/5-Workshop/5.3/33.png)

#### NAT Gateway

NAT allows **private** subnets to reach the internet: EC2/SSM, S3 artifact downloads, and Lambda VPC (**`ContextBuilderLambda`**, **`BedrockInvokeLambda`**) calling **`bedrock-mantle.us-east-1.api.aws`** — embed + chat cross-region from `ap-southeast-1`.

1. **NAT gateways** → **Create NAT gateway**.
2. Name: `zerobug-nat`.
3. **Subnet:** `zerobug-public-a` *(NAT must be in a public subnet)*.
4. **Connectivity:** Public.
5. **Elastic IP:** **Allocate Elastic IP**.
6. → **Create**. Wait until **Available** (a few minutes).
7. Copy **NAT Gateway ID**.

![](/images/5-Workshop/5.3/34.png)
![](/images/5-Workshop/5.3/35.png)

{{% notice warning %}}
**NAT is not free tier** (~0.045 USD/hour). Create only **1 NAT** (not one per AZ). Delete + release Elastic IP when finished (section 5.10).
{{% /notice %}}

<!-- Image: /images/5-Workshop/5.3-Trí/nat-gateway.png -->
