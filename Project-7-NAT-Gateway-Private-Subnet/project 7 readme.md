# Project 7 — NAT Gateway & Private Subnet

Short description
-----------------
This project demonstrates how to design and implement a VPC that uses a NAT Gateway to provide outbound internet access for instances in private subnets, while keeping those instances unreachable from the public internet. The project shows the required AWS network components, routing, and verification steps.

Contents
--------
- Overview & architecture
- Resources created
- Prerequisites
- Deployment (Console / AWS CLI / Terraform)
- Verification / testing
- Cleanup
- Security & cost considerations
- Files in this folder
- Author / Contact

Overview & architecture
-----------------------
Goal: Host EC2 instances in a private subnet that can access the internet (for updates, package downloads, etc.) but are not directly reachable from the public internet. Use a public subnet with an Internet Gateway (IGW) and a NAT Gateway (with an Elastic IP) to provide outbound internet for the private subnet.

Architecture (logical):
- VPC (CIDR e.g. 10.0.0.0/16)
  - Public subnet (e.g. 10.0.1.0/24)
    - NAT Gateway (placed here)
    - Route table pointing 0.0.0.0/0 → Internet Gateway
  - Private subnet (e.g. 10.0.2.0/24)
    - EC2 instance(s) without public IP
    - Route table pointing 0.0.0.0/0 → NAT Gateway
  - Internet Gateway attached to VPC
  - Security groups and NACLs as required

Resources created
-----------------
- VPC
- Public subnet
- Private subnet
- Internet Gateway (IGW)
- Route table for public subnet (routes to IGW)
- Route table for private subnet (routes to NAT)
- Elastic IP (EIP) for NAT Gateway
- NAT Gateway in public subnet
- EC2 instance in private subnet for testing
- (Optional) Bastion host in public subnet for administrative SSH into private instance

Prerequisites
-------------
- AWS account with necessary IAM permissions to create VPCs, subnets, gateways, NATs, EC2 and EIPs.
- AWS CLI configured (if using CLI) or access to AWS Console.
- Basic knowledge of AWS networking and EC2.

Deployment
----------

1) Console-based quick steps
   - Create a VPC (10.0.0.0/16).
   - Create a public subnet (10.0.1.0/24) and private subnet (10.0.2.0/24) in the same AZ.
   - Create and attach an Internet Gateway to the VPC.
   - Create a route table for the public subnet: add route 0.0.0.0/0 → Internet Gateway; associate public subnet.
   - Allocate an Elastic IP (EIP).
   - Create a NAT Gateway in the public subnet and assign the EIP.
   - Create a route table for the private subnet: add route 0.0.0.0/0 → NAT Gateway; associate private subnet.
   - Launch an EC2 instance into the private subnet (no public IP). Optionally launch a bastion in the public subnet to access the private instance.

2) AWS CLI (examples)
   - Create VPC:
     aws ec2 create-vpc --cidr-block 10.0.0.0/16
   - After creating subnets and IGW, allocate an EIP:
     aws ec2 allocate-address --domain vpc
   - Create NAT gateway:
     aws ec2 create-nat-gateway --subnet-id <public-subnet-id> --allocation-id <eip-allocation-id>
   - Create route from private route table to NAT Gateway:
     aws ec2 create-route --route-table-id <private-rt-id> --destination-cidr-block 0.0.0.0/0 --nat-gateway-id <nat-gateway-id>

3) Terraform (recommended for repeatability)
   - Create a small Terraform configuration that defines aws_vpc, aws_subnet (public + private), aws_internet_gateway, aws_eip, aws_nat_gateway, aws_route_table and routes, and aws_instance.
   - Use modules or the official AWS provider docs for NAT+private patterns.
   - Example: see Terraform `aws_nat_gateway` & `aws_route_table` docs for usage patterns.

Verification / testing
----------------------
1. From a machine that can SSH into the private instance (e.g., a bastion host), connect to the private EC2 instance.
2. From the private instance, test internet access:
   - curl http://ifconfig.co
     - The output should show the Elastic IP of the NAT Gateway (not the private instance's IP).
   - sudo yum update (or apt-get update) — verify package downloads succeed.
3. Confirm inbound internet connectivity is blocked:
   - Try to reach the private instance from the internet (without bastion); it should fail because the instance has no public IP and the private subnet does not route to IGW.

Cleanup
-------
To avoid unexpected charges:
- Delete the NAT Gateway (note: you must delete the NAT before releasing the EIP).
- Release the Elastic IP.
- Detach and delete the Internet Gateway.
- Delete the EC2 instance(s), subnets, route tables, and VPC.
- If using Terraform: terraform destroy in the directory that created resources.

Security & cost considerations
------------------------------
- NAT Gateways are billed per hour + data processing charges. For low-cost environments consider NAT instances (but they require management and are less scalable).
- Ensure Security Groups allow only necessary outbound/inbound ports. Private instances should have minimal inbound rules.
- Use IAM least-privilege for automation scripts.
- Consider using AWS-managed solutions (NAT Gateway) for production for reliability; NAT instances may be useful for learning or cost control.
- When architecting for HA across AZs, create NAT Gateways in each AZ and route private subnets in that AZ to the local NAT Gateway.

References
----------
- AWS VPC documentation: https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html
- AWS NAT Gateway doc: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html
- Terraform AWS provider docs: https://registry.terraform.io/providers/hashicorp/aws/latest/docs

Author
------
- AYUSH KUMAR / GitHub: Ayushkumar2533
