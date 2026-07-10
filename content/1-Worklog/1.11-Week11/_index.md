---
title: "Week 11 Worklog"
date: 2026-06-28
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

* Research performance testing tools and script load tests for the system.
* Perform load testing to evaluate the actual scaling capabilities of the Auto Scaling Group (ASG) under heavy stress.
* Monitor scaling activities and optimize cooldown periods and scale-in rules.
* Document the Auto Scaling configuration and compile the load testing analysis report.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Research load testing tools (such as Apache JMeter, Locust, Artillery) to simulate large volumes of concurrent traffic. | 29/06/2026 | 30/06/2026 | <https://000006.awsstudygroup.com/> |
| 3 | - Configure a detailed load testing script, sending concurrent HTTP requests to the Application Load Balancer to spike CPU usage. | 30/06/2026 | 01/07/2026 | N/A |
| 4 | - Run the load testing scenarios, monitoring CloudWatch metrics to verify the ASG scales out (provisions new instances). | 01/07/2026 | 02/07/2026 | N/A |
| 5 | - Monitor the scale-in process when traffic drops, making configurations to optimize the Cooldown Period. | 02/07/2026 | 03/07/2026 | N/A |
| 6 | - Author technical documentation describing Auto Scaling setups and compiling load test analysis results. | 03/07/2026 | 03/07/2026 | <https://000002.awsstudygroup.com/> |

### Week 11 Achievements:

* Selected and mastered a suitable load testing tool to simulate user traffic patterns.
* Successfully set up a secure and effective load testing script targeting the application's API endpoints.
* Executed the load tests successfully: Verified that the Auto Scaling Group provisions new EC2 instances when average CPU usage exceeds thresholds.
* Optimized the cooldown duration and scale-in rules to reduce resource waste and cost when traffic decreases.
* Completed a comprehensive technical report documenting the Auto Scaling configuration and system load test outcomes.
