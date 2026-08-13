# Project 5 — EC2 Live Website & Database Integration

Short summary
A demo project showing how to deploy a web application on an AWS EC2 instance and connect it to a database (Amazon RDS or MySQL/Postgres on EC2). The original report and screenshots are in `Project 5 cloud computing.pdf`.

What this contains
- Steps to provision an EC2 web server, install a web stack, and deploy app code
- Options to use Amazon RDS or a DB on EC2 and how to connect securely
- Security group and SSH best practices, testing, and cleanup guidance

Tech stack (example)
- OS: Ubuntu / Amazon Linux
- Web server: Apache or Nginx
- Runtime: PHP / Node.js / Python (as applicable)
- Database: Amazon RDS (recommended) or MySQL/Postgres

Quick deploy (example)
1. Launch EC2 (Ubuntu), open SSH (22) and HTTP (80) in SG.  
2. SSH in: `ssh -i "key.pem" ubuntu@<EC2_IP>`.  
3. Install web server & runtime (e.g., `sudo apt install apache2 php php-mysql`).  
4. Deploy app to `/var/www/html`, set permissions.  
5. Provision DB (RDS or MySQL), create DB/user, allow access from web SG.  
6. Configure app DB settings (.env or config), restart web server, test at `http://<EC2_IP>`.

Security & best practices
- Use key-based SSH, restrict SSH by IP.  
- Keep secrets out of source (use env vars / AWS Secrets Manager).  
- Use HTTPS in production and restrict DB access to the web server only.

Cleanup
Terminate EC2, delete snapshots/EBS, and remove RDS to avoid charges.

Author:
Ayushkumar2533
