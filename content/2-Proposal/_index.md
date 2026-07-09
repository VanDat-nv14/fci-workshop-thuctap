---
title: "Proposal"
date: 2026-04-17
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Rookwork – Task Management & Team Collaboration Platform on AWS
## A Complete Cloud Infrastructure Solution for a Real-World Application on Amazon Web Services

---

### 1. Executive Summary

**Rookwork** is a multi-platform task management and team collaboration application, supporting both **Web (Browser)** and **Desktop (Electron)** environments. The application enables teams to organize projects, assign tasks, track progress, and communicate in real-time via WebSocket.

This project was carried out during an internship at **Amazon Web Services Vietnam Co., Ltd.** (17/04/2026 – 12/07/2026), under the mentorship of **Nguyen Gia Hung**. The core objective is to build a complete system — from application development to full AWS cloud infrastructure deployment — following real industry standards. This includes a 3-tier architecture, VPC network segmentation, relational database on RDS, global content delivery via CloudFront, and a fully automated CI/CD pipeline using GitHub Actions.

---

### 2. Problem Statement

**Current Problem:**
Popular team productivity tools such as Trello, Jira, and Notion are widely adopted but are primarily third-party SaaS solutions. They tend to be costly at scale and do not allow full control over data sovereignty or underlying infrastructure. There is currently no locally developed solution built and operated transparently on AWS Cloud infrastructure.

**Proposed Solution:**
Rookwork is deployed on AWS using a 3-tier architecture: a React/Vite Frontend served globally via Amazon S3 + CloudFront, a Spring Boot Backend running in a Docker container on Amazon EC2, and an Amazon RDS PostgreSQL database placed in a Private Subnet — fully shielded from the internet. The entire CI/CD process is automated via GitHub Actions, from developer code push to end-user receiving the latest update. Security follows a Defense-in-Depth approach with Bastion Host, IAM Least Privilege, Spring Security + JWT, and AWS Secrets Manager.

**Benefits and ROI:**
The project delivers a real, immediately usable software product, while serving as a comprehensive learning platform covering the full AWS Software Development Lifecycle (SDLC). Infrastructure costs are kept low by leveraging AWS Free Tier and resource optimization. The system is easily scalable with an ALB + Auto Scaling Group architecture combined with automated CI/CD, significantly reducing time and risk when releasing new software versions.

---

### 3. Solution Architecture

Rookwork follows a **3-tier architecture** on AWS, deployed in region `ap-southeast-1` (Singapore):

```
[ End Users (Web / Desktop) ]
        │
        ▼
[ Amazon CloudFront (CDN) ] ←── [ Amazon S3 (Frontend Static Files) ]
        │
        ▼ (API requests to api.rookwork.asia)
[ Amazon Route 53 (DNS) ]
        │
        ▼
[ Application Load Balancer (ALB) ]
        │
        ▼
[ Auto Scaling Group ]
  └── [ Amazon EC2 – Docker Container (Spring Boot Backend, port 8080) ]
        │
        ├── [ AWS Secrets Manager (DB Credentials, JWT Secret) ]
        │
        ▼
[ Amazon RDS – PostgreSQL (Private Subnet) ]
```

**Network Layer (VPC Architecture):**

| Layer | Resources | Network Location |
| :--- | :--- | :--- |
| **Presentation Layer** | Amazon S3 + CloudFront, domain `rookwork.asia` | Public – AWS Edge Locations |
| **Application Layer** | ALB + Auto Scaling Group + EC2 (Spring Boot in Docker) | Public Subnet (SSH secured by Bastion Host) |
| **Data Layer** | Amazon RDS PostgreSQL | Private Subnet (no direct internet exposure) |

**AWS Services Used:**

| AWS Service | Role in System |
| :--- | :--- |
| **Amazon EC2** | Server running Docker container for Spring Boot Backend |
| **Amazon RDS (PostgreSQL)** | Relational database in Private Subnet |
| **Amazon S3** | Storage for Frontend static build files (React/Vite) |
| **Amazon CloudFront** | CDN for global Frontend delivery with cache optimization |
| **Amazon Route 53** | DNS management, resolving domain `rookwork.asia` |
| **AWS VPC** | Secure network segmentation (Public/Private Subnet) |
| **Application Load Balancer** | Load balancing and traffic routing to Backend |
| **Auto Scaling Group** | Automatically scales EC2 instances based on traffic load |
| **AWS IAM** | Access control for GitHub Actions and AWS resources |
| **AWS Secrets Manager** | Secure storage for sensitive credentials (DB, JWT Secret) |
| **AWS Bastion Host** | Secure SSH gateway for administering internal servers |

---

### 4. Technical Implementation

**Technology Stack:**

*Backend:*

| Component | Technology | Version |
| :--- | :--- | :--- |
| Language | Java | 21 (LTS) |
| Framework | Spring Boot | 4.0.2 |
| Security | Spring Security + JWT (JJWT 0.12.6) | – |
| Third-party Auth | Google OAuth2 | – |
| ORM | Spring Data JPA + Hibernate | – |
| Database Migration | Flyway (PostgreSQL) | – |
| Real-time Communication | Spring WebSocket + STOMP | – |
| Secret Management | AWS Secrets Manager (spring-cloud-aws 4.0.2) | – |
| Containerization | Docker (eclipse-temurin:21-jdk) | – |
| Build Tool | Apache Maven | – |

*Frontend:*

| Component | Technology | Version |
| :--- | :--- | :--- |
| UI Framework | React | 19 |
| Build Tool | Vite | – |
| CSS Framework | Tailwind CSS | v4 |
| Desktop App | Electron | 28 |
| Routing | React Router DOM | v7.13.0 |
| Realtime | STOMP.js + SockJS | – |
| Animation | Motion (Framer Motion) | v12 |
| Drag & Drop | React DnD | v16 |
| Package Manager | Yarn Workspaces (Monorepo) | – |

**CI/CD Pipeline (GitHub Actions):**

The system uses **GitHub Actions** to fully automate the build and deploy process upon new code pushed to the `main` branch:

*Backend Pipeline:*
```
Push to main → Checkout code → Setup JDK 21 → Maven Build JAR
→ SCP copy JAR to EC2 → SSH into EC2 → Docker stop/rm old container
→ Docker build new image → Docker run new container (port 8080)
```

*Frontend Pipeline:*
```
Push to main → Checkout code → Setup Node.js 22 → Yarn install
→ Vite Build (VITE_API_URL=https://api.rookwork.asia)
→ Configure AWS Credentials → S3 sync assets (cache: 1 year)
→ S3 upload index.html (no-cache) → CloudFront Invalidation
```

---

### 5. Roadmap & Milestones

| Phase | Week | Key Activities |
| :---: | :---: | :--- |
| **Cloud Foundations** | Week 1–2 | Learn AWS basics: IAM, EC2, S3, MFA, Cost Management |
| **Networking & Storage** | Week 3–4 | VPC, Bastion Host, NAT Gateway, S3 Static Web, CloudFront |
| **Application Deployment** | Week 5–6 | Deploy app to EC2, connect RDS, configure Route 53 |
| **Real-world Infrastructure** | Week 7–8 | Build actual Rookwork VPC, ALB, Auto Scaling, CI/CD Pipeline |
| **Production & Monitoring** | Week 9–10 | Full Production deployment, HTTPS, CloudWatch, security & cost optimization |
| **Wrap-up & Handover** | Week 11–12 | Load Testing, internal Demo, Bug Fix, report writing, project handover |

---

### 6. Budget Estimation

**AWS Infrastructure Cost (estimated monthly):**

| Service | Configuration | Estimated Cost |
| :--- | :--- | :--- |
| Amazon EC2 (t3.micro) | 1 instance, Free Tier for 12 months | ~$0.00/month |
| Amazon RDS (db.t3.micro) | PostgreSQL, Single-AZ, 20 GB SSD | ~$0.00/month (Free Tier) |
| Amazon S3 | 5 GB storage, ~10,000 requests | ~$0.12/month |
| Amazon CloudFront | 10 GB data transfer out | ~$0.00/month (Free Tier) |
| Amazon Route 53 | 1 hosted zone | ~$0.50/month |
| AWS Secrets Manager | 2 secrets | ~$0.80/month |
| **Total Estimate** | | **~$1.42/month** |

> *Note: Actual costs may vary based on traffic volume and usage beyond AWS Free Tier limits.*

---

### 7. Risk Assessment

**Risk Matrix:**

| Risk | Impact | Probability | Mitigation Strategy |
| :--- | :---: | :---: | :--- |
| EC2 instance failure / overload | High | Low | Auto Scaling Group + ALB automatically distributes traffic and launches new instances |
| RDS data loss | Very High | Very Low | Daily automated backups + periodic manual snapshots enabled |
| AWS budget overrun | Medium | Medium | Set up AWS Budget Alerts, regularly review Cost Explorer |
| Sensitive credential exposure | High | Low | Use AWS Secrets Manager + IAM Least Privilege, never hardcode credentials |
| Failed deployment (rollback needed) | Medium | Medium | Keep previous Docker image, rollback by re-running old container version |

**Contingency Plans:**
- Disable Auto Scaling and run manually on a single EC2 if the CI/CD pipeline encounters failures.
- Use RDS Snapshot to restore data within 5 minutes in the event of a critical failure.
- Can downgrade to a simpler deployment model (without ALB) to reduce costs if necessary.

---

### 8. Expected Outcomes

- The **Rookwork** system is fully and stably deployed on the AWS Production environment with official domain `rookwork.asia`.
- **Fully automated CI/CD pipeline** operates end-to-end: from developer code push to end users receiving the latest update with no manual intervention.
- Infrastructure is designed following **Defense-in-Depth** and **Least Privilege** security principles.
- Students gain real hands-on experience operating a complete Cloud system end-to-end, including architecture design, deployment, monitoring, and optimization — ready to apply in a professional work environment.
- The Rookwork team delivers a **production-ready software product** — not just a demo or dev environment prototype.
