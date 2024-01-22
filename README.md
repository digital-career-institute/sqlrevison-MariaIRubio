# DDL Commands:
# Create a Database:
Task: Create a new database named "SchoolDB" with an appropriate character set.
```sql
CREATE DATABASE SchoolDB;
```
# Create Tables:
# Teachers
```sql
CREATE TABLE Teachers(
	teacher_id INT PRIMARY KEY,
	teacher_name VARCHAR(255)
);


INSERT INTO Teachers (teacher_id, teacher_name)
VALUES
  (201, 'Mr. Johnson'),
  (202, 'Mrs. Smith'),
  (203, 'Mr. Davis'),
  (204, 'Miss Wilson'),
  (205, 'Mrs. Brown');

SELECT * FROM Teachers;
```
Task: Design and create a table named "Students" with the following columns:
# Classrooms:

classroom_id (Primary Key)
classroom_name
teacher_id
```sql
CREATE TABLE Classrooms(
	classroom_id INT PRIMARY KEY,
	classroom_name VARCHAR(255),
	teacher_id INT, 
	FOREIGN KEY(teacher_id) REFERENCES Teachers(teacher_id)
);


INSERT INTO Classrooms (classroom_id, classroom_name, teacher_id)
VALUES
  (101, 'Mathematics', 201),
  (102, 'English', 202),
  (103, 'Science', 203),
  (104, 'History', 204),
  (105, 'Art', 205);
  
 SELECT * FROM Classrooms;
```
#  Students:
student_id (Primary Key)
student_name
classroom_id (Foreign Key referencing classroom_id in the "Classrooms" table)
grade
```sql
CREATE TABLE Students (
	student_id INT PRIMARY KEY, 
	student_first_name VARCHAR(255),
	student_last_name VARCHAR(255),
	classroom_id INT,
	FOREIGN KEY(classroom_id) REFERENCES Classrooms(classroom_id)
);
```
# Modify Table Structure:

Task: Add a new column named "Gender" (VARCHAR, Maximum 10 characters) to the "Students" table.
```sql
ALTER TABLE Students ADD COLUMN Gender VARCHAR(10);
ALTER TABLE Students ADD COLUMN age INT;
SELECT * FROM Students;
```
# DML Commands:
Insert Data:

Task: Insert at least 5 records into the "Students" table.
```sql
INSERT INTO Students
VALUES
  (1, 'John', 'Smith', 101, 'Male', 8),
  (2, 'Emily', 'Johnson', 102, 'Female', 10),
  (3, 'Michael', 'Williams', 101, 'Male', 12),
  (4, 'Sophia', 'Jones', 103, 'Female', 14),
  (5, 'Daniel', 'Miller', 102, 'Male', 16),
  (6, 'Olivia', 'Brown', 104, 'Female', 9),
  (7, 'Matthew', 'Davis', 103, 'Male', 10),
  (8, 'Emma', 'Moore', 105, 'Female', 13),
  (9, 'William', 'Taylor', 104, 'Male', 15),
  (10, 'Ava', 'Anderson', 105, 'Female', 7);

SELECT * FROM Students;
```
# Update Records:

Task: Update the "Age" of students who are in the "Class" 10 to 16.
```sql
UPDATE Students SET age = 16 WHERE age = 10;

```
# Delete Records:

Task: Delete all records of students whose "Age" is less than 10.
```sql
DELETE FROM Students WHERE age < 10;

```
# DQL Command:
# Select Data:

Task: Write a SELECT query to retrieve the "FirstName," "LastName," and "Age" of all students in the "SchoolDB."
```sql
SELECT 
	student_first_name, student_last_name, age
FROM 
	Students;
```
# Aggregate Functions:

Task: Use aggregate functions to find the average age of all students.
```sql
SELECT 
	ROUND(AVG(age),0) AS average_age 
FROM 
	Students;
```
# Primary Key and Foreign Key:
# Define Primary Key:

Task: Explain the concept of a primary key and its significance. Also, ensure that the "StudentID" column in the "Students" table is a primary key.
```sql
Primary Key is the attribute that allows a value from each row to be unique to be identified in a table easily. It doesn't allow duplicates.
```
# Create Foreign Key:

Task: Create a new table named "Courses" with columns: "CourseID" (Integer, Primary Key) and "CourseName" (VARCHAR, Maximum 50 characters). Then, add a foreign key constraint in the "Students" table referencing the "CourseID" in the "Courses" table.
```sql
CREATE TABLE Courses (
	CourseID INT PRIMARY KEY,
	CourseName VARCHAR(50));
	
INSERT INTO Courses (CourseID, CourseName)
VALUES
  (301, 'Math 301'),
  (302, 'Math 302'), 
  (303, 'English 303'),
  (304, 'English 304'), 
  (305, 'Science 305'),
  (306, 'Science 306'), 
  (307, 'History 307'),
  (308, 'History 308'), 
  (309, 'Art 309'),
  (400, 'Art 400'); 

SELECT * FROM Courses;

ALTER TABLE Students ADD COLUMN CourseID INT;
ALTER TABLE Students ADD FOREIGN KEY(CourseID) REFERENCES Courses(CourseID);

UPDATE Students SET CourseID = 301 WHERE student_id = 1;
UPDATE Students SET CourseID = 302 WHERE student_id = 3;
UPDATE Students SET CourseID = 303 WHERE student_id = 2;
UPDATE Students SET CourseID = 304 WHERE student_id = 5;
UPDATE Students SET CourseID = 305 WHERE student_id = 4;
UPDATE Students SET CourseID = 306 WHERE student_id = 7;
UPDATE Students SET CourseID = 307 WHERE student_id = 6;
UPDATE Students SET CourseID = 308 WHERE student_id = 9;
UPDATE Students SET CourseID = 309 WHERE student_id = 8;
UPDATE Students SET CourseID = 400 WHERE student_id = 10;

SELECT * FROM Students;

```

Write SQL queries to perform the following tasks:

# List all students with their respective classrooms and teacher names.
```sql
SELECT 
	student_id, 
	student_first_name, 
	student_last_name, 
	classroom_name, 
	teacher_name
FROM 
	Students s
JOIN 
	Classrooms c ON s.classroom_id = c.classroom_id
JOIN 
	Teachers t ON c.teacher_id = t.teacher_id;
```
# Show the average grade for each classroom. Display the classroom name and the average grade.
```sql
SELECT 
	classroom_name, ROUND(AVG(age), 0) AS average_age 
FROM 
	Students s 
JOIN 
	Classrooms c ON s.classroom_id = c.classroom_id 
GROUP BY 
	classroom_name;
```
# Find the names of students who belong to a classroom taught by a specific teacher (you can choose a teacher_id).
```sql
SELECT 
	Student_first_name, 
	student_last_name, 
	teacher_id
FROM 
	Students s
JOIN 
	Classrooms c ON s.classroom_id = c.classroom_id
WHERE
	teacher_id IN (202,201,203);
```
# List all classrooms along with the number of students in each classroom.
```sql
SELECT 
	classroom_name, COUNT(s.classroom_id) AS total_students
FROM 
	Classrooms c
JOIN 
	Students s ON c.classroom_id = s.classroom_id
GROUP BY 
	classroom_name;
```
# Show the details of classrooms that have an average grade above a certain value (you can choose a specific grade).
```sql
SELECT c.classroom_id, classroom_name, ROUND(AVG(age),0) AS average_age
FROM 
	Classrooms c
JOIN 
	Students s ON c.classroom_id = s.classroom_id
GROUP BY
	c.classroom_id, c.classroom_name
HAVING 
	ROUND(AVG(age),0) >12;
```
