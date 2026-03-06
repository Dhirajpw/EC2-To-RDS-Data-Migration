# EC2-To-RDS-Data-Migration
This project demonstrates how to migrate a database from an Amazon EC2 instance to Amazon RDS to improve scalability, availability, and management.
# Architecture Overview
<img width="1672" height="655" alt="Screenshot 2026-03-06 201316" src="https://github.com/user-attachments/assets/a33c4f9a-c6c0-4ce5-9643-1a4ee9e7f89b" />
This workflow outlines the end-to-end process of migrating a MariaDB database from EC2 to Amazon RDS. It includes installing and configuring MariaDB on the EC2 instance, creating and populating the source database, generating a consistent backup using mysqldump, and restoring the data into the RDS MariaDB instance. Secure connectivity between EC2 and RDS is ensured through correctly configured security group rules and MariaDB client access, enabling a smooth and reliable migration.

Key Workflow:

- Configure MariaDB on EC2

- Create and populate the source database

- Export the database using mysqldump

- Import and validate data in Amazon RDS

# Prerequisites
- AWS EC2 instance with SSH access
- AWS RDS MariaDB instance
- Security Group allowing:
    - Port 22 (SSH)
    - Port 3306 (MariaDB)
- RDS endpoint, username, and password

# Step 1: Install MariaDB on EC2
    #!/bin/bash
    sudo apt update -y
    sudo apt install mariadb-server mariadb-client -y
    sudo systemctl start mariadb
    sudo systemctl enable mariadb
<img width="1643" height="707" alt="image" src="https://github.com/user-attachments/assets/82f4dd30-e244-4ac0-b825-af5984dca679" />
<img width="1186" height="532" alt="Screenshot 2026-03-06 221059" src="https://github.com/user-attachments/assets/564fd458-625e-46ad-9bb6-d60967fcca04" />

# Step 2: Configure Traditional MariaDB Ec2 and Create Database
```bash   
sudo mysql -u root -p
```
```sql
    SHOW DATABASES;

    CREATE DATABASE Student_db;
    SHOW DATABASES;

    USE Student_db;
    CREATE TABLE user (
      id INT,
      name VARCHAR(10),
      location VARCHAR(20)
    );

    INSERT INTO user VALUES
    (1,'Vishal','Pune'),
    (2,'Ganesh','Chakan'),
    (3,'Sanket','Kolhapur'),
    (4,'Omkar','Ahamedabad'),
    (5,'Yashraj','Sambhajinagar'),
    (6,'Rushikesh','Ahilyanagar'),
    (7,'Mangesh','Shirdi');
    
    SELECT * FROM user;
```
<img width="555" height="122" alt="Screenshot 2026-03-06 225747" src="https://github.com/user-attachments/assets/c71afabe-3c7e-4f89-be0b-0eacc66c403e" />
<img width="1095" height="64" alt="Screenshot 2026-03-06 225809" src="https://github.com/user-attachments/assets/c74cc41f-b063-4315-9724-bd0da5428a82" />
<img width="1892" height="422" alt="Screenshot 2026-03-06 225834" src="https://github.com/user-attachments/assets/cf26420f-a9a4-4235-ba87-fa652caa6fee" />


# Step 3: Backup Traditional Database
    mysqldump -u root -p Student_db > student_db_bkp.sql
<img width="779" height="966" alt="Screenshot 2026-03-06 230129" src="https://github.com/user-attachments/assets/8d550de0-aea0-4d4f-a1b6-9bcf543887fe" />

# Step 4.1: Create RDS instance
Create RDS server and attach to the running instance
<img width="1358" height="225" alt="Screenshot 2026-03-06 231630" src="https://github.com/user-attachments/assets/8c90f96d-b2ad-4cd5-9f38-8a57221e1735" />
# Step 4.2: Connect to AWS RDS to EC2
    sudo mysql -h <rds-endpoint> -u <rds-username> -p
<img width="1471" height="320" alt="Screenshot 2026-03-06 232033" src="https://github.com/user-attachments/assets/23aec4d3-c6a9-46e9-94ed-d265f584c2a4" />

Create new database in RDS and Restore Backup to RDS:
    CREATE DATABASE Student_db;
    SHOW DATABASES;
    USE Student_db;
<img width="1737" height="936" alt="Screenshot 2026-03-06 232458" src="https://github.com/user-attachments/assets/83640b36-3f75-459f-8c75-a2390d000517" />

Restore Backup to RDS
    sudo mysql -h <rds-endpoint> -u <rds-username> -p Student_db < Student_bkp.sql

# Step 5: Verify Migration on RDS
    SHOW DATABASES;
    USE Student_db;
    SHOW TABLES;
    SELECT * FROM user;
<img width="1737" height="936" alt="Screenshot 2026-03-06 232458" src="https://github.com/user-attachments/assets/168a19b7-dec1-4a6e-905c-72a8b2ab6909" />
<img width="513" height="371" alt="Screenshot 2026-03-06 232536" src="https://github.com/user-attachments/assets/43cedd11-d03c-4bd6-8785-cdf5af4031e1" />


# Conclusion
This project successfully demonstrates migration of a local MariaDB database from EC2 to Amazon RDS, ensuring a smooth transition from self-managed databases to managed AWS database services.

# Contact
Author: Dhiraj Waghumbare
Email: dhiraj.waghumbare@outlook.com
Linkedin: https://www.linkedin.com/in/dhiraj-waghumbare
