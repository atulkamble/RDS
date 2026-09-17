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

CREATE TABLE student (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    age INT,
    gender VARCHAR(10),
    department VARCHAR(50),
    email VARCHAR(100),
    phone VARCHAR(15),
    city VARCHAR(50),
    admission_date DATE
);

INSERT INTO student
(first_name,last_name,age,gender,department,email,phone,city,admission_date)
VALUES
('Atul','Kamble',24,'Male','Computer Science','atul@example.com','9876543210','Pune','2026-05-21'),

('Ravi','Sharma',22,'Male','Mechanical','ravi@example.com','9876501234','Mumbai','2026-05-20'),

('Sneha','Patil',23,'Female','Electronics','sneha@example.com','9876512345','Nagpur','2026-05-19');


SELECT *from student;
```
