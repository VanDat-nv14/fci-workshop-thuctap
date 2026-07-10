---
title: "AWS Services Integration"
menuTitle: "AWS Services Integration"
date: 2026-05-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

In this section, we configure the core AWS infrastructure services required for our application, including domain resolution (Route 53), S3 static hosting, secure HTTPS delivery (CloudFront + ACM), private file storage (S3 Storage), and email notification services (SES).

### Content

* **[5.7.1. Domain & AWS Route 53 Configuration](5.7.1-Route53/)** - Guide on delegating domain resolution from Tenten.vn to AWS Route 53.
* **[5.7.2. Static Website Hosting with S3](5.7.2-S3-Static-Hosting/)** - Guide on creating an S3 Bucket and uploading React Frontend assets.
* **[5.7.3. Secure Delivery via Amazon CloudFront & ACM](5.7.3-CloudFront-ACM/)** - Guide on requesting ACM SSL certificates, setting up CloudFront CDN, and mapping Route 53 records to CDN.
* **[5.7.4. Private File Storage with S3](5.7.4-S3-Storage/)** - Guide on configuring a private S3 Bucket, Spring Boot Backend, and S3 Service in Java.
* **[5.7.5. Email Integration with Amazon SES](5.7.5-SES/)** - Guide on verifying domain identity on AWS SES, creating SMTP credentials, and sending emails.
