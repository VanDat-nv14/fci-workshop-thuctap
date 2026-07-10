---
title: "Proposal"
date: 2026-04-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Rookwork - Team Collaboration Management Software

### 1. Project Overview
In the context of the Industry 4.0 revolution and the growing trend of remote working and hybrid work, effective team management has become a critical factor for individuals, project teams, and businesses alike. **Rookwork** is an integrated, modern team collaboration management software designed to operate as a multi-platform application (browser and desktop app). The system uses a Client-Server architecture with a frontend built using **React 19**, which can be packaged as a desktop application using **Electron** for a seamless experience, alongside a robust backend built on **Spring Boot** combined with AWS services.

> **Note on current status:** At this stage, the primary platform being used is the **web browser** version (runs directly in the browser, no installation required). The **desktop version (packaged with Electron)** has been developed but **has not been fully synchronized** with the latest web version, and may therefore be missing some features or patches compared to the current web release. The long-term goal of the project remains to complete and synchronize the multi-platform desktop application as originally planned.

### 2. Problem Statement
Many teams still face major challenges in collaboration and project management:
- **Scattered tools:** Many teams still manage work manually through asynchronous channels such as Excel, Google Sheets, Messenger/Zalo, and email. This makes it difficult to track progress, leads to mistakes, lacks transparency, and wastes time on unnecessary communication.
- **Complex usability:** Existing project management platforms (such as Trello, Jira, Asana, Monday.com) often have complex interfaces and workflows with multiple layers of configuration, requiring users to spend time learning before they can use them effectively. This is especially challenging for small teams, students, or beginners who need a simple, intuitive tool that is accessible from the very first use.

### 3. Solution Architecture (Workflow)
The system operates on a distributed model within the AWS Cloud environment. The processing flow is structured as follows:

![Rookwork Architecture](/images/2-Proposal/workflow.png)

**1. Networking & Content Delivery Flow:**
- All requests from users are resolved via **Amazon Route 53** (DNS Resolution).
- The static Frontend interface is fully hosted on an **Amazon S3 (FE Static)** bucket.
- To accelerate page load times and enhance security, the frontend is distributed via **Amazon CloudFront**, coupled with the **AWS WAF** firewall to prevent web exploits.

**2. Routing & Compute Backend Flow:**
- API traffic passes through the **Internet Gateway** into the **Amazon VPC**, and is then load-balanced by the **Application Load Balancer (ALB)**.
- The Java Spring Boot Backend is deployed on **Amazon EC2** virtual machines, securely placed inside *Private Subnets*.
- For EC2 instances to communicate with the internet (e.g., calling external APIs), outbound traffic is routed through a **NAT Gateway**.

**3. Database & Storage Flow:**
- Core business data is stored on **Amazon RDS (PostgreSQL)**, deployed in a **Multi-AZ (DB Replication)** model across multiple Availability Zones for disaster recovery.
- Database schema changes are managed via Flyway.
- Attachments are securely written to S3 by EC2 instances via an internal route using the **VPC S3 Gateway Endpoint**. Users can subsequently download files safely via Direct File Access or CloudFront.

**4. Notifications & Shared Services Flow:**
- Events that trigger emails (such as team invitations) prompt EC2 to directly invoke **Amazon SES** to deliver emails to users.
- SSL/TLS certificates are centrally managed by **AWS Certificate Manager (ACM)**, tightly integrated with system permissions governed by **AWS IAM**.

### 4. Technical Implementation
*Technical Requirements*
- **Backend:** Java Spring Boot, Spring Security (JWT/OAuth2), Spring Data JPA.
- **Database:** PostgreSQL, Flyway (Database Migration).
- **Frontend:** ReactJS / TypeScript.
- **AWS Services Utilized:**
  - *Network & Security:* Amazon Route 53, Amazon CloudFront, AWS WAF, Amazon VPC (IGW, NAT Gateway, ALB, S3 Gateway Endpoint), AWS ACM, AWS IAM.
  - *Compute & Storage:* Amazon EC2, Amazon RDS (PostgreSQL Multi-AZ), Amazon S3 (FE Static & File Storage).
  - *Others:* Amazon SES (Email Notification).

### 5. Implementation Roadmap
- *Phase 1:* Design AWS Cloud architecture, configure VPC, Subnets, Route 53, and IAM.
- *Phase 2:* Build the Spring Boot Backend and PostgreSQL database (RDS Multi-AZ).
- *Phase 3:* Deploy Backend onto EC2 with ALB, and configure NAT Gateway.
- *Phase 4:* Develop Frontend, upload to S3, and configure CloudFront + WAF.
- *Phase 5:* Integrate S3 Endpoint for file storage and Amazon SES for sending emails.
- *Phase 6:* End-to-end testing, security review, and system handover.

### 6. Operational Costs
The Rookwork system is deployed on AWS following a High Availability architecture standard with three layers of security. The platform relies on core services including an EC2 cluster running the Spring Boot backend, an Amazon RDS PostgreSQL database, and Amazon S3 for hosting the React 19 frontend and static assets.

In the current phase, as the system primarily serves internal testing and pre-council acceptance review with limited traffic, the estimated total infrastructure cost is maintained at an optimal level of approximately **$136 USD/month** (equivalent to **3.48 million VND**). This budget includes:
- A **Multi-AZ** redundancy setup for both the backend and database tiers.
- A **NAT Gateway** to enforce network security within Private Subnets.
- An **S3 Gateway Endpoint** that reduces internal bandwidth costs to zero.

If further budget reduction is required during this evaluation phase, the project can flexibly switch to a **Single-AZ** configuration or implement scheduled auto start/stop scripts for non-business hours.

### 7. Risk Assessment
*Risk Matrix*
- VPC routing misconfigurations preventing EC2 from accessing the internet or DB: High impact, medium probability.
- AWS budget overruns due to NAT Gateway uptime or Multi-AZ configurations: Medium impact, high probability (for learning accounts).

*Mitigation Strategy*
- Configure infrastructure using Infrastructure as Code (IaC) for easier control and rollback.
- Set strict AWS Budgets and configure CloudWatch to monitor network traffic. Disable DB Replication (Multi-AZ) in Dev environments to save costs.