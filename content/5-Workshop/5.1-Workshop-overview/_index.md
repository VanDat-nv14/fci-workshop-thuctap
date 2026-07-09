---
title: "Overview"
date: 2026-06-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# Workshop Overview – Deploying the VPC Network Infrastructure for Rookwork

## Objectives

In this hands-on lab, we will deploy a basic AWS VPC network system with **High Availability**. This is the actual network infrastructure applied to the **Rookwork** group project during the internship.

---

## System Architecture

The system architecture is hosted in the **Asia Pacific (Singapore) `ap-southeast-1`** region and consists of the following key components:

| Component | Details |
| :--- | :--- |
| **VPC** | Initialize a Virtual Private Cloud to isolate network resources |
| **Availability Zones (AZs)** | Resources distributed across 2 AZs (`ap-southeast-1a` and `ap-southeast-1b`) for High Availability |
| **Subnets** | 4 subnets: **2 Public Subnets** (for Load Balancer, NAT Gateway) and **2 Private Subnets** (for secure backend) |
| **Internet Gateway** | `rookwork-igw` – Public network outbound internet access |
| **NAT Gateway** | `rookwork-nat` – Allows private network one-way outbound internet access |
| **VPC Endpoint** | `rookwork-vpce-s3` – Secure internal connection to S3 (bypassing public internet) |
| **Security Groups** | `rookwork-alb-sg` (for ALB) and `backend-sg` (for Backend EC2) |

---

## Architecture Diagram

![VPC Rookwork Architecture Diagram](/images/5-Workshop/5.1-Workshop-overview/vpc-architecture.png)

---

## Lab Steps in This Workshop

1. **[5.2 – Prerequisites](../5.2-Prerequisite/):** Verify your account and working region.
2. **[5.3 – Initialize VPC and Subnets](../5.3-S3-vpc/):** Create the entire network infrastructure using "VPC and more" mode.
3. **[5.4 – Configure NAT Gateway](../5.4-S3-onprem/):** Verify the NAT Gateway is functioning correctly.
4. **[5.5 – Set Up Security Groups](../5.5-Policy/):** Create layered virtual firewalls for ALB and Backend.
5. **[5.6 – Resource Cleanup](../5.6-Cleanup/):** Delete resources to avoid unexpected charges.
