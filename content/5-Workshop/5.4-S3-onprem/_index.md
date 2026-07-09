---
title: "Configure NAT Gateway"
date: 2026-06-01
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

# Configure NAT Gateway

## Purpose

To allow resources in the **Private Subnet** (e.g., Backend EC2 servers) to download updates and libraries from the Internet **without exposing their IP addresses**, we use a **NAT Gateway**.

The NAT Gateway acts as an intermediary: it receives outbound internet requests from internal servers, performs the requests on their behalf, and returns the results — ensuring backend servers have no direct internet exposure.

---

## Steps

### Step 1: Navigate to NAT Gateways

- From the left-hand menu in the VPC page, select **NAT Gateways**.

---

### Step 2: Verify the Automatically Created NAT Gateway

When creating the VPC using **"VPC and more"** mode, the NAT Gateway named **`rookwork-nat`** was **automatically created** with:
- **Connectivity type**: `Public`
- **Subnet**: Attached to one of the Public Subnets (`rookwork-subnet-public1-...`)
- **Elastic IP**: Automatically allocated

---

### Step 3: Check Status

- Ensure the **State** column of `rookwork-nat` shows **green (✅ Available)**.
- If the status is still `Pending`, wait an additional 1–2 minutes and refresh the page.

---

### Step 4: Confirm Elastic IP Assignment

Ensure the NAT Gateway has been automatically allocated an **Elastic IP (EIP)** — this is the fixed public IP address representing the entire Private Subnet when communicating with the internet.

In the image below, we can see the NAT Gateway has successfully associated (**Status: Succeeded**) with IPs such as `54.254.36.209` and `47.131.240.109`.

![Rookwork NAT Gateway successfully created with Elastic IP](/images/5-Workshop/5.4-S3-onprem/nat-gateway.png)

---

## Results Achieved

- ✅ NAT Gateway `rookwork-nat` is in **Available** (green) state.
- ✅ A fixed **Elastic IP** has been assigned.
- ✅ Private Subnets are routed through the NAT Gateway in their corresponding Route Table (`rookwork-rtb-private1-...`).

> [!NOTE]
> NAT Gateway is billed **per hour** and **per GB of data processed**. Remember to delete this resource after completing the lab in step [5.6 – Resource Cleanup](../5.6-Cleanup/).
