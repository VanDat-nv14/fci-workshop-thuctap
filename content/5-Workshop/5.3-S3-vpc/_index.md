---
title: "Initialize VPC and Subnets"
date: 2026-06-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

# Initialize VPC and Subnets

In this step, we will create the entire Rookwork VPC network infrastructure in a single operation using AWS's **"VPC and more"** feature, which automatically creates the VPC, Subnets, Route Tables, Internet Gateway, NAT Gateway, and VPC Endpoint synchronously.

---

## Steps

### Step 1: Access the VPC Service

- Log in to the **AWS Management Console**.
- Search for **VPC** in the search bar and click **Create VPC**.

---

### Step 2: Select the "VPC and more" Option

- Select **`VPC and more`** to create the entire network stack in one go.
- Instead of creating each resource individually (Subnet, Route Table, IGW, etc.), AWS will automatically generate everything based on your configuration.

---

### Step 3: Enter Configuration Parameters

Fill in the following parameters in the configuration form:

| Parameter | Value |
| :--- | :--- |
| **Name tag auto-generation** | `rookwork` |
| **IPv4 CIDR block** | `10.0.0.0/16` (default) |
| **Number of Availability Zones** | `2` (`ap-southeast-1a` and `ap-southeast-1b`) |
| **Number of public subnets** | `2` (auto-creates `rookwork-subnet-public1-...` and `rookwork-subnet-public2-...`) |
| **Number of private subnets** | `2` (auto-creates `rookwork-subnet-private1-...` and `rookwork-subnet-private2-...`) |
| **NAT gateways** | `1 per AZ` |
| **VPC endpoints** | `S3 Gateway` (auto-creates `rookwork-vpce-s3`) |

---

### Step 4: Preview and Create VPC

- Click **Preview** to visualize the resource map that will be created.
- Click **Create VPC** and wait for the system to deploy (approximately 1–2 minutes).

Below is the resulting **Resource Map** showing that the Route Tables (`rookwork-rtb-public`, `rookwork-rtb-private1-ap-southeast-1a`, etc.) have been correctly configured.

![Rookwork VPC Resource Map after successful initialization](/images/5-Workshop/5.1-Workshop-overview/vpc-architecture.png)

---

## Results Achieved

After completing this step, the system will have automatically created the following resources:

- ✅ **1 VPC**: `rookwork-vpc` with CIDR `10.0.0.0/16`
- ✅ **4 Subnets**: 2 Public + 2 Private across 2 AZs
- ✅ **1 Internet Gateway**: `rookwork-igw`
- ✅ **1 NAT Gateway**: `rookwork-nat` (with automatically assigned Elastic IP)
- ✅ **3 Route Tables**: 1 Public + 2 Private
- ✅ **1 VPC Endpoint**: `rookwork-vpce-s3` (S3 Gateway)

> [!TIP]
> After creation, navigate to the **Resource Map** section (within the VPC detail page) to visually verify that the entire network connection diagram has been set up correctly.
