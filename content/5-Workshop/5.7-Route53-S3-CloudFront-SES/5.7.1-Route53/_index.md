---
title: "Domain & AWS Route 53 Configuration"
menuTitle: "Domain & AWS Route 53 Configuration"
date: 2026-05-01
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

To delegate DNS management of your custom domain purchased from Tenten.vn to AWS Route 53, perform the following delegation steps.

### Step 1: Create a Hosted Zone in Route 53
1. Log in to the **AWS Management Console** and navigate to the **Route 53** service.
2. Select **Hosted zones** from the left panel, and click **Create hosted zone**.
   ![Search and access Hosted Zones](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-1.png)
   ![Click Create hosted zone button](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-2.png)
3. Enter details:
   * **Domain name:** Enter the exact domain name you purchased (e.g., `rookwork.asia`).
   * **Type:** Select **Public hosted zone**.
   ![Configure domain name for Hosted Zone](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-3.png)
4. Click **Create hosted zone**. The system automatically generates 2 default records: **NS** (Name Server) and **SOA** (Start of Authority).
   ![Hosted Zone created successfully](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-4.png)

### Step 2: Retrieve Route 53 Name Servers
1. Click on the Hosted Zone you just created.
2. Locate the record with type **NS**.
3. Copy the **4 Name Server addresses** listed in the Value column (e.g., `ns-xxxx.awsdns-xx.com.`, note to omit the trailing period when configuring on Tenten).

### Step 3: Configure Name Servers on Tenten.vn
1. Log in to your customer portal on **Tenten.vn**.
2. Navigate to your domain list, and select **Configure Name Server (NS)** for the corresponding domain name.
   ![Tenten domain list](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/TenTen-1.png)
   ![Select Name Server configuration](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/TenTen-2.png)
3. Replace Tenten's default name servers with the **4 NS addresses** copied from Route 53 in Step 2.
   ![Enter 4 NS addresses from Route 53](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/TenTen-3.png)
4. Save the configuration.
   ![NS configuration saved successfully](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/TenTen-4.png)

*(Note: Global DNS propagation can take anywhere from a few minutes up to 24 hours to fully update).*

---

Once DNS delegation is configured, proceed to **[5.7.2. Static Website Hosting with S3](../5.7.2-s3-static-hosting/)** to prepare hosting for your Frontend assets.
