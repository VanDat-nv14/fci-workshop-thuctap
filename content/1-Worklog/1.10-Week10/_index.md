---
title: "Week 10 Worklog"
date: 2026-06-21
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Research the concepts and operational mechanisms of Auto Scaling on the AWS Cloud platform.
* Configure a Launch Template for Backend EC2 instances.
* Initialize an Auto Scaling Group (ASG) to automatically manage the running EC2 instances.
* Establish scaling policies based on CPU utilization metrics.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Research Auto Scaling concepts (Horizontal vs. Vertical Scaling) and core components (Launch Templates, Target Groups). | 22/06/2026 | 23/06/2026 | <https://000006.awsstudygroup.com/> |
| 3 | - Configure a detailed Launch Template for EC2 Backend instances, including AMI selection, Instance Type, Key Pair, and Security Groups. | 23/06/2026 | 24/06/2026 | <https://000029.awsstudygroup.com/> |
| 4 | - Create an Auto Scaling Group (ASG) integrated within the VPC, deploying across two Availability Zones in Private Subnets. | 24/06/2026 | 25/06/2026 | <https://000030.awsstudygroup.com/> |
| 5 | - Configure a Target Tracking Scaling Policy monitoring average CPU utilization exceeding 70% to trigger scale-out activities. | 25/06/2026 | 25/06/2026 | <https://000036.awsstudygroup.com/> |
| 6 | - Perform initial verification of the automatic instance provisioning by the ASG and its integration with the Application Load Balancer (ALB). | 26/06/2026 | 26/06/2026 |  |

### Week 10 Achievements:

* Mastered the principles of Auto Scaling and resource scaling mechanisms on the AWS Cloud.
* Successfully created a standard Launch Template containing configuration and Docker setup parameters for the Backend.
* Configured an active Multi-AZ Auto Scaling Group (ASG) within the Private Subnets.
* Set up a Target Tracking scaling policy based on CPU utilization metrics to dynamically handle traffic spikes.
* Verified the automatic registration of new EC2 instances into the Application Load Balancer's Target Group.
