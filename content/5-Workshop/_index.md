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

### Workshop – Deploying VPC Infrastructure and Backend CI/CD for Rookwork

A step-by-step guide to building a High Availability AWS VPC network system and setting up an automated CI/CD pipeline to deploy the Spring Boot backend application.

1. **[5.1 – Workshop Overview](5.1-Workshop-overview/)** – System architecture and components to be deployed
2. **[5.2 – Prerequisites](5.2-Prerequisite/)** – Verify IAM account and working region
3. **[5.3 – Set Up VPC Network Infrastructure](5.3-S3-vpc/)** – Initialize VPC, NAT Gateway, and layered Security Groups
4. **[5.4 – Deploy Backend CI/CD with GitHub Actions](5.4-Backend-CICD/)** – Configure GitHub Secrets and automation pipeline
5. **[5.5 – Resource Cleanup](5.5-Cleanup/)** – Delete resources after completing the lab
