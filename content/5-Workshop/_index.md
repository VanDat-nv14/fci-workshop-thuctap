---
title: "Workshop"
date: 2026-04-17
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Hands-on Workshop

This section documents the in-depth hands-on lab carried out during the internship, focusing on deploying the AWS network infrastructure for the **Rookwork** group project.

The workshop follows a consistent structure: **Objectives → Architecture → Implementation Steps → Results & Lessons Learned**.

---

### Workshop – Deploying VPC Network Infrastructure for Rookwork

A step-by-step guide to building a High Availability AWS VPC network system, including Subnets, NAT Gateway, Internet Gateway, VPC Endpoint, and layered Security Groups.

1. **[5.1 – Workshop Overview](5.1-Workshop-overview/)** – System architecture and components to be deployed
2. **[5.2 – Prerequisites](5.2-Prerequisite/)** – Verify IAM account and working region
3. **[5.3 – Initialize VPC and Subnets](5.3-S3-vpc/)** – Create entire network infrastructure using "VPC and more"
4. **[5.4 – Configure NAT Gateway](5.4-S3-onprem/)** – Allow Private Subnet to safely access the internet
5. **[5.5 – Set Up Security Groups](5.5-Policy/)** – Layered virtual firewall for ALB and Backend
6. **[5.6 – Resource Cleanup](5.6-Cleanup/)** – Delete resources after completing the lab
