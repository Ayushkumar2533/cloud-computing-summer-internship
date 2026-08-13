# Project 2 — VPC Peering with Public & Private Subnets

Author: Ayush kumar
Repository: cloud-computing-summer-internship  
Project: VPC Peering, Public and Private Subnets (Project 2)  


## Overview
This project demonstrates designing and implementing two AWS VPCs connected by a VPC Peering connection. Each VPC contains a public subnet and a private subnet. The public subnet hosts bastion (jump) instances and NAT Gateway as needed; the private subnet hosts application servers that are not directly accessible from the internet. Routing and security group rules are configured to allow secure, controlled communication between resources in the two VPCs.

Objectives:
- Create two VPCs with non-overlapping CIDR blocks.
- Create public and private subnets in each VPC.
- Configure Internet Gateway (IGW) for public subnets and NAT Gateway for private subnets (optional).
- Establish VPC Peering between the two VPCs and update route tables to allow cross-VPC traffic.
- Deploy EC2 instances to validate connectivity.
- Demonstrate secure access patterns (bastion → private instance).

## Architecture Diagram (ASCII)
Replace this with a drawn diagram if desired.

VPC A (10.0.0.0/16)                      VPC B (10.1.0.0/16)
----------------------                   ----------------------
| Public Subnet A   | <--- IGW            | Public Subnet B   | <--- IGW
| - Bastion (EC2)   |                     | - Bastion (EC2)   |
----------------------                   ----------------------
| Private Subnet A  |                     | Private Subnet B  |
| - App Server (EC2)|                     | - App Server (EC2)|
----------------------                   ----------------------
          \                                    /
           \  VPC Peering Connection (peering-id)
            \                                /
             --------------------------------
                  Private routing between VPCs

## Key components / Resources created
- VPC A and VPC B (non-overlapping CIDRs, e.g., 10.0.0.0/16 and 10.1.0.0/16)
- Public and Private subnets in each VPC (spread across AZs if desired)
- Internet Gateway (IGW) attached to each VPC (for public subnets)
- Route Tables (public/private) with appropriate routes
- NAT Gateway in public subnet (optional) to enable outbound internet for private subnet
- VPC Peering connection between VPC A and VPC B
- Security Groups for bastion and app servers
- EC2 instances: bastion hosts in public subnets, app servers in private subnets

## CIDR plan (example)
- VPC A: 10.0.0.0/16
  - Public Subnet A: 10.0.1.0/24
  - Private Subnet A: 10.0.2.0/24
- VPC B: 10.1.0.0/16
  - Public Subnet B: 10.1.1.0/24
  - Private Subnet B: 10.1.2.0/24

## Security & Best Practices
- Use least-privilege security groups: only open required ports (SSH: 22 to bastion, app ports between subnets).
- Do NOT allow 0.0.0.0/0 to private instances.
- Use key pairs or AWS Systems Manager Session Manager for secure access.
- Avoid overlapping CIDR ranges for peered VPCs.
- If applying in production, use multiple AZs for HA and route table design accordingly.

## How I built it (high-level steps)
1. Create VPCs:
   - Create VPC A (CIDR 10.0.0.0/16).
   - Create VPC B (CIDR 10.1.0.0/16).
2. Create subnets:
   - Add public and private subnets to each VPC.
3. Create Internet Gateways:
   - Attach IGW to each VPC.
4. Configure route tables:
   - Public route table: route 0.0.0.0/0 → IGW.
   - Private route table: route 0.0.0.0/0 → NAT Gateway (if used).
5. Create NAT Gateway (in each public subnet) if private instances require outbound internet.
6. Launch EC2 instances:
   - Bastion host(s) in public subnets with public IP.
   - Application server(s) in private subnets (no public IP).
7. Create VPC Peering connection:
   - Request peering from VPC A → VPC B and accept from VPC B (or vice versa).
8. Update route tables for peering:
   - In VPC A route table for private subnets, add route to 10.1.0.0/16 via peering connection.
   - In VPC B route table for private subnets, add route to 10.0.0.0/16 via peering connection.
9. Update security groups:
   - Allow traffic between private subnets (e.g., app server SG allows inbound from peer VPC CIDR or specific SG).
10. Validate connectivity:
   - SSH to bastion host (Public IP).
   - From bastion, SSH (or ping/curl) to app server private IP in same VPC and to app server in peered VPC (using private IP).

## Deployment 

### Using AWS Console
- Use the VPC Dashboard to create VPCs, subnets, route tables, IGWs, NAT Gateways.
- Use the VPC Peering section to create and accept peering.
- Edit route tables to add routes to the peered VPC CIDR via the peering connection.
- Launch EC2 instances in the Console and attach security groups.

