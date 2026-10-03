```
// RDS 

1. Search RDS >> RDS and Aurora
2. Full Configuration >> Free Tier 
database-1.c3iuq8u6iyy3.us-east-1.rds.amazonaws.com
mysql - 3306
identifier - database-1
db - mysql
admin
Admin123456

public access - enable 
defaults 
750hours - micro instance - automated backup - ON 
20GB - SSD 
Snapshot will be created 

Check Security Group of DB-Instance 

SG - Inbound - Mysql - 3306

3. Connect via code snipets

curl -o global-bundle.pem https://truststore.pki.rds.amazonaws.com/global/global-bundle.pem
mysql -h database-1.c3iuq8u6iyy3.us-east-1.rds.amazonaws.com -P 3306 -u admin -p --ssl-mode=VERIFY_IDENTITY --ssl-ca=./global-bundle.pem

4. connect via Cloud Shell >> create environment 

5. connect via SQLEctron 

download and install SQLEctron from https://sqlectron.github.io/

database-1.c3iuq8u6iyy3.us-east-1.rds.amazonaws.com
mysql - 3306
db - mysql
admin
Admin123456

6. after connection >> practice 

SHOW DATABASES;

CREATE DATABASE university;

USE university;

CREATE TABLE student (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(100)
);


INSERT INTO student
(name, department)
VALUES
('Atul', 'Cloud');

INSERT INTO student
(name, department)
VALUES
('Rahul', 'IT');

INSERT INTO student (name, department) VALUES
('Atul', 'Cloud'),
('Rahul', 'IT'),
('Priya', 'DevOps'),
('Amit', 'Cloud'),
('Sneha', 'HR'),
('Rohit', 'IT'),
('Neha', 'DevOps'),
('Vikas', 'Networking'),
('Pooja', 'Cloud'),
('Karan', 'Security'),
('Anjali', 'IT'),
('Suresh', 'DevOps'),
('Megha', 'Cloud'),
('Akash', 'Networking'),
('Riya', 'Security'),
('Manish', 'IT'),
('Komal', 'HR'),
('Nikhil', 'Cloud'),
('Sakshi', 'DevOps'),
('Rajesh', 'Networking');

SELECT *from student;

SELECT * FROM student
WHERE department = 'IT';

SELECT * FROM student
ORDER BY department DESC;

SELECT * FROM student
ORDER BY name ASC;










```
