---
title: "Set Up Security Groups"
date: 2026-06-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

# Set Up Security Groups

## Purpose

A **Security Group** acts as a **Virtual Firewall** at the instance level, controlling **Inbound** (incoming) and **Outbound** (outgoing) traffic flows. In the Rookwork system, we apply a **Layered Security** model:

- **`rookwork-alb-sg`**: Only opens Ports 80/443 to the entire internet.
- **`backend-sg`**: Only accepts traffic from **`rookwork-alb-sg`** — the backend has absolutely no direct internet exposure.

Both Security Groups reside within VPC **`vpc-027222b4e12adb751`** (the rookwork VPC).

---

## Steps

### Step 1: Navigate to Security Groups

- From the left-hand menu, select **Security Groups** under the **Security** section.
- Click **Create security group**.

---

### Step 2: Create SG for Load Balancer (`rookwork-alb-sg`)

Fill in the following information:

| Field | Value |
| :--- | :--- |
| **Security group name** | `rookwork-alb-sg` |
| **Description** | Security group for Application Load Balancer |
| **VPC** | Select the `rookwork` VPC |

**Inbound rules:**

| Type | Protocol | Port Range | Source |
| :--- | :--- | :--- | :--- |
| HTTP | TCP | 80 | `0.0.0.0/0` (Anywhere) |
| HTTPS | TCP | 443 | `0.0.0.0/0` (Anywhere) |

**Outbound rules:** Keep default (All traffic – `0.0.0.0/0`)

Click **Create security group** to save.

---

### Step 3: Create SG for Backend (`backend-sg`)

Fill in the following information:

| Field | Value |
| :--- | :--- |
| **Security group name** | `backend-sg` |
| **Description** | Security group for Backend EC2 instances |
| **VPC** | Select the `rookwork` VPC |

**Inbound rules:**

| Type | Protocol | Port Range | Source |
| :--- | :--- | :--- | :--- |
| Custom TCP | TCP | 8080 | `rookwork-alb-sg` (Security Group ID) |

> [!IMPORTANT]
> In the **Source** field, instead of entering a CIDR IP, select **Security Group** and type `rookwork-alb-sg`. This ensures the backend **only accepts** requests from the Load Balancer, never directly from any internet source.

**Outbound rules:** Keep default (All traffic – `0.0.0.0/0`)

Click **Create security group** to save.

---

## Results Achieved

![Rookwork Security Groups successfully created](/images/5-Workshop/5.5-Policy/security-groups.png)

After completing these steps, you will see 2 new Security Groups in the list:

- ✅ `rookwork-alb-sg` – Allows HTTP/HTTPS from anywhere.
- ✅ `backend-sg` – Only allows Port 8080 from `rookwork-alb-sg`.

> [!TIP]
> This model is called **Defense-in-Depth**: even if an attacker bypasses the Load Balancer, the backend cannot be accessed directly from the internet.
