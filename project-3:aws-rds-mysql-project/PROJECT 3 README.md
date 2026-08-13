
# AWS RDS MySQL Project (Project 3)

## Overview
This project shows how to provision and use an AWS RDS MySQL database for a sample application. It includes steps for creating the RDS instance, securing access, connecting from an app or client, basic schema examples, backups, and cleanup instructions.

## Features
- Provision an AWS RDS MySQL instance
- Secure access with VPC Security Groups
- Example database schema and sample SQL
- Backup, restore, and cost-management guidance

## Technologies
- AWS RDS (MySQL)
- MySQL client (mysql, MySQL Workbench, DBeaver)
- Optional: Node.js / Python / Java for sample app
- Optional: AWS CLI or Terraform for IaC

## Prerequisites
- AWS account with RDS permissions
- AWS CLI configured (recommended)
- MySQL client installed locally
- Basic SQL / app-DB knowledge

## Quick Setup (copy/paste friendly)

1. Create RDS instance (Console or CLI)
   - Engine: MySQL (choose version)
   - DB instance id: `project3-rds`
   - Master username: `admin` (choose secure)
   - Master password: (store securely)
   - Instance class: e.g., `db.t3.micro` (or free-tier if eligible)
   - Public accessibility: No (recommended for production)

2. Security Group
   - Allow inbound TCP port `3306`
   - For development: Source = your IP (e.g., `203.0.113.45/32`)
   - For production: restrict to app server SG only

3. Connect (example)
   From a permitted host or allowed IP:
   ```
   mysql -h <RDS_ENDPOINT> -u admin -p
   ```

4. Create DB & user (run inside MySQL)
   ```sql
   CREATE DATABASE project3_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

   CREATE USER 'app_user'@'%' IDENTIFIED BY 'StrongPasswordHere';
   GRANT ALL PRIVILEGES ON project3_db.* TO 'app_user'@'%';
   FLUSH PRIVILEGES;
   ```

5. App configuration (example .env)
   ```
   DB_HOST=<RDS_ENDPOINT>
   DB_PORT=3306
   DB_NAME=project3_db
   DB_USER=app_user
   DB_PASSWORD=StrongPasswordHere
   ```

## Example Schema
```sql
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(100) NOT NULL UNIQUE,
  email VARCHAR(255) NOT NULL UNIQUE,
  password_hash VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE posts (
  id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT NOT NULL,
  title VARCHAR(255) NOT NULL,
  body TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

## Backup & Restore
- Enable automated backups (point-in-time recovery) during creation.
- To take a manual snapshot: RDS Console → Databases → Actions → Take snapshot.
- Restore from snapshot: Actions → Restore snapshot (creates a new instance).
- Export/import with mysqldump:
  ```
  mysqldump -h <source> -u <user> -p <db> > dump.sql
  mysql -h <rds> -u <user> -p <db> < dump.sql
  ```

## Security & Best Practices
- Do NOT enable public access for production.
- Use Security Groups, not broad IP rules.
- Store credentials in AWS Secrets Manager or environment variables.
- Enable encryption at rest and enforce SSL/TLS in transit.
- Consider IAM DB authentication for additional security.
- Rotate credentials regularly and grant least privilege.

## Cost Management & Cleanup
- Choose small instance classes for development.
- Stop or delete RDS instances when not needed.
- Delete unnecessary snapshots (snapshots incur storage cost).
- To delete: RDS Console → Databases → Actions → Delete.

## Troubleshooting
- "Connection refused": check Security Group, VPC/subnet, and public accessibility.
- "Access denied": check DB username, password, and user host (`'user'@'host'`) privileges.
- Slow queries: enable slow query log and add indexes as needed.

## Contributing
1. Fork repo
2. Create branch: `git checkout -b feature/your-change`
3. Commit and push
4. Open a Pull Request with a description

## Author
- Name: Your Name
- GitHub: https://github.com/ayushkumar2533
- Email: ayushkumar2533@gmail.com

```
