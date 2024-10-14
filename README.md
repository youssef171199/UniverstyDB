# SQL Database README

This repository contains SQL scripts and instructions for various database tasks.

## Task 1: Database Creation or Restoration

### Instructions:
1. Create a new database:<br>
CREATE DATABASE UniversityDB;

2. Create Courses table<br>
 CREATE TABLE Students( <br>
 StudentID INT PRIMARY KEY,<br>
 FirstName VARCHAR(50),<br>
 LastName VARCHAR(50),<br>
 DateOfBirth DATE,<br>
 Email VARCHAR(100)
);<br>
GO
3. Create Courses table
CREATE TABLE Courses (<br>
    CourseID INT PRIMARY KEY,<br>
    CourseName VARCHAR(100),<br>
    Department VARCHAR(50),
    Credits INT
);<br>
GO
4. Create Enrollments table<br>
CREATE TABLE Enrollments (<br>
    EnrollmentID INT PRIMARY KEY,<br>
    StudentID INT,<br>
    CourseID INT,<br>
    EnrollmentDate DATE,<br>
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),<br>
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);<br>
5. Locate the backup file named "YourBackupFile.bak" in the directory "D:\SQL BACKUP\YourBackupFile.bak".
6. Open SQL Server Management Studio (SSMS) and connect to your SQL Server instance.
7. Execute the SQL script :<br>
    <--Check if the database already exists and drop it if needed--><br>
    IF EXISTS (SELECT name FROM sys.databases WHERE name = 'UniversityDB')<br>
        BEGIN<br>
    ALTER DATABASE UniversityDB SET SINGLE_USER WITH ROLLBACK IMMEDIATE;<br>
    DROP DATABASE UniversityDB;<br>
    END<br>
    GO<br>
    <--Restore the database from the backup file--><br>
    RESTORE DATABASE UniversityDB<br>
    FROM DISK = 'D:\SQL BACKUP\YourBackupFile.bak'<br>
    WITH REPLACE, RECOVERY;<br>
    GO<br>

8. Replace the file path in the script with your actual backup file path.
9. This script will restore the "UniversityDB" database from the backup file.

## Task 2: Data Analysis

### Instructions:
1. Count of Students by Department:<br>
    SELECT CourseID, COUNT(*) AS NumberOfStudents<br>
    FROM Enrollments<br>
    GROUP BY CourseID;<br>
2. Total Enrollment by Year:<br>
    SELECT YEAR(EnrollmentDate) AS AcademicYear, COUNT(*) AS TotalEnrollment
    FROM Enrollments<br>
    GROUP BY YEAR(EnrollmentDate)<br>
    ORDER BY AcademicYear;<br>
3. Average GPA by Course:
    SELECT C.CourseID, C.CourseName, AVG(S.GPA) AS AverageGPA
    FROM Courses C<br>
    INNER JOIN Enrollments E ON C.CourseID = E.CourseID<br>
    INNER JOIN Students S ON E.StudentID = S.StudentID<br>
    GROUP BY C.CourseID, C.CourseName<br>
    ORDER BY C.CourseID;


## Task 3: Query (Breakdown, Rollup, or Cube)

### Instructions:
1. Breakdown by Department:
    SELECT C.Department, COUNT(*) AS EnrollmentCount<br>
    FROM Enrollments E<br>
    JOIN Courses C ON E.CourseID = C.CourseID<br>
    GROUP BY C.Department<br>
    ORDER BY EnrollmentCount DESC;<br>

2. Rollup by Year and Department:
    SELECT <br>
    CASE<br>
        WHEN YEAR(EnrollmentDate) IS NULL THEN '0'<br>
        ELSE YEAR(EnrollmentDate)<br>
    END AS AcademicYear,<br>
    Department,<br>
    COUNT(*) AS EnrollmentCount<br>
    FROM Enrollments E<br>
    INNER JOIN Students S ON E.StudentID = S.StudentID<br>
    INNER JOIN Courses C ON E.CourseID = C.CourseID<br>
    GROUP BY ROLLUP((YEAR(EnrollmentDate), Department))<br>
    ORDER BY AcademicYear, Department;

## Task 4: Result Reports with Charts or Graphs

### Instructions:
1. Power BI:
2. right click on the result grid 
3. Export query results to a CSV file using SQL Server Management Studio.

## Important Note:
- Ensure you have necessary permissions to perform these operations.
- Modify file paths, database names, and values as per your environment before executing SQL scripts.
- Always validate SQL queries before execution and handle sensitive data securely.
