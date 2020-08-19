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


```
```
```
```
```
```
```
```
```
```
