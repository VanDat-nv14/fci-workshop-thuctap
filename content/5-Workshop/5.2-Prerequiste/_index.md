---
title : "Prerequisite"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

Before starting the AWS infrastructure deployment for the **Rookwork** project, ensure all of the following prerequisites are in place.

---

#### 1. AWS Account

- An active **AWS account** (AWS Free Tier or a learning account is acceptable).
- The account must have permissions to create: **VPC, EC2, RDS, S3, SES, CloudFront, WAF, Route 53, IAM, ACM**.

{{% notice tip %}}
💡 It is recommended to use an **IAM User** rather than the Root Account for deployments. Create an IAM User with **AdministratorAccess** or a custom policy scoped to the required services.
{{% /notice %}}

---

#### 2. Required Tools

Install the following tools on your local machine:

| Tool | Version | Purpose |
|------|---------|---------|
| **Java JDK** | 17+ | Build and run Spring Boot backend |
| **Maven** | 3.8+ | Dependency management and backend build |
| **Node.js** | 18+ | Build React 19 frontend |
| **Yarn** | 1.22+ | Frontend package management |
| **Docker** | 24+ | Package the application into containers |
| **AWS CLI** | v2 | Interact with AWS from the command line |
| **Git** | 2.x | Source code management |

##### Verify Installation

Run the following commands to confirm each tool is installed correctly:

```bash
java --version        # Java 17+
mvn --version         # Maven 3.8+
node --version        # Node.js 18+
yarn --version        # Yarn 1.22+
docker --version      # Docker 24+
aws --version         # AWS CLI 2.x
git --version         # Git 2.x
```

---

#### 3. Configure AWS CLI

Configure the AWS CLI with your IAM User credentials:

```bash
aws configure
```

Enter the following when prompted:
- **AWS Access Key ID** — from IAM Console → Security credentials
- **AWS Secret Access Key** — from IAM Console → Security credentials
- **Default region name** — e.g., `ap-southeast-1` (Singapore)
- **Default output format** — `json`

##### Verify Connectivity

```bash
aws sts get-caller-identity
```

---

#### 4. IAM Permissions

Attach the following **IAM policy** to your IAM User to grant sufficient permissions for deploying Rookwork infrastructure:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:*",
                "rds:*",
                "s3:*",
                "cloudfront:*",
                "wafv2:*",
                "route53:*",
                "ses:*",
                "acm:*",
                "iam:CreateRole",
                "iam:AttachRolePolicy",
                "iam:PassRole",
                "iam:GetRole",
                "iam:CreateInstanceProfile",
                "iam:AddRoleToInstanceProfile",
                "elasticloadbalancing:*",
                "autoscaling:*",
                "ecr:*",
                "logs:*",
                "cloudwatch:*"
            ],
            "Resource": "*"
        }
    ]
}
```

---

#### 5. Prepare Source Code

Clone the Rookwork project source code to your local machine:

```bash
# Backend (Spring Boot)
git clone https://github.com/ChauThanhDat720/rookwork-backend.git

# Frontend (React 19)
git clone https://github.com/ChauThanhDat720/rookwork-frontend.git
```

##### Test Backend Build

```bash
cd rookwork-backend
mvn clean package -DskipTests
```

##### Test Frontend Run

```bash
cd rookwork-frontend
yarn install
yarn build:browser
yarn start:browser
```

---

#### 6. Verify Docker

Ensure Docker is running and can pull images:

```bash
docker info
docker pull amazoncorretto:17
```


---

Once all prerequisites are complete, you are ready to begin deploying the AWS infrastructure for Rookwork. Proceed to [5.3 — Initialize VPC, NAT Gateway and Security Groups](../5.3-S3-vpc/).