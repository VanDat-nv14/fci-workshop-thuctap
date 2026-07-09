---
title: "Blog 1"
date: 2026-06-30
weight: 1
chapter: false
pre: "<b> 3.1. </b>"
---

# Scaling telco automation to millions of devices with Managed Red Hat Ansible Automation Platform (AAP) on AWS and Red Hat OpenShift Service on AWS (ROSA)

Hello everyone in the AWS Study Group VN community.

For those working in the telecommunications sector or managing massive fleets of end-user devices (such as home routers, modems, or broadband gateways) numbering in the millions, performing configuration updates or bulk firmware upgrades without causing system congestion is always a challenging operational bottleneck.

Today, I would like to share an architecture summarized and detailed from the AWS Architecture Blog. It addresses this massive scale automation challenge by combining **Managed Red Hat Ansible Automation Platform (AAP) on AWS** and **Red Hat OpenShift Service on AWS (ROSA)**.

### Core Concept: Complete Decoupling of Control Plane and Execution Plane

Traditional management systems often bundle the control plane (UI, APIs) and the execution plane (running automation jobs) into the same infrastructure. When you trigger automation across hundreds of thousands of devices simultaneously, process spikes consume all available resources, causing the operator's control panel to freeze.

With this modern architecture:

*   **Control Plane:** AAP on AWS acts as the central orchestrator (handling UI, APIs, and coordination). It dynamically synchronizes device lists (Inventories) from external systems like CMDBs or Cloud Providers, and fetches playbooks from centralized repositories like GitHub.
*   **Execution Plane:** Running heavy Ansible playbooks is delegated entirely to containerized Pods (specifically Container Groups in Kubernetes) running on the ROSA cluster.

During workload spikes, the execution plane can scale dynamically to communicate with device gateways, while the control plane remains responsive and smooth.

### High-Availability Infrastructure: Multi-Region & Stateless Design with Amazon S3

To support millions of endpoints nationwide, the system is deployed across multiple AWS Regions (such as us-east-1, us-west-1, us-west-2). In each region, worker nodes are distributed across three independent Availability Zones (AZ-A, AZ-B, and AZ-C) to ensure high availability. A failure in one zone will not impact the overall service.

Furthermore, to keep the AAP cluster lightweight and stateless, heavy data storage requirements (like Ansible Content Collections and Automation Execution Environment Container Images) are offloaded to Amazon S3 Buckets.

### Real-Time Auto-Scaling and Cost Optimization

This is the most cost-effective aspect of the solution. Instead of maintaining a massive server cluster all year round just to handle occasional maintenance cycles, the combination of ROSA and AWS allows for pay-as-you-go elastic scaling.

Consider a real-world timeline of a scheduled firmware upgrade:

*   **09:00 AM (Idle State):** The system runs only 3 EC2 Worker Nodes for minor monitoring, keeping costs at a bare minimum.
*   **09:01 AM (Maintenance Peak):** A "Firmware Tuesday" update is triggered simultaneously for 1,000 home routers.
*   **09:02 AM (Process Spike):** To handle parallel connections, AAP requests the creation of 200 execution Pods on Kubernetes.
*   **09:05 AM (Elastic Provisioning):** Since the initial worker nodes are fully utilized, new Pods enter a Pending state. The ROSA Cluster Autoscaler immediately triggers AWS to provision additional EC2 instances. Jobs are processed smoothly.
*   **09:30 AM (Completion):** The firmware upgrade completes successfully, and the 200 execution Pods are automatically terminated.
*   **09:45 AM (Scale-in):** ROSA detects that the newly provisioned EC2 instances are idle. The Metrics Server scales down the infrastructure back to the baseline of 3 EC2 instances.

Consequently, the enterprise only pays for the extra compute resources utilized during that short window of peak activity.

### Offloading Logs to OLAP for AI Training (Operations LLM)

Running automation across millions of devices generates a massive volume of logs. To avoid I/O bottlenecks, this architecture avoids traditional file logging. Instead, execution logs and metrics from AAP are continuously streamed to a distributed OLAP (Online Analytical Processing) database designed for real-time analysis (e.g., Grafana / Grafana Loki).

In addition to providing dashboards for debugging, this clean, high-volume log dataset serves as training data for Operations LLMs. In the future, AI models can learn from these logs to automatically diagnose and apply self-healing solutions to the telecom network.

### Additional Advantages

*   **No Dependency Conflicts:** Each automation script is packaged into an isolated Container Image (Execution Environment), preventing dependency conflicts across different hardware versions.
*   **Reduced Overhead:** Since both AAP and ROSA are fully managed services, patching operating systems, configuring network layers, and maintaining infrastructure are handled by Red Hat and AWS. Operators can focus purely on writing playbooks.
*   **Cost Control Guardrails (Fences):**
    *   **Kubernetes Resource Quotas:** Hard limits on the maximum CPU/Memory AAP can consume within its namespace.
    *   **AWS Service Quotas:** Spending limits placed on the maximum number of EC2 instances allowed to launch, preventing runaway loops from escalating the AWS bill.

---

**Original Source:** [AWS IBM-RedHat Blog](https://aws.amazon.com/vi/blogs/ibm-redhat/scaling-telco-automation-to-millions-of-devices-with-managed-red-hat-ansible-automation-platform-aap-on-aws-and-red-hat-openshift-service-on-aws-rosa/)

**Translated Post:** [AWS Study Group VN | Facebook](https://www.facebook.com/share/p/1Uaej3AE3d/)

**Illustrations:**

![Telco Automation Architecture Diagram](/images/3-BlogsTranslated/blog1.0.png)
![Auto-scaling Timeline Diagram](/images/3-BlogsTranslated/blog1.1.png)