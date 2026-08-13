# Cloud Computing Summer Internship

## About the Internship

This repository contains the work, projects, documentation, and learning outcomes from my **Cloud Computing Summer Internship** completed through **Grass Solution Pvt. Ltd.**

The internship provided hands-on exposure to cloud computing and AWS services, with a focus on deploying, configuring, and managing cloud infrastructure through practical projects.

## Organization

**Grass Solution Pvt. Ltd.**

The internship was completed through Grass Solution Pvt. Ltd., where I worked on practical cloud computing tasks and AWS-based infrastructure projects.

## Internship Focus

During the internship, I worked with various AWS services and cloud infrastructure concepts, including:

* Amazon EC2
* Amazon VPC
* Public and Private Subnets
* VPC Peering
* Internet Gateway
* Route Tables
* Amazon RDS
* MySQL
* AWS Route 53
* Elastic IP
* Application Load Balancer
* Auto Scaling Groups
* NAT Gateway
* Bastion Host
* Security Groups
* DNS Routing
* High Availability
* Private Subnet Networking

## Projects Completed

### Project 1 – AWS Cloud Infrastructure

Introduction to AWS cloud infrastructure and core cloud networking concepts.

### Project 2 – VPC Peering with Public & Private Subnets

Built a multi-VPC architecture containing public and private subnets and established secure communication between separate VPCs using VPC Peering and private IP routing.

### Project 3 – Amazon RDS MySQL Implementation

Provisioned an Amazon RDS MySQL database and established a secure connection from an EC2 Linux instance using Security Groups. Database operations were performed to create a database, table, insert data, and verify the results.

### Project 4 – AWS Route 53 Domain Routing

Implemented DNS domain routing using Route 53 with two approaches:

* Route 53 → S3 static website
* Route 53 → EC2 web server using Elastic IP

The project also covered A records, Alias records, DNS propagation, verification, and cloud resource cleanup.

### Project 5 – EC2 Live Website + Database Integration

Deployed a PHP web application on an EC2 instance and integrated it with an Amazon RDS MySQL database. The project included Apache/PHP configuration, database connectivity, application deployment, and live verification.

### Project 6 – Application Load Balancer + Auto Scaling

Implemented a highly available architecture using an Application Load Balancer and Auto Scaling Group to distribute traffic across multiple EC2 instances and automatically replace failed instances.

The project demonstrated:

* Launch Templates
* Target Groups
* Application Load Balancer
* Auto Scaling Groups
* Health Checks
* Target Tracking
* Self-Healing
* High Availability

The Auto Scaling configuration used desired capacity of 2, minimum capacity of 1, maximum capacity of 4, with CPU utilization as the scaling metric.

### Project 7 – NAT Gateway & Private Subnet Internet Access

Configured a secure AWS network architecture in which an EC2 instance inside a private subnet could access the internet without having a public IP address.

The implementation included:

* Bastion Host
* Elastic IP
* NAT Gateway
* Private Route Table
* Private EC2 Instance
* SSH connectivity
* Outbound internet testing

The private instance was configured without a public IPv4 address, while the NAT Gateway provided outbound internet access.

## Skills & Technologies

**Cloud Platform**

* AWS

**Compute**

* Amazon EC2

**Networking**

* Amazon VPC
* Subnets
* Route Tables
* Internet Gateway
* NAT Gateway
* VPC Peering
* Elastic IP
* Security Groups
* Application Load Balancer

**Database**

* Amazon RDS
* MySQL

**Web & Server**

* Apache
* PHP
* Linux

**DNS**

* Amazon Route 53

**Cloud Architecture**

* High Availability
* Auto Scaling
* Self-Healing
* Private/Public Network Architecture
* DNS Routing
* Secure Cloud Connectivity

## Internship Documents

The repository also contains the official internship documentation:

* **Offer Letter** – Official internship offer/documentation from Grass Solution Pvt. Ltd.
* **Project Documentation** – Documentation and implementation details for the AWS projects completed during the internship.

## Learning Outcomes

Through this internship, I gained practical exposure to:

* Designing AWS cloud architectures
* Configuring EC2 instances and web servers
* Building public and private network environments
* Connecting EC2 with RDS MySQL
* Configuring DNS using Route 53
* Implementing load balancing and auto scaling
* Creating secure private-subnet architectures
* Using NAT Gateway for outbound connectivity
* Understanding high availability and self-healing infrastructure
* Working with Linux and AWS networking concepts

## Repository Structure

```text
Cloud-Computing-Summer-Internship/
│
├── Project-1/
├── Project-2-VPC-Peering/
├── Project-3-RDS/
├── Project-4-Route53/
├── Project-5-EC2-RDS-Integration/
├── Project-6-ALB-Auto-Scaling/
├── Project-7-NAT-Gateway/
│
├── Internship-Offer-Letter.pdf
│
└── README.md
```

## Conclusion

This internship provided practical experience with AWS cloud infrastructure and helped develop an understanding of how different AWS services can be combined to build secure, scalable, highly available, and production-oriented cloud architectures.

The projects in this repository document my hands-on learning and implementation throughout the **Cloud Computing Summer Internship** with **Grass Solution Pvt. Ltd.**
