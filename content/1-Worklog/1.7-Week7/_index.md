---
title: "Week 7 Worklog"
date: 2026-05-31
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Objectives:

* Analyze infrastructure requirements and design a practical VPC network architecture specifically for the group project "rookwork".
* Build secure network partitions (Public/Private Subnets), deploy NAT Gateway, and configure Bastion Host for the project.
* Set up the EC2 application server environment in the private network and configure a secure connection to the RDS database.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Analyze infrastructure requirements and design a detailed VPC architecture for the group project "rookwork". | 01/06/2026 | 02/06/2026 | <https://000003.awsstudygroup.com/> |
| 3 | - Set up network subnets (Public/Private) and NAT Gateway to ensure secure internet connectivity for internal servers. | 02/06/2026 | 03/06/2026 | <https://000003.awsstudygroup.com/> |
| 4 | - Install and configure a Bastion Host in the Public Subnet as the sole administration gateway for the team's internal EC2 instances. | 03/06/2026 | 04/06/2026 | <https://000092.awsstudygroup.com/> |
| 5 | - Design and configure detailed multi-tier network permissions (Security Groups) between Bastion, Backend EC2, and RDS Database for the project. | 04/06/2026 | 05/06/2026 | <https://000003.awsstudygroup.com/> |
| 6 | - Configure the runtime environment for the backend application on EC2 in the Private Subnet and set up a secure connection to RDS. | 05/06/2026 | 05/06/2026 | <https://000005.awsstudygroup.com/> |

### Week 7 Achievements:

* Successfully analyzed and deployed a specialized VPC architecture optimized for the operational design of the group project "rookwork".
* Properly allocated IP blocks for Public and Private Subnets, configuring Route Tables and NAT Gateway to enable secure internet access for application servers.
* Deployed a Bastion Host in the Public Subnet as a jump server, enabling the entire team to securely manage internal EC2 instances via SSH.
* Completed the setup of strict multi-tier Security Groups for the group project (Bastion -> Backend EC2 -> RDS Database), minimizing the attack surface.
* Resolved internal network connection issues (SSH timeout) by reviewing and reconfiguring Security Groups and Route Tables.
* Successfully installed the runtime environment for the backend application on an EC2 instance residing entirely within the project's Private Subnet.
* Established a secure connection from Backend EC2 to Amazon RDS within the VPC internal network, ensuring database data is not exposed to the public internet.
