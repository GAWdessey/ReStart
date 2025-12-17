# AWS Project: Relational Database Implementation with Amazon RDS

## Project Overview
As part of the **AWS ReStart Program**, I completed a challenge lab designed to reinforce the deployment of managed database instances and the execution of SQL commands via a remote interface. 

This project demonstrates the architecture of a secure **Amazon RDS (MySQL)** instance connected to a **Linux EC2 Bastion Host**, simulating a real-world tiered architecture where databases are isolated from direct public access.

### 🛠 Architecture & Services Used
* **Amazon RDS (Relational Database Service):** MySQL engine (db.t3.micro).
* **Amazon EC2:** Linux server acting as a jump box/bastion host.
* **VPC Security Groups:** Configured ingress rules for Port 3306 (MySQL) strictly from the web server.
* **MySQL Client:** Command Line Interface (CLI) for database interaction.
* **SQL:** DDL (Data Definition Language) and DML (Data Manipulation Language).

---

## 📸 Implementation Steps & Evidence

### Phase 1: Infrastructure Setup
I launched an RDS MySQL instance within the Lab VPC and deployed a Linux EC2 instance. The critical security step involved configuring the **Security Group** to allow traffic on port `3306` only from the Linux Server's security group, ensuring the database was not exposed to the open internet.

**Connecting to the Server:**
Using SSH to connect to the EC2 instance and installing the MariaDB/MySQL client.
![EC2 Connection](./Capture1.PNG)

**RDS Configuration:**
Verifying the RDS instance status and endpoint availability in the AWS Console.
![RDS Console](./Capture2.PNG)

---

### Phase 2: Database Architecture (SQL)
Once connected to the RDS instance via the MySQL CLI, I designed a relational schema to track Student and Certification data.

#### Step 1: Creating the Primary Table
I created a table named `RESTART` to hold student demographic data.
* **Columns:** StudentID (PK), StudentName, RestartCity, GraduationDate.

![Create Table RESTART](./Screenshot1Capture.PNG)

#### Step 2: Populating Data
Inserted 10 sample records representing students from various cohort locations (Cape Town, Johannesburg, Durban, Pretoria).

![Insert Data RESTART](./Screenshot2Capture.PNG)

#### Step 3: Verifying Data Integrity
Queried the table to ensure all records were committed correctly.

![Select All RESTART](./Screenshot3Capture.PNG)

---

### Phase 3: Relational Data & Foreign Keys

#### Step 4: Creating the Secondary Table
Created a `CLOUD_PRACTITIONER` table to track certification dates.
* **Constraint:** Established a **Foreign Key** relationship linking `StudentID` to the `RESTART` table to maintain referential integrity.

![Create Table CLOUD_PRACTITIONER](./Screenshot4Capture.PNG)

#### Step 5: Populating Certification Data
Inserted certification records for specific students.

![Insert Data CLOUD_PRACTITIONER](./Screenshot5Capture.PNG)

#### Step 6: Verifying Secondary Data
Queried the certification table to review entries.

![Select All CLOUD_PRACTITIONER](./Screenshot6Capture.PNG)

---

### Phase 4: Data Analysis (Joins)

To generate a meaningful report, I performed an **INNER JOIN** between the `RESTART` and `CLOUD_PRACTITIONER` tables. This query combined data to show exactly which students have achieved certification and when.

```sql
SELECT 
    R.StudentID, 
    R.StudentName, 
    C.CertificationDate 
FROM RESTART R 
INNER JOIN CLOUD_PRACTITIONER C 
ON R.StudentID = C.StudentID;
