---
title: "Prerequisites"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

# Prerequisites

Before starting the hands-on lab, make sure you have the following conditions ready to avoid errors during implementation.

---

## Requirements

### 1. AWS Account
- Have an **active AWS account**.
- If you don't have one, register at [aws.amazon.com](https://aws.amazon.com/).

### 2. IAM Permissions
- The IAM User account needs **`AmazonVPCFullAccess`** or **`AdministratorAccess`** permissions.
- Do not use the **root** account for daily operations.

> [!WARNING]
> Ensure your IAM account is **not restricted** from creating VPCs, Subnets, NAT Gateways, Security Groups, and VPC Endpoints. Without sufficient permissions, you will encounter `UnauthorizedOperation` errors in the following steps.

### 3. Select the Correct Region
- Select the correct **working region: Asia Pacific (Singapore) (`ap-southeast-1`)** before starting.
- Verify by looking at the top-right corner of the AWS Management Console.

---

## Cost Considerations

> [!NOTE]
> Some resources in this lab may **incur costs** if left running, especially **NAT Gateway** and **Elastic IP**. Make sure to complete the [Resource Cleanup (5.6)](../5.6-Cleanup/) step after finishing the lab.
