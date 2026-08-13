# Project 6 — Application Load Balancer & Auto Scaling

A hands-on project demonstrating how to design and deploy a scalable, highly available web application on AWS using an Application Load Balancer (ALB) and Auto Scaling Groups (ASG). This project covers load balancing, health checks, auto-scaling policies, instance provisioning, monitoring and cleanup.

---

## Table of contents
- [Overview](#overview)
- [Goals](#goals)
- [Architecture](#architecture)
- [AWS Services Used](#aws-services-used)
- [Prerequisites](#prerequisites)
- [Implementation / Setup (high level)](#implementation--setup-high-level)
- [Testing & Validation](#testing--validation)
- [Cleanup & Cost Considerations](#cleanup--cost-considerations)
- [Troubleshooting & Tips](#troubleshooting--tips)
- [Learning Outcomes](#learning-outcomes)
- [Author](#author)
- [License](#license)

---

## Overview
This project builds an auto-scaled web application environment that automatically distributes incoming traffic across healthy EC2 instances and adjusts capacity in response to demand. The ALB routes traffic, performs health checks, and integrates with the Auto Scaling Group to maintain desired capacity and reliability.

## Goals
- Deploy a simple web application behind an Application Load Balancer.
- Configure Auto Scaling to add/remove instances based on CPU or request metrics.
- Implement health checks and target groups for graceful traffic routing.
- Monitor performance with CloudWatch and apply scaling policies.
- Demonstrate safe teardown and cost-aware operation.

## Architecture
- AWS VPC with public subnets for Load Balancer and private/public subnets for EC2 (as designed).
- Application Load Balancer (ALB) with listener on HTTP/HTTPS.
- Target Group(s) for EC2 instances (registered by ASG).
- Auto Scaling Group (ASG) backed by a Launch Template or Launch Configuration.
- Security Groups: ALB (HTTP/HTTPS inbound), EC2 (allow traffic from ALB), SSH restricted to admin IP.
- CloudWatch for alarms and metrics; optional SNS for notifications.

Simple ASCII diagram:
ALB (Listener) --> Target Group --> Auto Scaling Group --> EC2 instances (web app)
                       ^                                         |
                       |-----------------------------------------|

## AWS Services Used
- Amazon EC2
- Elastic Load Balancing (Application Load Balancer)
- Auto Scaling Groups (Launch Template/Configuration)
- Amazon VPC, Subnets, Route Tables
- Security Groups & IAM (roles for EC2)
- Amazon CloudWatch (metrics & alarms)
- (Optional) Amazon SNS for notifications
- (Optional) SSM Session Manager for secure instance access

## Prerequisites
- AWS account with appropriate permissions (EC2, ELB, IAM, CloudWatch, AutoScaling, VPC).
- AWS CLI configured or access to AWS Console.
- Basic knowledge of EC2 instances and Linux.
- (Optional) Terraform or CloudFormation if you prefer IaC.

## Implementation / Setup (high level)
1. Create or reuse a VPC and subnets (ensure at least two AZs for high availability).
2. Create a Launch Template or Launch Configuration:
   - AMI with your web application (or user-data to install app on boot).
   - Instance type, IAM role, security group.
3. Create an Application Load Balancer:
   - Configure listeners (HTTP/HTTPS) and security group.
   - Create a Target Group (health check path, protocol).
4. Create Auto Scaling Group:
   - Attach Launch Template.
   - Set min/desired/max capacity across subnets (multiple AZs).
   - Register Target Group with ASG so instances automatically join target group.
5. Configure Scaling Policies:
   - Target tracking (e.g., keep average CPU ~50%) or step scaling based on CloudWatch alarms.
6. Add CloudWatch metrics and alarms for monitoring.
7. Test scaling by generating load (e.g., curl loops, ApacheBench, or a load testing tool).
8. Verify health checks, instance registration, and that traffic is routed correctly by ALB.

Note: If using Terraform/CloudFormation, put the above resources into templates and deploy using `terraform apply` or `aws cloudformation deploy`.

## Testing & Validation
- Access ALB DNS name from browser to validate app response.
- Check Target Group in console to see healthy instances.
- Simulate increased load and confirm ASG scales out.
- Simulate reduced load and confirm ASG scales in.
- Inspect CloudWatch metrics and scaling activity logs.

## Cleanup & Cost Considerations
- Delete Auto Scaling Group, Load Balancer, Target Groups, Launch Templates, and EC2 instances.
- Remove associated CloudWatch alarms, unused EBS volumes, snapshots and security groups.
- Be mindful of ALB hourly/GB charges and EC2 instance costs—tear down resources if not in use.

Quick manual teardown (Console) order:
1. Terminate instances (or delete ASG to let it terminate).
2. Delete Auto Scaling Group.
3. Delete Target Group.
4. Delete Load Balancer.
5. Delete Launch Template.
6. Remove CloudWatch alarms and logs.

## Troubleshooting & Tips
- If instances show unhealthy: check EC2 user-data, security groups, and health check path.
- Ensure ALB security group allows inbound from the Internet (or intended sources).
- Ensure EC2 security group allows traffic from ALB security group.
- Use SSM Session Manager instead of SSH (requires IAM/SSM agent).
- For sticky sessions, enable ALB target group stickiness if needed.

## Learning Outcomes
- How ALB distributes traffic across targets and integrates with ASG.
- How scaling policies and CloudWatch alarms work together to adapt capacity.
- Best practices for secure, cost-aware, and highly-available deployments.

## References
- AWS ALB documentation: https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html
- AWS Auto Scaling docs: https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html
- CloudWatch alarms and metrics: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html

---

## Author
Ayushkumar


