# 📘 Student Database Management System

## 📌 Introduction

This project presents the design and implementation of a **Student Database Management System** using SQL concepts. It is based on the topics covered in the syllabus and demonstrates how to manage student academic records efficiently.

The system includes:

* A **Student table** for storing academic data
* A **Classroom table** for managing class assignments

It covers SQL concepts from basic queries to advanced database programming like **stored procedures and triggers**.
<img width="1337" height="536" alt="Screenshot 2026-03-20 182337" src="https://github.com/user-attachments/assets/5aae9f66-9a3a-40f5-84a5-48781b977e14" />

---

## 🎯 Project Objectives

* Design a relational database for student records
* Implement SQL commands (DDL, DML, DQL, DCL, TCL)
* Ensure data integrity using constraints
* Perform complex queries using joins, subqueries, and aggregate functions
* Use advanced SQL features like window functions
* Automate operations using stored procedures and triggers

---

## 🗂️ Database Design

### 📊 Entities

* **Student** – Stores student details and marks
* **Classroom** – Stores classroom allocation

### 🔗 Relationship

* One classroom can have many students
* One student belongs to one or no classroom

---

## 🏗️ Table Structure (DDL)

```sql
CREATE DATABASE mark_list;
USE mark_list;

CREATE TABLE student (
    student_id VARCHAR(10) PRIMARY KEY,
    student_name CHAR(20) NOT NULL,
    tamil INT,
    english INT,
    maths INT,
    total_mark INT,
    percentage INT
);

CREATE TABLE classroom (
    student_id VARCHAR(10),
    class_name VARCHAR(20),
    FOREIGN KEY (student_id) REFERENCES student(student_id)
);
```

---

## ✏️ Data Operations

### ➤ Insert Data

```sql
INSERT INTO student (student_id, student_name, tamil, english, maths) VALUES
("AUG001", "ABI", 70, 80, 90),
("AUG002", "CHRIS", 79, 89, 89),
("AUG008", "Subathara", 89, 45, 67);

INSERT INTO classroom VALUES
("AUG001", "X- A"),
("AUG002", "X- B");
```

### ➤ Update Data

```sql
UPDATE student SET total_mark = tamil + english + maths;
UPDATE student SET percentage = total_mark / 3;
```

---

## 🔍 Data Querying

### ➤ Filtering

```sql
SELECT * FROM student WHERE percentage < 50;
SELECT * FROM student WHERE student_name LIKE 'A%';
SELECT * FROM student WHERE student_name IN ('ABI', 'CHRIS');
SELECT * FROM student WHERE tamil BETWEEN 60 AND 80;
```

### ➤ Joins

```sql
-- INNER JOIN
SELECT s.student_id, s.student_name, c.class_name
FROM student s
INNER JOIN classroom c ON s.student_id = c.student_id;

-- LEFT JOIN
SELECT s.student_id, s.student_name, c.class_name
FROM student s
LEFT JOIN classroom c ON s.student_id = c.student_id;
```

---

## 🚀 Advanced SQL Concepts

### 📈 Aggregate Functions

```sql
SELECT AVG(total_mark) AS avg_total_marks FROM student;

SELECT c.class_name, COUNT(*) AS total_students
FROM classroom c
GROUP BY c.class_name;
```

### 🔁 Subquery

```sql
SELECT student_name, tamil
FROM student
WHERE tamil > (SELECT AVG(tamil) FROM student);
```

### 🪟 Window Functions

```sql
SELECT student_id, student_name, total_mark,
RANK() OVER (ORDER BY total_mark DESC) AS rank_no
FROM student;

SELECT student_id, student_name, total_mark,
SUM(total_mark) OVER (ORDER BY student_id) AS running_total
FROM student;
```

---

## ⚙️ Database Programming

### 🔧 Stored Procedure

```sql
DELIMITER //

CREATE PROCEDURE calculate_student_percentage(
    IN student_id VARCHAR(10),
    INOUT total_marks INT,
    OUT percentage DECIMAL(5,2)
)
BEGIN
    DECLARE tamil INT;
    DECLARE english INT;
    DECLARE maths INT;

    SELECT tamil, english, maths 
    INTO tamil, english, maths
    FROM student 
    WHERE student.student_id = student_id;

    SET total_marks = tamil + english + maths;
    SET percentage = total_marks / 3.0;
END //

DELIMITER ;
```

---

### ⚡ Triggers

#### BEFORE INSERT Trigger

```sql
DELIMITER //

CREATE TRIGGER before_insert_student
BEFORE INSERT ON student
FOR EACH ROW
BEGIN
    IF NEW.tamil < 0 THEN
        SET NEW.tamil = 0;
    END IF;
END //

DELIMITER ;
```

#### AFTER UPDATE Trigger

```sql
DELIMITER //

CREATE TRIGGER after_update_student
AFTER UPDATE ON student
FOR EACH ROW
BEGIN
    INSERT INTO update_log (student_id, old_tamil, new_tamil, update_time)
    VALUES (OLD.student_id, OLD.tamil, NEW.tamil, NOW());
END //

DELIMITER ;
```

---

## ✅ Conclusion

This project demonstrates a complete implementation of a **Student Database Management System** using SQL.

### Key Learnings:

* ✔ Database design with constraints
* ✔ Data insertion, updating, and querying
* ✔ Use of joins, subqueries, and aggregate functions
* ✔ Advanced concepts like window functions
* ✔ Automation using stored procedures and triggers

---

## 📌 Future Enhancements

* Add a **User Interface (UI)**
* Integrate with a web application
* Add login/authentication system
* Expand to include more subjects and reports








