---
title: "Static Website Hosting with S3"
menuTitle: "Static Website Hosting with S3"
date: 2026-05-01
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---

Amazon S3 is the ideal hosting solution for static frontend React assets due to its high durability, availability, and low cost.

### Step 1: Create an Amazon S3 Bucket
1. Open the **Amazon S3** service on the AWS Console.
   ![Search and access S3](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-1.png)
2. Click **Create bucket**.
   ![Click Create bucket button](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-2.png)
3. Configure settings:
   * **Bucket name:** Use your domain name (e.g., `rookwork.asia` or `www.rookwork.asia`) for easy tracking.
     ![Set S3 Bucket Name](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-3.png)
   * **AWS Region:** Select your preferred region (e.g., `ap-southeast-1` Singapore).
     ![Select Region](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-4.png)
   * **Block Public Access settings:** Select **Block all public access** (We keep the bucket private and only allow access through CloudFront Origin Access Control to maximize security).
     ![Block All Public Access](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-5.png)
     ![Configure Static Encryption](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-6.png)
4. Click **Create bucket** at the bottom.
   ![Confirm and Create Bucket](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-7.png)
   ![S3 Bucket created successfully](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-8.png)

### Step 2: Upload Frontend Assets to S3
1. Open the bucket you just created.
2. Click **Upload**, then click **Add folder** or **Add files**.
3. Select all folders and files inside your local build directory `dist/` (after building your React project) and select them. (Ensure the `index.html` file is placed directly at the bucket root directory).
4. Click **Upload** to complete the file upload process to S3.

---

After uploading Frontend assets to S3, proceed to **[5.7.3. Secure Delivery via Amazon CloudFront & ACM](../5.7.3-cloudfront-acm/)** to configure HTTPS delivery and CDN.
