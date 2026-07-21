---
title: "Event 1"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Event Reflection – AWS Community Day (May 23rd)

## 1. Purpose and Significance of the Event

The AWS Community Day event (May 23rd) was an opportunity to access the latest technology trends in the AWS ecosystem, particularly in prominent topics such as Generative AI (GenAI), Multi-Agent Systems, Amazon CloudFront, along with product development methodologies and real-world deployment experiences shared by industry practitioners.

Beyond theoretical knowledge, the program also offered practical scenarios, case studies, and problem-solving experiences in a time-constrained product development environment. Through these sessions, attendees gained a clearer understanding of how enterprises leverage AWS technology to build modern, scalable systems that meet real-world demands.

---

## 2. Session Summaries

### Session 1: Build Second Brain (Speaker: Anh Tinh)

This session focused on explaining why AI sometimes gives inaccurate answers. According to the speaker, the issue does not lie in the model's capability but primarily stems from a lack of **Context**.

Instead of cramming as much data as possible into a prompt, users should provide selective, accurate information that is directly relevant to the problem at hand. The current trend in AI usage is shifting from prompt optimization toward building Context and Memory, enabling AI to retain long-term knowledge and respond more accurately.

**💡 Key Insight:** The quality of context is the decisive factor in AI effectiveness. An AI system only truly delivers value when provided with clear goals, background data, and appropriate constraints.

---

### Session 2: Friendly AI Assistant with Amazon Quick (Speaker: Hai Anh)

This session introduced the **Amazon Quick Suite** solution, which allows businesses to combine their internal data with external knowledge sources to build intelligent AI assistants.

The AI Agents created can automate many office tasks such as:
- Writing Meeting Minutes
- Sending notification emails
- Managing work schedules
- Supporting cross-departmental coordination

This significantly reduces manual workload while improving productivity and operational efficiency across the organization.

---

### Session 3: From Edge To Origin – CloudFront as Your Foundation (Speaker: Tuan Thinh)

This session clarified the role of Amazon CloudFront in modern system architecture. CloudFront is not merely a CDN service for faster content delivery — it also serves as the **first line of defense** for any system.

**Flat-rate Pricing Mechanism:**

The speaker introduced pricing tiers — **Free, Pro, Business, and Premium** — to help businesses proactively control costs and minimize the risk of unexpected large bills during DDoS attacks or sudden traffic spikes.

**Technical Benefits:**
- Reduces load on origin servers through data compression
- Handles TLS Handshakes directly at Edge Locations
- Blocks DDoS attacks before malicious traffic reaches the backend

This contributes to improved performance, increased availability, and optimized operational costs.

![Why CloudFront is better equipped for volumetric attack](/images/4-EventParticipated/4.1-Event1/cloudfront-ddos.png)
*CloudFront architecture for DDoS mitigation*

---

### Session 4: 36 Hours with LotusHacks – Building UTMorpho from Idea to Reality (Speaker: Team VIB)

This session recounted the journey of building **UTMorpho** during a 36-hour Hackathon competition.

UTMorpho is an AI Agent capable of converting natural language descriptions into user interfaces (UI) and supporting direct editing via a **WYSIWYG** canvas. This solution significantly reduces the need to continuously rewrite prompts when adjusting designs.

**Lessons from the Product Development Journey:**
- Valuable ideas often originate from daily work pain points.
- In high-pressure environments, team coordination and mutual understanding matter as much as individual technical skills.
- Correctly identifying the problem and focusing on building an **MVP (Minimum Viable Product)** is the decisive factor for project success.

![Architecture](/images/4-EventParticipated/4.1-Event1/utmorpho-architecture.png)
*System architecture of UTMorpho*

---

### Session 5: Deep Dive Talk – How LLM Actually Works? (Speaker: Dao Duc)

This session took a deep dive into the inner workings of **Large Language Models (LLMs)**, with a particular focus on their **non-deterministic** nature.

Even when setting **Temperature = 0**, the model can still produce varying outputs due to:
- Floating-point computation effects on GPU hardware
- **Batching** techniques in cloud infrastructure

**Recommendations for deploying AI in Production:**
- Use **Temperature = 0.1**
- Apply **Majority Voting** techniques
- Prioritize structured outputs (**JSON Mode**)
- Implement **Guardrails** to control output quality and system safety

![The role of Temperature, Top-P, and Top-K](/images/4-EventParticipated/4.1-Event1/llm-temperature.png)
*The impact of the Temperature parameter on LLMs*

---

### Session 6: Enterprise-Grade Multi-Agent System – The Case of Startup Credit Scoring (Speaker: Cat Vy)

The final session presented a **Multi-Agent System** model for startup credit evaluation.

Since startups typically lack credit history and collateral, VPBank built a **Virtual Credit Committee** comprising multiple AI Agents, each handling a specialized role:

| Agent | Responsibility |
|---|---|
| Financial Analyst | Financial data analysis |
| Market Assessor | Market evaluation |
| Team Evaluator | Founding team assessment |
| Risk Manager | Risk governance |
| Compliance Officer | Regulatory compliance checks |

**Results Achieved:**
- Application review time reduced from **2–3 weeks** to **2–4 hours**
- Operational costs reduced by approximately **95%**
- Approval rate **doubled**

Dividing tasks into independent specialized Agents enables parallel processing, improving both accuracy and decision-making efficiency.

![Virtual credit committee architecture](/images/4-EventParticipated/4.1-Event1/virtual-credit-committee.png)
*Multi-Agent architecture in the Virtual Credit Committee system*

![Proposed Deployment Approach](/images/4-EventParticipated/4.1-Event1/deployment-approach.png)
*Deployment approach for the AgentCore system on AWS*

---

## 3. Key Highlights and Lessons Learned

### On AI

Model effectiveness depends heavily on the quality of the provided context. Clearly defining goals, input data, constraints, and expected outputs helps AI deliver more accurate responses. Additionally, since LLMs still exhibit instability, real-world systems need to be designed with testing, monitoring, and result validation mechanisms.

### On Cloud Architecture

Amazon CloudFront not only accelerates access speeds but also plays an important role in performance optimization, cost control, and enhanced system security.

### On Product Development

The Hackathon environment demonstrated the importance of quickly building a Minimum Viable Product (MVP), continuously iterating based on real feedback, rather than pursuing a perfect solution from day one.

---

## 4. Plans to Apply to Study and Work

Following the event, I plan to apply the knowledge gained to my learning and personal development:

1. **Change how I use AI:** Focus on building rich context rather than only optimizing prompts, improving efficiency in learning, research, and programming.

2. **Develop personal AI workflows:** Build workflows for automatic note-taking, document summarization, data analysis support, and personal knowledge management.

3. **Deepen Cloud expertise:** Continue studying Amazon CloudFront and Multi-Agent System architectures to build AI solutions capable of handling complex enterprise problems.

4. **Design LLM control mechanisms:** When integrating LLMs into real projects, prioritize designing cross-validation, monitoring, and result evaluation mechanisms to ensure accuracy, stability, and system safety.

---

### Some Photos From The Event

![AWS Community Day 2026](/images/4-EventParticipated/4.1-Event1/event1.jpg)
*AWS Community Day 2026 Event Photo*

Overall, the event helped me better understand how AI, cloud infrastructure, and product thinking can be combined to solve real-world problems. It also motivated me to continue learning more about AI engineering, cloud architecture, and multi-agent systems in the future.
