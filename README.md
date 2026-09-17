# Database Fundamentals with AWS RDS: MySQL Setup, Tables & Data Operations

## 1. Introduction to Databases

A **database** is an organized collection of data that allows applications to store, retrieve, update, and delete information efficiently.

Example:

```text
Database: companydb

employees
--------------------------------
id | name  | department | salary
--------------------------------
1  | Atul  | Cloud      | 80000
2  | Rahul | DevOps     | 70000
```

Basic database operations are commonly called **CRUD**:

```text
C = Create
R = Read
U = Update
D = Delete
```

---

# 2. Types of Databases

### Relational Databases — SQL

Data is stored in **tables with rows and columns**.

Examples:

* MySQL
* PostgreSQL
* MariaDB
* Oracle
* Microsoft SQL Server

Example:

```text
Database
   |
   +-- Table
        |
        +-- Rows
        +-- Columns
```

### NoSQL Databases

Designed for flexible or non-relational data structures.

Examples:

```text
Key-Value     → DynamoDB
Document      → MongoDB
Graph         → Neo4j
In-memory     → Redis
```

### SQL vs NoSQL

| SQL               | NoSQL                                 |
| ----------------- | ------------------------------------- |
| Tables            | Documents / Key-value etc.            |
| Structured schema | Flexible schema                       |
| Relationships     | Often designed for distributed access |
| MySQL, PostgreSQL | DynamoDB, MongoDB                     |

---

# 3. Introduction to Amazon RDS

Amazon Web Services **Amazon Relational Database Service (RDS)** is a managed relational database service.

AWS manages many administrative tasks such as:

```text
Hardware provisioning
Database installation
Backups
Software patching
Monitoring
High availability options
Scaling
```

RDS supports database engines including:

```text
MySQL
PostgreSQL
MariaDB
Oracle
SQL Server
Amazon Aurora
```

### Basic Architecture

```text
             AWS Cloud
                 |
        +----------------+
        |      VPC       |
        |                |
Client--|--> EC2/App     |
        |       |        |
        |       | 3306   |
        |       v        |
        |   +---------+  |
        |   | RDS     |  |
        |   | MySQL   |  |
        |   +---------+  |
        +----------------+
```

For MySQL, the default port is:

```text
3306
```

---

# 4. Create an RDS MySQL Database

Go to:

```text
AWS Console
   ↓
RDS
   ↓
Databases
   ↓
Create database
```

For a basic lab, configure:

```text
Creation method: Standard create
Engine: MySQL
Template: Free tier / Dev-Test (as available)

DB instance identifier:
mydb

Master username:
admin

Credentials:
Set your own strong password

Instance class:
Choose a small instance suitable for the lab

Storage:
20 GiB

VPC:
Default VPC

Public access:
Yes          # only for a temporary learning lab

Security Group:
Create/select security group

Port:
3306
```

Then click:

```text
Create database
```

Wait until:

```text
Status: Available
```

> **Production note:** Avoid publicly accessible databases. Normally keep RDS in private subnets and connect through an application server, bastion, VPN, or other private connectivity.

---

# 5. Configure RDS Security Group

For a simple lab connection, the RDS security group needs to allow MySQL traffic from the machine or EC2 instance that will connect.

```text
Type: MySQL/Aurora
Protocol: TCP
Port: 3306
Source: Your IP address
```

Preferred architecture:

```text
Internet
   |
   v
EC2 / Application
Security Group: sg-app
   |
   | TCP 3306
   v
RDS MySQL
Security Group: sg-db
```

For EC2 → RDS, a better rule is:

```text
RDS Security Group

Type: MySQL/Aurora
Port: 3306
Source: sg-app
```

### Point to Remember

Do **not** use this for production:

```text
3306 → 0.0.0.0/0
```

It exposes the database port broadly.

---

# 6. Find the RDS Endpoint

Open:

```text
RDS
→ Databases
→ mydb
→ Connectivity & security
```

Copy the endpoint.

Example format:

```text
mydb.xxxxxxxxxxxx.us-east-1.rds.amazonaws.com
```

You will need:

```text
Endpoint
Username
Password
Port 3306
```

---

# 7. Install MySQL Client

## Amazon Linux 2023

```bash
sudo dnf update -y
sudo dnf install mariadb105 -y
```

Check:

```bash
mysql --version
```

## Ubuntu

```bash
sudo apt update
sudo apt install mysql-client -y
```

Check:

```bash
mysql --version
```

---

# 8. Connect to RDS MySQL

```bash
mysql -h <RDS-ENDPOINT> -P 3306 -u admin -p
```

Example:

```bash
mysql -h mydb.xxxxxxxxxxxx.us-east-1.rds.amazonaws.com -P 3306 -u admin -p
```

Enter the password when prompted.

Successful connection:

```text
Welcome to the MySQL monitor.

mysql>
```

Do not put the password directly in the command:

```bash
mysql -u admin -pMyPassword
```

Using `-p` and entering it interactively is safer.

---

# 9. Check Databases

Inside MySQL:

```sql
SHOW DATABASES;
```

Expected structure:

```text
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```

---

# 10. Create a Database

```sql
CREATE DATABASE companydb;
```

Verify:

```sql
SHOW DATABASES;
```

Select it:

```sql
USE companydb;
```

Check the currently selected database:

```sql
SELECT DATABASE();
```

---

# 11. Create a Table

Create an `employees` table:

```sql
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(100),
    salary DECIMAL(10,2)
);
```

Check tables:

```sql
SHOW TABLES;
```

Check table structure:

```sql
DESCRIBE employees;
```

Architecture:

```text
companydb
    |
    +--- employees
           |
           +--- id
           +--- name
           +--- department
           +--- salary
```

---

# 12. Insert Data into Table

Insert one record:

```sql
INSERT INTO employees
(name, department, salary)
VALUES
('Atul', 'Cloud', 80000);
```

Insert multiple records:

```sql
INSERT INTO employees
(name, department, salary)
VALUES
('Rahul', 'DevOps', 70000),
('Priya', 'AWS', 75000),
('Amit', 'Linux', 60000);
```

---

# 13. Read Data

Display all records:

```sql
SELECT * FROM employees;
```

Example:

```text
+----+-------+------------+----------+
| id | name  | department | salary   |
+----+-------+------------+----------+
| 1  | Atul  | Cloud      | 80000.00 |
| 2  | Rahul | DevOps     | 70000.00 |
| 3  | Priya | AWS        | 75000.00 |
| 4  | Amit  | Linux      | 60000.00 |
+----+-------+------------+----------+
```

Select specific columns:

```sql
SELECT name, department FROM employees;
```

Filter records:

```sql
SELECT * FROM employees
WHERE department = 'DevOps';
```

Salary filter:

```sql
SELECT * FROM employees
WHERE salary > 70000;
```

Sort:

```sql
SELECT * FROM employees
ORDER BY salary DESC;
```

---

# 14. Update Data

Change an employee's salary:

```sql
UPDATE employees
SET salary = 85000
WHERE id = 1;
```

Verify:

```sql
SELECT * FROM employees;
```

### Important

Always be careful with `WHERE`.

Correct:

```sql
UPDATE employees
SET salary = 85000
WHERE id = 1;
```

Dangerous:

```sql
UPDATE employees
SET salary = 85000;
```

The second command updates **every row**.

---

# 15. Delete Data

Delete one record:

```sql
DELETE FROM employees
WHERE id = 4;
```

Verify:

```sql
SELECT * FROM employees;
```

Again, avoid:

```sql
DELETE FROM employees;
```

unless you intentionally want to remove every row.

---

# 16. CRUD Practice

```text
CREATE
   ↓
INSERT INTO employees ...

READ
   ↓
SELECT * FROM employees;

UPDATE
   ↓
UPDATE employees
SET ...
WHERE ...;

DELETE
   ↓
DELETE FROM employees
WHERE ...;
```

Commands:

```sql
INSERT INTO employees
(name, department, salary)
VALUES ('John', 'AWS', 65000);

SELECT * FROM employees;

UPDATE employees
SET salary = 70000
WHERE name = 'John';

DELETE FROM employees
WHERE name = 'John';
```

---

# 17. Useful MySQL Commands

```sql
SHOW DATABASES;

CREATE DATABASE companydb;

USE companydb;

SHOW TABLES;

DESCRIBE employees;

SELECT * FROM employees;

SELECT DATABASE();

SELECT VERSION();

DROP TABLE employees;

DROP DATABASE companydb;

EXIT;
```

Be careful with:

```sql
DROP TABLE employees;
```

and:

```sql
DROP DATABASE companydb;
```

These remove database objects rather than individual records.

---

# 18. Basic Troubleshooting

### Cannot connect to RDS

Check:

```text
1. RDS Status = Available
2. Correct endpoint
3. Correct username
4. Correct password
5. Port = 3306
6. Security Group allows the client
7. Network routing/connectivity is available
8. Public accessibility is configured appropriately for a direct lab connection
```

Test DNS:

```bash
nslookup <RDS-ENDPOINT>
```

Test port connectivity:

```bash
nc -zv <RDS-ENDPOINT> 3306
```

Then retry:

```bash
mysql -h <RDS-ENDPOINT> -P 3306 -u admin -p
```

---

# 19. Points to Remember

* **RDS is managed** — AWS handles much of the database infrastructure administration.
* **MySQL uses TCP 3306** by default.
* **Endpoint is the hostname** used to connect to RDS.
* **Security Groups control network access** to the database.
* Keep production RDS instances **private whenever practical**.
* Never expose port `3306` to `0.0.0.0/0` for convenience.
* Don't hard-code database passwords in application code, Git repositories, or scripts.
* Use strong authentication and least-privilege database accounts.
* Enable appropriate **backups, encryption, monitoring, and Multi-AZ** based on production requirements.
* Use `WHERE` carefully with `UPDATE` and `DELETE`.
* Take extra care before running `DROP DATABASE`, `DROP TABLE`, or bulk deletion commands.

## Quick Lab Flow

```text
Create RDS MySQL
       ↓
Configure Networking/Security Group
       ↓
Copy RDS Endpoint
       ↓
Install MySQL Client
       ↓
Connect to RDS
       ↓
CREATE DATABASE
       ↓
CREATE TABLE
       ↓
INSERT DATA
       ↓
SELECT DATA
       ↓
UPDATE DATA
       ↓
DELETE DATA
```

### Minimum Commands for Live Practice

```bash
mysql -h <RDS-ENDPOINT> -P 3306 -u admin -p
```

```sql
CREATE DATABASE companydb;

USE companydb;

CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(100),
    salary DECIMAL(10,2)
);

INSERT INTO employees
(name, department, salary)
VALUES
('Atul', 'Cloud', 80000),
('Rahul', 'DevOps', 70000),
('Priya', 'AWS', 75000);

SELECT * FROM employees;

UPDATE employees
SET salary = 85000
WHERE id = 1;

DELETE FROM employees
WHERE id = 2;

SELECT * FROM employees;

EXIT;
```

This gives you a clean **RDS MySQL → Database → Table → Insert → Select → Update → Delete** live classroom lab.
