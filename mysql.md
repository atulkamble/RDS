```
mysql >>

powershell run as admin >> 
choco install mysql


mysql >> 3306
http - 80 
https - 443 
ssh - 22

sudo dnf install mariadb1011 -y
sudo dnf install mariadb1011-server -y
mysql --version
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo systemctl status mariadb

sudo mysql 

SHOW DATABASES;
CREATE DATABASE university;
SHOW DATABASES;
USE university;

Below is a simple **MySQL CRUD practice lab** using a `university` database and a `students` table.

### 1. Create Database

```sql
CREATE DATABASE university;

USE university;
```

### 2. Create Students Table

```sql
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    course VARCHAR(100),
    age INT,
    city VARCHAR(100)
);
```

Check the table:

```sql
SHOW TABLES;

DESC students;
```

### 3. CREATE — Insert Student Records

Insert one student:

```sql
INSERT INTO students (name, email, course, age, city)
VALUES ('Rahul Sharma', 'rahul@gmail.com', 'Computer Science', 21, 'Pune');
```

Insert multiple students:

```sql
INSERT INTO students (name, email, course, age, city)
VALUES
('Priya Patil', 'priya@gmail.com', 'Information Technology', 22, 'Mumbai'),
('Amit Kumar', 'amit@gmail.com', 'Computer Science', 20, 'Delhi'),
('Sneha Joshi', 'sneha@gmail.com', 'Data Science', 23, 'Pune'),
('Rohan Singh', 'rohan@gmail.com', 'Cyber Security', 21, 'Mumbai');
```

### 4. READ — Retrieve Records

View all students:

```sql
SELECT * FROM students;
```

Find a specific student:

```sql
SELECT * FROM students
WHERE student_id = 1;
```

Students from Pune:

```sql
SELECT * FROM students
WHERE city = 'Pune';
```

Select specific columns:

```sql
SELECT name, course, city
FROM students;
```

Filter by age:

```sql
SELECT * FROM students
WHERE age >= 22;
```

Sort students:

```sql
SELECT * FROM students
ORDER BY name ASC;
```

### 5. UPDATE — Modify Records

Change a student's city:

```sql
UPDATE students
SET city = 'Nagpur'
WHERE student_id = 1;
```

Change multiple fields:

```sql
UPDATE students
SET course = 'Artificial Intelligence',
    age = 22
WHERE student_id = 1;
```

Verify:

```sql
SELECT * FROM students
WHERE student_id = 1;
```

### 6. DELETE — Remove Records

Delete one student:

```sql
DELETE FROM students
WHERE student_id = 5;
```

Verify:

```sql
SELECT * FROM students;
```

Delete students from a particular city:

```sql
DELETE FROM students
WHERE city = 'Mumbai';
```

### CRUD Summary

| CRUD           | SQL Command | Purpose             |
| -------------- | ----------- | ------------------- |
| **C – Create** | `INSERT`    | Add new student     |
| **R – Read**   | `SELECT`    | View student data   |
| **U – Update** | `UPDATE`    | Modify student data |
| **D – Delete** | `DELETE`    | Remove student data |

### Complete Practice Flow

```sql
-- Create Database
CREATE DATABASE university;
USE university;

-- Create Table
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    course VARCHAR(100),
    age INT,
    city VARCHAR(100)
);

-- CREATE
INSERT INTO students (name, email, course, age, city)
VALUES
('Rahul Sharma', 'rahul@gmail.com', 'Computer Science', 21, 'Pune'),
('Priya Patil', 'priya@gmail.com', 'Information Technology', 22, 'Mumbai'),
('Amit Kumar', 'amit@gmail.com', 'Computer Science', 20, 'Delhi');

-- READ
SELECT * FROM students;

-- UPDATE
UPDATE students
SET course = 'Data Science'
WHERE student_id = 1;

-- READ Updated Record
SELECT * FROM students
WHERE student_id = 1;

-- DELETE
DELETE FROM students
WHERE student_id = 3;

-- Final Records
SELECT * FROM students;
```

**Important:** Always use a `WHERE` condition with `UPDATE` and `DELETE` unless you intentionally want to modify/delete **all rows**.
