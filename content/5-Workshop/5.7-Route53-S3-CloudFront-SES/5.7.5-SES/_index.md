---
title: "Email Integration with Amazon SES"
menuTitle: "Email Integration with Amazon SES"
date: 2026-05-01
weight: 5
chapter: false
pre: " <b> 5.7.5. </b> "
---

Amazon SES is a high-performance, cost-effective cloud email service optimized for sending transactional emails in the Rookwork project.

### Step 1: Verify Domain Identity on Amazon SES
1. Search for **Amazon Simple Email Service (SES)** on the AWS Console.
2. Under the **Configuration** menu, click **Identities**, then click **Create identity**.
   ![Access SES and click Create identity](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/SES-1.png)
3. Select **Domain** (Recommended when you own a custom domain):
   * **Domain:** Enter your custom domain name (e.g., `rookwork.asia`).
   * **Easy DKIM:** Enable and leave default settings to authenticate your identity reliably.
4. Click **Create identity**.
   ![Enter Domain Name and Configure DKIM](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/SES-2.png)
5. The system provides CNAME records for DKIM setup. Click **Create records in Route 53** to automatically add these DNS records to your Hosted Zone.
   ![Click Create records in Route 53](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/SES-3.png)
6. *(Note)* By default, new SES accounts are placed in the **Sandbox** environment. You must verify a separate **Email address** identity (such as your personal email) to act as a recipient for testing emails during development.
   ![Verify recipient email address in SES Sandbox](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/SES-4.png)

### Step 2: Create SMTP Credentials for Spring Boot
1. On the left menu of SES, select **SMTP settings**.
2. Click **Create SMTP credentials**.
3. Name your IAM user and click Create.
4. Copy and store the generated **SMTP Username** and **SMTP Password** securely.

### Step 3: Configure Spring Boot Backend for Email Delivery
In the Rookwork backend application (`rookwork-backend-sb`), configure secure SMTP connection parameters using system environment variables (`.env` file):

```properties
spring.mail.host=email-smtp.ap-southeast-1.amazonaws.com
spring.mail.port=587
spring.mail.username=${SMTP_USERNAME}
spring.mail.password=${SMTP_PASSWORD}
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

Use the `JavaMailSender` class to write an email service to deliver verification OTPs for registration, password recovery, and invitations to join collaborative workspaces.

![JavaMailSender implementation code for sending OTP emails](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/SES-code.png)

---

After completing all AWS Core services integration, proceed to **[5.8. Deploy Frontend CI/CD with GitHub Actions](../../5.8-cicd-frontend/)** to complete the frontend deployment automation.
