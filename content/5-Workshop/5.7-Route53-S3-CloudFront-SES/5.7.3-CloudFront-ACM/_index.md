---
title: "Secure Delivery via Amazon CloudFront & ACM"
menuTitle: "Secure Delivery via Amazon CloudFront & ACM"
date: 2026-05-01
weight: 3
chapter: false
pre: " <b> 5.7.3. </b> "
---

To enable secure HTTPS delivery for the website and optimize page loading globally, configure AWS Certificate Manager (ACM) combined with CloudFront.

### Step 1: Request SSL/TLS Certificate via ACM
1. Search for **Certificate Manager** on the AWS Console.
2. *(Important)* Switch region to **us-east-1 (N. Virginia)** (CloudFront requires SSL/TLS certificates to be provisioned in this region to be used).
   ![Change ACM Region to us-east-1](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/ACM-1.png)
3. Click **Request certificate**, select **Request a public certificate**, and click Next.
   ![Request Public SSL Certificate](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/ACM-2.png)
4. In the configuration:
   * **Fully qualified domain name:** Enter your domain name (e.g., `rookwork.asia` and add an additional wildcard domain `*.rookwork.asia`).
   * **Validation method:** Choose **DNS validation**.
5. Click **Request**.
   ![Enter Domain Name and Choose DNS Validation](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/ACM-3.png)
6. The initial status will be *Pending validation*. Click the Certificate ID, and click **Create records in Route 53** to automatically generate DNS validation records in Route 53.
   ![Create DNS validation records in Route 53](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/ACM-4.png)
7. Wait a few minutes for the status to change to **Issued**.
   ![ACM Certificate issued successfully](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/ACM-5.png)

### Step 2: Create a CloudFront Distribution
1. Search for **CloudFront** on the AWS Console.
2. Click **Create distribution**.
   ![Access CloudFront and click Create distribution](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-1.png)
3. In the configuration:
   * **Origin domain:** Select your S3 bucket.
     ![Select S3 Bucket as Origin](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-2.png)
   * **Origin access:** Select **Origin access control settings (OAC)** (recommended). Click **Create control setting**, leave default settings, and save.
     ![Configure Origin Access Control (OAC)](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-3.png)
   * **Viewer protocol policy:** Select **Redirect HTTP to HTTPS**.
     ![Choose Redirect HTTP to HTTPS](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-4.png)
   * **Alternate domain name (CNAME):** Add your domain names (e.g., `rookwork.asia` and `www.rookwork.asia`).
   * **Custom SSL certificate:** Select the ACM certificate generated in Step 1.
     ![Add CNAME and ACM SSL Certificate](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-5.png)
   * **Default root object:** Enter `index.html`.
     ![Set Default Root Object to index.html](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-6.png)
4. Click **Create distribution**.
   ![CloudFront Distribution created successfully](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Cloudfront-7.png)
5. Copy the suggested **Bucket Policy** JSON code from the CloudFront console, and paste it into the **Permissions -> Bucket Policy** tab of your S3 Bucket to grant CloudFront access to S3 files.
   ![Open Permissions tab in S3 Bucket to Edit Policy](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-policy-1.png)
   ![Click Edit S3 Bucket Policy](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-policy-2.png)
   ![Paste Policy JSON copied from CloudFront](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-policy-3.png)
   ![Click Save changes to save the policy](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-policy-4.png)
   ![Bucket Policy updated successfully](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/S3-policy-5.png)

### Step 3: Create DNS Record in Route 53 Pointing to CloudFront
1. Return to the **Route 53** service and open the Hosted Zone for your domain.
2. Click **Create record**.
   ![Open Hosted Zone and click Create record](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-CF-1.png)
3. Configure the record:
   * **Record name:** Leave blank (for the apex domain `rookwork.asia`).
   * **Record type:** Choose **A - Routes traffic to an IPv4 address and some AWS resources**.
   * **Alias:** Enable (toggle switch to the right).
   * **Route traffic to:** Choose **Alias to CloudFront distribution** -> Select the CloudFront distribution you created.
   ![Configure A Alias Record pointing to CloudFront](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-CF-2.png)
4. Click **Create records**.
   ![A DNS record created successfully by Route 53](/images/5-Workshop/5.7-Route53-S3-CloudFront-SES/Route53-CF-3.png)

You can now access your website securely using HTTPS through your custom domain name!

---

Once the static website is live and secure via HTTPS, proceed to **[5.7.4. Private File Storage with S3](../5.7.4-s3-storage/)** to configure private storage for the Backend application.
