---
title: "Resource Cleanup"
date: 2026-06-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

# Resource Cleanup

> [!CAUTION]
> **Important:** Resources such as **NAT Gateway** and **Elastic IP** will **continuously incur hourly charges** even when there is no traffic. Please complete all cleanup steps after finishing the lab to avoid unexpected costs.

---

## Cleanup Order

Follow the steps below in the exact order, as some resources depend on others:

### Step 1: Delete NAT Gateway

1. Navigate to **VPC → NAT Gateways**.
2. Select NAT Gateway **`rookwork-nat`**.
3. Click **Actions → Delete NAT gateway**.
4. Confirm and wait for the status to change to `Deleted` (this process takes approximately **2–3 minutes**).

> [!WARNING]
> You must delete the NAT Gateway **before** releasing the Elastic IP. If done in reverse order, AWS will throw an error indicating the resource is still in use.

---

### Step 2: Release Elastic IPs

1. Navigate to **VPC → Elastic IPs**.
2. Select the allocated IP addresses (e.g., `54.254.36.209`, `47.131.240.109`).
3. Click **Actions → Release Elastic IP addresses**.
4. Confirm to fully release them.

---

### Step 3: Delete the VPC

1. Navigate to **VPC → Your VPCs**.
2. Select VPC **`vpc-027222b4e12adb751`** (the rookwork VPC).
3. Click **Actions → Delete VPC**.
4. Confirm by typing `delete`.

> [!NOTE]
> When deleting the VPC, AWS will **automatically delete** all dependent resources including:
> - ✅ 4 Subnets (`rookwork-subnet-public1/2-...`, `rookwork-subnet-private1/2-...`)
> - ✅ 3 Route Tables (`rookwork-rtb-public`, `rookwork-rtb-private1/2-...`)
> - ✅ Internet Gateway (`rookwork-igw`)
> - ✅ 2 Security Groups (`rookwork-alb-sg`, `backend-sg`)
> - ✅ VPC Endpoint (`rookwork-vpce-s3`)

---

## Post-Cleanup Verification

After completing all steps, verify the following:

- [ ] The **NAT Gateways** list no longer shows `rookwork-nat`.
- [ ] The **Elastic IPs** list is empty or no longer contains the used IPs.
- [ ] The **Your VPCs** list no longer shows the rookwork VPC.
- [ ] The **Security Groups** list no longer contains `rookwork-alb-sg` and `backend-sg`.

---

## Workshop Summary

Congratulations! You have successfully completed the entire **Deploying VPC Network Infrastructure for Rookwork** workshop. Through this hands-on lab, you have learned:

- 🌐 How to design and deploy a High Availability AWS VPC network architecture across 2 AZs.
- 🔒 How to apply a Layered Security model using Security Groups.
- 🔀 How NAT Gateway enables Private Subnet resources to safely access the internet.
- 💰 The importance of resource cleanup to control AWS costs.
