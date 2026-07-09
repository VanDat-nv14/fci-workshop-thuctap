---
title: "Week 8 Worklog"
date: 2026-06-07
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---


### Week 8 Objectives:

* Configure Application Load Balancer and Auto Scaling Group to enable automatic system scaling.
* Set up automatic backup for RDS and EBS.
* Learn and practice CI/CD workflows on AWS.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Learn and configure Application Load Balancer (ALB) to distribute incoming traffic. | 08/06/2026 | 09/06/2026 | <https://000006.awsstudygroup.com/> |
| 3 | - Configure Auto Scaling Group (ASG) for EC2 to automatically scale up or down based on actual user load and optimize costs. | 09/06/2026 | 10/06/2026 | <https://000006.awsstudygroup.com/> |
| 4 | - Set up automated backup (Snapshot/Backup) for Amazon RDS and EBS volumes, and test system Fault Tolerance. | 10/06/2026 | 11/06/2026 | <https://000013.awsstudygroup.com/> |
| 5 | - Learn about the CI/CD process on the AWS cloud platform. | 11/06/2026 | 11/06/2026 | <https://000152.awsstudygroup.com/> |
| 6 | - Configure AWS CodePipeline and AWS CodeDeploy (or GitHub Actions) to automatically deploy new code from GitHub to EC2. | 12/06/2026 | 12/06/2026 | <https://000017.awsstudygroup.com/> |

### Week 8 Achievements:

* Understood the role of Application Load Balancer (ALB) in distributing traffic across multiple EC2 instances.
* Configured ALB with a Target Group and Health Check to ensure traffic is only routed to healthy instances.
* Set up an Auto Scaling Group with a Launch Template, configuring appropriate min/max/desired capacity values.
* Configured a CPU-based scaling policy so the system can automatically scale-out and scale-in based on actual load.
* Enabled Automated Backup and created manual Snapshots for Amazon RDS.
* Created Snapshots for EBS volumes to allow data recovery when needed.
* Tested Fault Tolerance by stopping one EC2 instance and observing ALB automatically redirecting traffic to the remaining instance.
* Gained an overview of the CI/CD process and the benefits of automating deployments on AWS.
* Practiced configuring AWS CodePipeline or GitHub Actions to automatically deploy new code to EC2 on a GitHub push.
