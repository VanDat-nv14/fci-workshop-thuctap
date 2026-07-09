---
title: "Event 1"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Reflection on the AI, CloudFront, and Multi-Agent System Event

### Purpose of the Event

- Provide practical perspectives on how to use AI effectively by building the right context.
- Introduce AI tools that support data analysis, workflow automation, and team collaboration.
- Explain the role of Amazon CloudFront in optimizing cost, performance, security, and reliability.
- Share real product-building experience through the UTMorpho case study from LotusHacks.
- Analyze the non-deterministic behavior of LLMs in real-world usage and how to reduce deployment risks.
- Introduce multi-agent systems through an enterprise startup credit scoring use case.

### Event Agenda

The AWS Community Day 2026 event focuses on the latest technology trends in the AWS ecosystem, with a special emphasis on Generative AI (GenAI), Multi-Agent Systems, Network Security (CloudFront), and operational optimization strategies for enterprises.

#### 1. GenAI Applications and Multi-Agent Systems
**Enterprise Credit Scoring System (Vy Lam - VPBank):**
- **Current Situation:** Traditional banking systems often reject startups due to a lack of historical data or collateral.
- **Solution:** Use a Multi-Agent model (Virtual Credit Committee). Instead of a single Agent, the system breaks down into specialized Agents: Financial Analysis, Market Assessment, Team Evaluation, Risk Analysis, and Compliance.
- **Effectiveness:** Shortens processing time from 2-3 weeks to 2-4 hours (95% faster), reduces operational costs by 95%, and doubles the approval rate.

**Business Process Automation (Phạm Ng Hải Anh - G-AsiaPacific):**
- Introduced Amazon Quick Suite, a solution that integrates company data and world knowledge to create AI Agents supporting repetitive tasks such as: automatically creating Minutes of Meeting (MoM), sending emails to stakeholders, and scheduling meetings.

#### 2. Engineering and Optimization in AI Deployment
**The Importance of Context (Tinh Truong - GoTymeX):**
- The main reason AI gives wrong answers is often a lack of Context rather than a weak model.
- **Advice:** Provide selective evidence instead of cramming too much data (Quality > Quantity). Shift from "Prompt" to "Context" and move towards "Memory" (Second AI Brain).

**Non-Determinism of LLMs:**
- Even with Temperature = 0, LLM models can still produce different results due to GPU architecture (floating-point operations) and batching techniques in cloud infrastructure.
- **Solution:** Should use Temperature = 0.1 to avoid infinite loops, run multiple times (Majority voting), and prioritize structured outputs (JSON mode).

#### 3. Infrastructure and Security with Amazon CloudFront (Nguyễn Tuấn Thịnh)
- **New Flat-rate Pricing Mechanism:** AWS launched fixed-price plans (Free, Pro, Business, Premium) for CloudFront, helping enterprises easily forecast costs and avoid "bill shock" when under DDoS attacks or when content goes viral.
- **Performance & Cost Optimization:**
  - CloudFront helps offload CPU for EC2 by handling data compression (reducing size by up to 82%) and TLS handshakes.
  - Utilizes AWS's global Edge network to block DDoS attacks right at the closest point to the source.

#### 4. Real Case Study: UTMorpho (Team VIB)
- **Project:** An AI Agent that allows creating UI from descriptions and enables direct editing on the canvas (WYSIWYG) instead of having to re-prompt multiple times which ruins the design.
- **Lessons Learned:** The most practical ideas come from daily work struggles. Understanding among team members is more important than individual skills when working under time pressure (36-hour hackathon).

### Key Takeaways for the Report
- **Business:** Multi-Agent AI helps save 95% of time and costs for complex processes like credit approval.
- **Engineering:** Context is the key for AI to operate effectively; the non-determinism of LLMs must be noted even at the lowest settings.
- **Infrastructure:** CloudFront is not just a CDN but also a security platform that helps control infrastructure costs effectively with new fixed-price plans.

### What I Learned

#### AI Mindset

- AI does not depend only on prompts; it depends heavily on the context provided.
- To get better AI results, users should clearly define goals, input data, constraints, evaluation criteria, and expected output.
- The Second AI Brain concept shows that AI is moving toward memory, personal workflows, and long-term knowledge systems.
- Students should start learning AI by building small use cases with real data and continuous improvement loops.

#### Cloud And Infrastructure Knowledge

- CloudFront can be considered an important foundation layer between users and the origin.
- CDN is not only for static content, but also supports security, reliability, and cost optimization.
- Moving workloads closer to the edge can improve user experience, especially for globally distributed users.
- Cloud architecture should balance performance, cost, security, and scalability.

#### Product-Building Experience

- Hackathons are useful environments for practicing how to turn ideas into working products quickly.
- Defining the problem clearly is more important than starting to code too early.
- In a short sprint, teams need to prioritize core features, divide tasks effectively, and check progress frequently.
- Failures or direction changes during development can become turning points that make the product more practical.

#### LLM And Multi-Agent Understanding

- LLMs may not be fully deterministic even with fixed-looking settings.
- When deploying AI in production, teams need testing, monitoring, and risk management strategies.
- Multi-agent systems are suitable for complex problems involving many evaluation steps, data sources, and specialized roles.
- Guardrails, compliance, and explainability are essential when applying AI in enterprise environments.

### Application To Study And Work

- Apply clearer contextual prompting when using AI for learning, research, and programming.
- Build small AI workflows for note-taking, document summarization, or personal data analysis.
- Learn more about Amazon CloudFront and how CDN can be integrated into web application architecture.
- Practice building MVPs through short projects or hackathons.
- When using LLMs in projects, design output validation instead of assuming results are always stable.
- Study multi-agent systems further for complex tasks such as financial analysis, risk assessment, or decision support.

### Event Experience

Joining this event was a valuable experience because the content was not limited to theory. It included practical examples of AI, cloud infrastructure, and product development. The sessions helped me understand how AI can be applied in different contexts, from data analysis assistants and automated workflows to enterprise-grade multi-agent systems.

The most impressive message for me was "context is everything." Before the event, I often thought that using AI mainly depended on writing good prompts. After the event, I understood that prompts are only one part of the process. The quality of context, background data, and specific goals are what truly determine whether AI can provide useful support.

The CloudFront session also expanded my view of cloud infrastructure. I realized that a CDN service can play an important role in the overall architecture, from improving page load speed and reducing origin costs to increasing security and reliability.

In addition, the UTMorpho case study from LotusHacks was inspiring because it showed the product-building mindset in action. Building from idea to demo within 36 hours highlighted the importance of teamwork, problem definition, and feature prioritization.

#### Some Photos From The Event

![AWS Community Day 2026](/images/4-EventParticipated/4.1-Event1/event1.jpg)
*AWS Community Day 2026 Event Photo*

![Virtual credit committee architecture](/images/4-EventParticipated/4.1-Event1/virtual-credit-committee.png)
*Multi-Agent architecture in the Virtual Credit Committee system*

![Proposed Deployment Approach](/images/4-EventParticipated/4.1-Event1/deployment-approach.png)
*Deployment approach for the AgentCore system on AWS*

![Why CloudFront is better equipped for volumetric attack](/images/4-EventParticipated/4.1-Event1/cloudfront-ddos.png)
*CloudFront architecture for DDoS mitigation*

![Architecture](/images/4-EventParticipated/4.1-Event1/utmorpho-architecture.png)
*System architecture of UTMorpho*

![The role of Temperature, Top-P, and Top-K](/images/4-EventParticipated/4.1-Event1/llm-temperature.png)
*The impact of the Temperature parameter on LLMs*

> Overall, the event helped me better understand how AI, cloud infrastructure, and product thinking can be combined to solve real-world problems. It also motivated me to continue learning more about AI engineering, cloud architecture, and multi-agent systems.
