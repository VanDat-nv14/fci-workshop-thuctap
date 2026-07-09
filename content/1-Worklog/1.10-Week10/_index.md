---
title: "Week 10 Worklog"
date: 2026-06-21
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

* Monitor and optimize the performance of the Rookwork system after its Production deployment.
* Set up automated alerting with Amazon CloudWatch to proactively detect resource and service issues.
* Review and strengthen infrastructure security following Least Privilege and Defense-in-Depth principles.
* Begin writing technical documentation and preparing content for the internship graduation report.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Set up an Amazon CloudWatch Dashboard to centrally monitor key performance metrics (CPU, Memory, Network) of EC2 and RDS in real time. | 22/06/2026 | 23/06/2026 | <https://000029.awsstudygroup.com/> |
| 3 | - Configure CloudWatch Alarms and Amazon SNS to automatically send email alerts when EC2 CPU exceeds 80% or RDS connections are disrupted. | 23/06/2026 | 24/06/2026 | <https://000036.awsstudygroup.com/> |
| 4 | - Review all IAM Policies and Security Groups, remove excessive access permissions following the Least Privilege principle to reduce the attack surface. | 24/06/2026 | 25/06/2026 | <https://000030.awsstudygroup.com/> |
| 5 | - Optimize infrastructure operating costs using AWS Cost Explorer and AWS Trusted Advisor, identify unused resources and propose adjustments. | 25/06/2026 | 25/06/2026 | <https://000034.awsstudygroup.com/> |
| 6 | - Write technical documentation describing the Rookwork system architecture, design decisions, and operational guidelines; begin drafting internship report content. | 26/06/2026 | 26/06/2026 | N/A |

### Week 10 Achievements:

* Successfully set up a CloudWatch Dashboard providing centralized and visual monitoring of key metrics across the entire Rookwork infrastructure: EC2 CPU Utilization, Network In/Out, RDS database connection count, and CloudFront Cache Hit ratio.
* Configured CloudWatch Alarms integrated with Amazon SNS to automatically send email notifications when performance anomalies or service disruptions are detected.
* Completed a full review and refinement of all Security Groups to minimize open rules: removed unnecessary inbound rules and ensured RDS only accepts internal connections from EC2.
* Reviewed IAM Roles and IAM Policies used by GitHub Actions, restricting permissions to only what is needed for deployment tasks (S3 sync, CloudFront Invalidation, no bucket deletion permission).
* Analyzed AWS Cost Explorer reports to identify cost optimization opportunities and planned cleanup of unused test resources.
* Completed a draft of the technical documentation describing infrastructure architecture, CI/CD pipeline, and basic operational guidelines for the Rookwork system; began drafting Worklog and Proposal content for the internship graduation report.
