# Project 4 — AWS Route 53 Domain Routing

## Overview
This project demonstrates domain routing and DNS configuration using AWS Route 53. The goal is to route a custom domain to AWS resources (for example: an S3 static website, an EC2 behind a Load Balancer, or a CloudFront distribution) and to correctly configure DNS records, hosted zones, and optional health checks and routing policies.

---

## Objectives
- Create a Hosted Zone in AWS Route 53.
- Configure DNS records (A, CNAME, Alias) to route a domain or subdomains to AWS resources.
- (Optional) Configure health checks and failover or other routing policies.
- Validate that traffic to the domain is routed correctly to the target backend (S3, ELB, CloudFront, or EC2).

---

## Technologies / Services
- AWS Route 53 (Hosted Zones, Record Sets, Health Checks, Routing Policies)
- Amazon S3 (for static site hosting, optional)
- Elastic Load Balancer (ALB/NLB) and EC2 (optional)
- Amazon CloudFront (optional)
- Domain registrar or Route 53 domain registration
- AWS Console and/or AWS CLI

---

## Prerequisites
- Active AWS account with permissions for Route 53 and any target services (S3, EC2, ELB, CloudFront).
- A registered domain (or willingness to register one via Route 53).
- Basic familiarity with AWS Console or AWS CLI.
- Tools like dig or nslookup for DNS verification (optional).

---

## High-level Steps
1. Create a Hosted Zone
   - AWS Console → Route 53 → Create Hosted Zone → add your domain (e.g., ayushkumar2533.fun).
2. Create DNS Records
   - Create an Alias A record for the root domain pointing to S3 website endpoint / ALB / CloudFront.
   - Create a CNAME (or Alias) for subdomains like www.example.com to point to the distribution or root domain.
3. (Optional) Configure Health Checks and Routing Policies
   - Add health checks for primary/secondary endpoints and configure failover, weighted, or latency-based routing as needed.
4. Update Registrar (if domain registered elsewhere)
   - At your domain registrar, replace the current name servers with the NS records provided by Route 53 (delegate the domain).
5. Test and Verify
   - Use dig/nslookup to confirm DNS records and access the domain in a browser.
6. Wait for DNS propagation (changes may take minutes to hours, depending on TTL).

---

## Testing
- CLI:
  - dig ayushkumar2533.fun +short
  - nslookup ayushkumar2533.fun
- Browser:
  - Open your domain and confirm the website loads from the expected backend.
- Route 53 Console:
  - Check health check statuses and routing policy behavior if configured.

---

## Expected Results
- The domain should resolve to the intended AWS resource once DNS is configured and propagated.
- If failover or routing policies are set, traffic should follow the configured policy (e.g., failover to secondary when primary is unhealthy).

---

## Author
Ayushkumar2533

---

