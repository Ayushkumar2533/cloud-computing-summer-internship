# Project 1: S3 Static Website Hosting

## Overview

This project demonstrates hosting a production-ready static website using AWS S3 and complementary AWS services. It covers site deployment, secure delivery via CloudFront/ACM, DNS mapping with Route 53, logging and monitoring, and security best practices for static content hosting.

The goal is to provide an end-to-end, repeatable pattern for serving static websites with high availability, low latency, cost-effectiveness, and secure HTTPS delivery.

---

## Objectives

- Host a static website on Amazon S3 with high durability and availability.
- Serve content securely over HTTPS using Amazon CloudFront and AWS Certificate Manager (ACM).
- Configure DNS with Amazon Route 53 for a custom domain.
- Implement secure bucket access patterns (prefer CloudFront + OAI over public buckets).
- Enable logging, versioning and basic monitoring for auditability and troubleshooting.
- Document deployment and teardown steps, CLI examples, and validation checks.

---

## Architecture (high-level)

- S3 (Origin): Stores website assets (HTML, CSS, JS, images).
- CloudFront: CDN in front of S3 for caching, HTTPS termination, and low-latency delivery.
- ACM: Manages TLS certificate in the edge region for HTTPS.
- Route 53: DNS records (A/ALIAS) mapping custom domain to CloudFront distribution.
- (Optional) WAF: Web Application Firewall for additional protection.
- Logging: S3 server access logs and CloudFront access logs for analytics and security.

---

## AWS Services Used

- Amazon S3 (Static website hosting; object storage)
- Amazon CloudFront (CDN)
- AWS Certificate Manager (ACM) (TLS certificates)
- Amazon Route 53 (DNS)
- AWS IAM (access control)
- (Optional) AWS WAF, AWS CloudWatch, CloudTrail

---

## Deliverables

- Working static website accessible at your-custom-domain.com (or S3 website endpoint).
- Documented deployment steps, CLI commands, and configuration snippets.
- Considerations for security, cost, and performance.
- Cleanup instructions to remove resources and avoid charges.

---

## Prerequisites

- AWS account with sufficient permissions to create S3 buckets, CloudFront distributions, ACM certificates, and Route 53 records.
- AWS CLI installed and configured with a user that has appropriate IAM permissions.
- Local website build directory containing `index.html` (and other assets).
- Custom domain configured in Route 53 if using a custom domain.

---

## Quick Deployment Guide (recommended production pattern)

1. Create the S3 bucket (name should match the site domain if desired):
   - Example:
     - aws s3 mb s3://example.com --region us-east-1

2. Upload site content:
   - aws s3 sync ./site s3://example.com --delete
   - For development only (NOT recommended for production behind CloudFront): aws s3 sync ./site s3://example.com --acl public-read

3. (Recommended) Keep bucket objects private and use CloudFront OAI
   - Create CloudFront distribution with the S3 bucket as origin.
   - Configure an Origin Access (Origin Access Identity, or Origin Access Control) to restrict direct S3 access.
   - Configure S3 bucket policy to grant read access only to the CloudFront origin identity.

4. Request or import ACM certificate (region us-east-1 for CloudFront):
   - Request certificate:
     - aws acm request-certificate --domain-name example.com --subject-alternative-names www.example.com --validation-method DNS
   - Validate the certificate via DNS (Route 53) or email.

5. Create CloudFront distribution:
   - Set origin to the S3 bucket (use the REST/virtual-hosted endpoint).
   - Set Viewer Protocol Policy to Redirect HTTP to HTTPS or HTTPS Only.
   - Attach the ACM certificate (us-east-1).
   - Enable logging (optional) and set appropriate TTLs, behaviors, and caching.

6. Create Route 53 record(s):
   - Create an A (ALIAS) record for your domain pointing to the CloudFront distribution domain name.

7. Invalidate CloudFront cache (when updating content):
   - aws cloudfront create-invalidation --distribution-id <DIST_ID> --paths "/*"

---

## Example S3 Bucket Policy (CloudFront OAI pattern)

Use the CloudFront origin identity in place of `<CLOUDFRONT_OAI_CANONICAL_USER_ID>`:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowCloudFrontServicePrincipalReadOnly",
      "Effect": "Allow",
      "Principal": {
        "CanonicalUser": "<CLOUDFRONT_OAI_CANONICAL_USER_ID>"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example.com/*"
    }
  ]
}
