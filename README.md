Creating a Database

```
CREATE DATABASE mydb;
```

Basic data types in SQL:

  - INT -> Whole Number
  - DECIMAL (M,N) -> M is the total number of digits, N is the number of digits after the decimal point
  - VARCHAR(len) -> string of length *len*
  - BLOB -> Binary large object can store large binary files such as photos, pdf etc.
  - DATE -> 'YYYY-MM-DD'
  - TIMESTAMP -> 'YYYY_MM_DD HH:MM:SS'

Use a database
```
USE mydb;
```

Creating a table
```
CREATE TABLE student (
  student_id INT PRIMARY KEY,
  name VARCHAR(30),
  major VARCHAR(20)
);
```

Alternate syntax to set the primary key
```
CREATE TABLE student (
  student_id INT,
  name VARCHAR(30),
  major VARCHAR(20),
  PRIMARY KEY (student_id)
);
```

Describe a table
```
DESCRIBE student;
```

Deleting a table
```
DROP TABLE student;
```

Add a column to a table after creating it
```
ALTER TABLE student ADD percent DECIMAL(4,2);
```

Delete a column from table
```
ALTER student DROP percent;
```

Select all records in a table
```
SELECT * from student
```

Inserting data in the table (observe the order).
```
INSERT INTO student VALUES(1, 'Joe', 'Mathematics', 88.96);
```

Another method to insert data in table
```
INSERT INTO student(name, student_id, percent, major)
    VALUES('Mary', 2, 90.19, 'Psycology');
```

If we don't add data in some column then it is set to NULL
```
INSERT INTO student(name, student_id, percent)
    VALUES('Harry', 3, 79.99);
```

Constraints of columns
  - DEFAULT 'xyz' -> if the data for the particular column is not provided then instead of setting it to NULL, it will be set to 'xyz'
  - NOT NULL -> the column will not caontain any NULL values.
  - AUTO_INCREMENT -> the value of column will automatically increment.
  - PRIMARY KEY -> sets the column as the Primary Key
  - UNIQUE -> the column will contain unique values.

Let's now create a new database with some Constraints
```
CREATE TABLE student (
  student_id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(30) NOT NULL UNIQUE,
  major VARCHAR(20) DEFAULT 'Yet to decide',
  percent DECIMAL(4,2)  
);


INSERT INTO student(name, major, percent) VALUES('Joe', 'Mathematics', 89.57);
INSERT INTO student(name, major, percent) VALUES('Travis', 'Biology', 85.32);
INSERT INTO student(name, major, percent) VALUES('Kanye', 'Psycology', 99.47);
INSERT INTO student(name, major, percent) VALUES('Wiz', 'Psycology', 93.27);
INSERT INTO student(name, major, percent) VALUES('Drake', 'Chemistry', 79.19);
INSERT INTO student(name, major, percent) VALUES('Kodak', 'Geography', 87.25);
INSERT INTO student(name, percent) VALUES('Uzi', 83.63);
INSERT INTO student(name, major ) VALUES('Gunna', 'Biology');
INSERT INTO student(name, major, percent) VALUES('Savage', 'Physics', 83.99);
INSERT INTO student(name, major, percent) VALUES('Passenger', 'Biology', 84.25);
INSERT INTO student(name, percent) VALUES('SixNine' , 85.29);
INSERT INTO student(name, major, percent) VALUES('Amine', 'Mathematics', 75.12);
```

Selecting data
```
mysql > SELECT * from student;
+------------+-----------+---------------+---------+
| student_id | name      | major         | percent |
+------------+-----------+---------------+---------+
|          1 | Joe       | Mathematics   |   89.57 |
|          2 | Travis    | Biology       |   85.32 |
|          3 | Kanye     | Psycology     |   99.47 |
|          4 | Wiz       | Psycology     |   93.27 |
|          5 | Drake     | Chemistry     |   79.19 |
|          6 | Kodak     | Geography     |   87.25 |
|          7 | Uzi       | Yet to decide |   83.63 |
|          8 | Gunna     | Biology       |    NULL |
|          9 | Savage    | Physics       |   83.99 |
|         10 | Passenger | Biology       |   84.25 |
|         11 | SixNine   | Yet to decide |   85.29 |
|         12 | Amine     | Mathematics   |   75.12 |
+------------+-----------+---------------+---------+
```
In the above command * means return all columns

Selecting particular columns
```
mysql> SELECT major,name from student;
+---------------+-----------+
| major         | name      |
+---------------+-----------+
| Mathematics   | Joe       |
| Biology       | Travis    |
| Psycology     | Kanye     |
| Psycology     | Wiz       |
| Chemistry     | Drake     |
| Geography     | Kodak     |
| Yet to decide | Uzi       |
| Biology       | Gunna     |
| Physics       | Savage    |
| Biology       | Passenger |
| Yet to decide | SixNine   |
| Mathematics   | Amine     |
+---------------+-----------+
```

Selecting rows with specific values
```
mysql> SELECT * from student WHERE major='Biology';
+------------+-----------+---------+---------+
| student_id | name      | major   | percent |
+------------+-----------+---------+---------+
|          2 | Travis    | Biology |   85.32 |
|          8 | Gunna     | Biology |    NULL |
|         10 | Passenger | Biology |   84.25 |
+------------+-----------+---------+---------+

mysql> SELECT name,percent from student WHERE major='Mathematics';
+-------+---------+
| name  | percent |
+-------+---------+
| Joe   |   89.57 |
| Amine |   75.12 |
+-------+---------+
```

Conditional operators
```
mysql> SELECT name,percent from student where major='Psycology' OR major='Biology';
+-----------+---------+
| name      | percent |
+-----------+---------+
| Travis    |   85.32 |
| Kanye     |   99.47 |
| Wiz       |   93.27 |
| Gunna     |    NULL |
| Passenger |   84.25 |
+-----------+---------+

mysql> SELECT name,major,percent from student where percent > 90;
+-------+-----------+---------+
| name  | major     | percent |
+-------+-----------+---------+
| Kanye | Psycology |   99.47 |
| Wiz   | Psycology |   93.27 |
+-------+-----------+---------+

mysql> SELECT name,major,percent from student where percent > 80 and major='Mathematics';
+------+-------------+---------+
| name | major       | percent |
+------+-------------+---------+
| Joe  | Mathematics |   89.57 |
+------+-------------+---------+
```

Updating databse.
```
UPDATE student
  SET major='Maths'
  WHERE major='Mathematics';
```

Deleting specific rows
```
DELETE FROM student WHERE major='Yet to decide';
```

Limiting the content recieved
```
mysql> SELECT * from student LIMIT 3;
+------------+--------+-----------+---------+
| student_id | name   | major     | percent |
+------------+--------+-----------+---------+
|          1 | Joe    | Maths     |   89.57 |
|          2 | Travis | Biology   |   85.32 |
|          3 | Kanye  | Psycology |   99.47 |
+------------+--------+-----------+---------+

```

Ordering the data
```
mysql> SELECT name,percent from student ORDER BY percent DESC LIMIT 3;
+-----------+---------+
| name      | percent |
+-----------+---------+
| Kanye     |   99.47 |
| Wiz       |   93.27 |
| Joe       |   89.57 |
+-----------+---------+
```

The IN keyword
```
mysql> SELECT * from student where major IN ('Maths','Psycology') AND percent > 85;
+------------+-------+-----------+---------+
| student_id | name  | major     | percent |
+------------+-------+-----------+---------+
|          1 | Joe   | Maths     |   89.57 |
|          3 | Kanye | Psycology |   99.47 |
|          4 | Wiz   | Psycology |   93.27 |
+------------+-------+-----------+---------+
```

Lets understand some concepts.

Primary Key - A primary key for a table is a column that has unique values and thus can be used to uniquely identify any row. You can imagine the data stored in tables by using a hashed values for the primary key, thus providing O(1) time for searching given primary keys.

Composite key - A table can have more than one primary key, and if this is the case then it may be allowed for a column to have duplicate values but the combinations of these (keys ) would certainly have to be different for each row.

Foreign Key - A foreign key  a field (or collection of fields) in one table that refers to the primary key in another table.


##### Lets now create a more complex Database.
<a id="raw-url" style="color:dodgerblue" href="https://github.com/gaurav1620/mysql-basics/blob/master/company-database.pdf">Click here</a> to view the database that we are going to create.

In companies, a dabase mostly consists of more than one table. We will create a complex model of The Office, with multiple tables for Employees, Branch, Client, Branch_Supplier etc. These tables contain foreign keys to other tables.

##### Schemas :
Employee Schema
```
CREATE TABLE employee (
  emp_id INT PRIMARY KEY,
  first_name VARCHAR(40),
  last_name VARCHAR(40),
  birth_day DATE,
  sex VARCHAR(1),
  salary INT,
  super_id INT,
  branch_id INT
);
```

Branch Schema
```
CREATE TABLE branch (
  branch_id INT PRIMARY KEY,
  branch_name VARCHAR(40),
  mgr_id INT,
  mgr_start_date DATE,
  FOREIGN KEY(mgr_id) REFERENCE employee(emp_id) ON DELETE SET NULL
);
```

Add the branch_id foreign key to the employee table
```
ALTER TABLE employee
ADD FOREIGN KEY(branch_id)
REFERENCES branch(branch_id)
ON DELETE SET NULL;
```

Add a super_id (supervisor id) to the employee table. Also observe that a supervisor is himself a employee. This can be done because, a table can have a foreign key which references its own primary key.
```
ALTER TABLE employee
ADD FOREIGN KEY(super_id)
REFERENCES employee(emp_id)
ON DELETE SET NULL;
```

Create the client table schema. Clients are the companies that buy the product from a specific branch.
```
CREATE TABLE client (
  client_id INT PRIMARY KEY,
  client_name VARCHAR(40),
  branch_id INT,
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE SET NULL
);
```

Create the works_with table. This table will store the sales that an amployee has made to a company (both these parameters are the primary key heare).
```
CREATE TABLE works_with (
  emp_id INT,
  client_id INT,
  total_sales INT,
  PRIMARY KEY(emp_id, client_id),
  FOREIGN KEY(emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
  FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
);
```

Create the branch_supplier table.
```
CREATE TABLE branch_supplier (
  branch_id INT,
  supplier_name VARCHAR(40),
  supply_type VARCHAR(40),
  PRIMARY KEY(branch_id, supplier_name),
  FOREIGN KEY(branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE
);
```

Add the data. Observe the order in which the data is inserted. For exampleobserver that branch_id is a foreign key for employee table whereas mgr_id is the foreign key for branch table which references the employee table. So this is a circular relationship, in order to insert data in these type of tables, first we insert null in some columns and then update after the reference table is created for the foreign key.
```

-- Corporate
INSERT INTO employee VALUES(100, 'David', 'Wallace', '1967-11-17', 'M', 250000, NULL, NULL);

INSERT INTO branch VALUES(1, 'Corporate', 100, '2006-02-09');

UPDATE employee
SET branch_id = 1
WHERE emp_id = 100;

INSERT INTO employee VALUES(101, 'Jan', 'Levinson', '1961-05-11', 'F', 110000, 100, 1);

-- Scranton
INSERT INTO employee VALUES(102, 'Michael', 'Scott', '1964-03-15', 'M', 75000, 100, NULL);

INSERT INTO branch VALUES(2, 'Scranton', 102, '1992-04-06');

UPDATE employee
SET branch_id = 2
WHERE emp_id = 102;

INSERT INTO employee VALUES(103, 'Angela', 'Martin', '1971-06-25', 'F', 63000, 102, 2);
INSERT INTO employee VALUES(104, 'Kelly', 'Kapoor', '1980-02-05', 'F', 55000, 102, 2);
INSERT INTO employee VALUES(105, 'Stanley', 'Hudson', '1958-02-19', 'M', 69000, 102, 2);

-- Stamford
INSERT INTO employee VALUES(106, 'Josh', 'Porter', '1969-09-05', 'M', 78000, 100, NULL);

INSERT INTO branch VALUES(3, 'Stamford', 106, '1998-02-13');

UPDATE employee
SET branch_id = 3
WHERE emp_id = 106;

INSERT INTO employee VALUES(107, 'Andy', 'Bernard', '1973-07-22', 'M', 65000, 106, 3);
INSERT INTO employee VALUES(108, 'Jim', 'Halpert', '1978-10-01', 'M', 71000, 106, 3);


-- BRANCH SUPPLIER
INSERT INTO branch_supplier VALUES(2, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'Uni-ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Patriot Paper', 'Paper');
INSERT INTO branch_supplier VALUES(2, 'J.T. Forms & Labels', 'Custom Forms');
INSERT INTO branch_supplier VALUES(3, 'Uni-ball', 'Writing Utensils');
INSERT INTO branch_supplier VALUES(3, 'Hammer Mill', 'Paper');
INSERT INTO branch_supplier VALUES(3, 'Stamford Lables', 'Custom Forms');

-- CLIENT
INSERT INTO client VALUES(400, 'Dunmore Highschool', 2);
INSERT INTO client VALUES(401, 'Lackawana Country', 2);
INSERT INTO client VALUES(402, 'FedEx', 3);
INSERT INTO client VALUES(403, 'John Daly Law, LLC', 3);
INSERT INTO client VALUES(404, 'Scranton Whitepages', 2);
INSERT INTO client VALUES(405, 'Times Newspaper', 3);
INSERT INTO client VALUES(406, 'FedEx', 2);

-- WORKS_WITH
INSERT INTO works_with VALUES(105, 400, 55000);
INSERT INTO works_with VALUES(102, 401, 267000);
INSERT INTO works_with VALUES(108, 402, 22500);
INSERT INTO works_with VALUES(107, 403, 5000);
INSERT INTO works_with VALUES(108, 403, 12000);
INSERT INTO works_with VALUES(105, 404, 33000);
INSERT INTO works_with VALUES(107, 405, 26000);
INSERT INTO works_with VALUES(102, 406, 15000);
INSERT INTO works_with VALUES(105, 406, 130000);
```
Let's start with some basic queries.

Find out the distinct branches that employees belong to.
```
mysql> SELECT DISTINCT branch_id FROM employee;
+-----------+
| branch_id |
+-----------+
|         1 |
|         2 |
|         3 |
+-----------+
```
##### Use of Functions

Use the COUNT function to find the total number of females in the employee table.
```
mysql> SELECT COUNT(*) from employee WHERE sex='F';
+----------+
| COUNT(*) |
+----------+
|        3 |
+----------+
```
Count how many of the employees have a super_id
```
mysql> SELECT COUNT(super_id) FROM employee;
+-----------------+
| COUNT(super_id) |
+-----------------+
|               8 |
+-----------------+
```

Find the number of males born after 1970
```
mysql> SELECT COUNT(*) FROM employee WHERE sex='M' AND birth_day >= '1971-01-01';
+----------+
| COUNT(*) |
+----------+
|        2 |
+----------+
```

Find average salary of employees
```
mysql> SELECT AVG(salary ) FROM employee;
+--------------+
| AVG(salary ) |
+--------------+
|   92888.8889 |
+--------------+
```
Find sum of salary of employees
```
mysql> SELECT SUM(salary) from employee;
+-------------+
| SUM(salary) |
+-------------+
|      836000 |
+-------------+
```

Find out total count of genders using GROUP BY
```
mysql> SELECT COUNT(sex), sex FROM employee GROUP BY sex;
+------------+------+
| COUNT(sex) | sex  |
+------------+------+
|          3 | F    |
|          6 | M    |
+------------+------+
```

Find out the total salary paid to different genders
```
mysql> SELECT SUM(salary),sex FROM employee GROUP BY sex;
+-------------+------+
| SUM(salary) | sex  |
+-------------+------+
|      228000 | F    |
|      608000 | M    |
+-------------+------+
```

Find total sales of each salseman
```
mysql> SELECT SUM(total_sales), emp_id FROM works_with GROUP BY emp_id;
+------------------+--------+
| SUM(total_sales) | emp_id |
+------------------+--------+
|           282000 |    102 |
|           218000 |    105 |
|            31000 |    107 |
|            34500 |    108 |
+------------------+--------+
```

##### Wildcards
Wildcards are used with the LIKE keyword to find matching strings (similar to regex).
% -> The '%' wildcard is used to specify any number of characters.
_ -> The '_' wildcard is used to specify a single occurence of a letter.

Find a company whose name ends in 'LLC'
```
mysql> SELECT * FROM client WHERE client_name LIKE '%LLC';
+-----------+--------------------+-----------+
| client_id | client_name        | branch_id |
+-----------+--------------------+-----------+
|       403 | John Daly Law, LLC |         3 |
+-----------+--------------------+-----------+
```

Find all employees born in February
```
mysql> SELECT * from employee WHERE birth_day LIKE '____-02-__';
+--------+------------+-----------+------------+------+--------+----------+-----------+
| emp_id | first_name | last_name | birth_day  | sex  | salary | super_id | branch_id |
+--------+------------+-----------+------------+------+--------+----------+-----------+
|    104 | Kelly      | Kapoor    | 1980-02-05 | F    |  55000 |      102 |         2 |
|    105 | Stanley    | Hudson    | 1958-02-19 | M    |  69000 |      102 |         2 |
+--------+------------+-----------+------------+------+--------+----------+-----------+

```
#### JOINS

Before proceeding let's add a new branch without any manager, and a employee without any branch.
```
mysql> INSERT INTO branch VALUES(4, 'Buffalo', NULL, NULL);
mysql> INSERT INTO employee VALUES(109, 'Bruno', 'Mars','1980-09-09', 'M', 50000, NULL, NULL);
```

Inner Join - A simple join that takes all rows that satisfy the condition. Below are some examples.
```
mysql> SELECT employee.emp_id, employee.first_name, branch.branch_name
    -> FROM employee
    -> JOIN branch
    -> ON employee.emp_id = branch.mgr_id;                                               
+--------+------------+-------------+
| emp_id | first_name | branch_name |
+--------+------------+-------------+
|    100 | David      | Corporate   |
|    102 | Michael    | Scranton    |
|    106 | Josh       | Stamford    |
+--------+------------+-------------+

mysql> SELECT employee.emp_id, employee.first_name, branch.branch_name
    -> FROM employee
    -> JOIN branch
    -> ON employee.branch_id = branch.branch_id;
+--------+------------+-------------+
| emp_id | first_name | branch_name |
+--------+------------+-------------+
|    100 | David      | Corporate   |
|    101 | Jan        | Corporate   |
|    102 | Michael    | Scranton    |
|    103 | Angela     | Scranton    |
|    104 | Kelly      | Scranton    |
|    105 | Stanley    | Scranton    |
|    106 | Josh       | Stamford    |
|    107 | Andy       | Stamford    |
|    108 | Jim        | Stamford    |
+--------+------------+-------------+
```

Left Join - It is used when you want to take all the rows from the left table (left table is the one you use after the 'FROM' keyword) even if they dont match any value in the right table.
```
mysql> SELECT employee.emp_id, employee.first_name, branch.branch_name                       
    -> FROM employee
    -> LEFT JOIN branch
    -> ON employee.branch_id = branch.branch_id;
+--------+------------+-------------+
| emp_id | first_name | branch_name |
+--------+------------+-------------+
|    100 | David      | Corporate   |
|    101 | Jan        | Corporate   |
|    102 | Michael    | Scranton    |
|    103 | Angela     | Scranton    |
|    104 | Kelly      | Scranton    |
|    105 | Stanley    | Scranton    |
|    106 | Josh       | Stamford    |
|    107 | Andy       | Stamford    |
|    108 | Jim        | Stamford    |
|    109 | Bruno      | NULL        |
+--------+------------+-------------+
```

Right Join - It is used when you want to take all the rows from the right table even if they dont match any value in the left table.
```
mysql> SELECT employee.emp_id, employee.first_name, branch.branch_name                       
    -> FROM employee
    -> RIGHT JOIN branch
    -> ON employee.branch_id = branch.branch_id;                                         
+--------+------------+-------------+
| emp_id | first_name | branch_name |
+--------+------------+-------------+
|    100 | David      | Corporate   |
|    101 | Jan        | Corporate   |
|    102 | Michael    | Scranton    |
|    103 | Angela     | Scranton    |
|    104 | Kelly      | Scranton    |
|    105 | Stanley    | Scranton    |
|    106 | Josh       | Stamford    |
|    107 | Andy       | Stamford    |
|    108 | Jim        | Stamford    |
|   NULL | NULL       | Buffalo     |
+--------+------------+-------------+
```

Full outer join - The combination of left and right join is called as full outer join. This functionality is not provided in MySQL.


##### Nested Queries

Find the First name of all the managers of all the branches
```
mysql> SELECT first_name
    -> FROM employee
    -> WHERE emp_id IN (
    ->     SELECT mgr_id FROM branch
    -> );
+------------+
| first_name |
+------------+
| David      |
| Michael    |
| Josh       |
+------------+
```
