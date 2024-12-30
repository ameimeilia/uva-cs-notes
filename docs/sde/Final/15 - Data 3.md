---
layout: default
title: Data 3
parent: Final Notes
nav_order: 5
---
SQLite Driver dependency:  
[https://mvnrepository.com/artifact/org.xerial/sqlite-jdbc/3.47.0.](https://mvnrepository.com/artifact/org.xerial/sqlite-jdbc/3.47.0.0) 
Current version dependency string: (includes the dependency slf4j)
implementation 'org.xerial:sqlite-jdbc:3.47.0.0'

runtimeOnly 'org.slf4j:slf4j-api:2.0.16'

runtimeOnly 'org.slf4j:slf4j-simple:2.0.16'

# Foreign Keys
## Example Courses Database
### Students

```sqlite
CREATE TABLE IF NOT EXISTS Students (
    StudentId INTEGER PRIMARY KEY,
    FirstName TEXT,
    LastName TEXT NOT NULL,
    ComputingID TEXT UNIQUE NOT NULL
) STRICT;
```

- starting data of the table is as follows:

|StudentID|FirstName|LastName|ComputingID|
|---|---|---|---|
|1|Jonathan|Doe|abc2def|
|2|Jane|Smith|ghi3jkl|
### Courses

```sqlite
CREATE TABLE IF NOT EXISTS Courses (
    Crn INTEGER PRIMARY KEY,
    Subject TEXT NOT NULL,
    CourseNumber INTEGER NOT NULL,
    Section INTEGER NOT NULL,
    MeetingTime TEXT,
    UNIQUE (Subject, Section, CourseNumber)    -- combination of 3 must be unique
) STRICT;
```

- starting data of the table is as follows:

|CRN|Subject|CourseNumber|Section|MeetingTime|
|---|---|---|---|---|
|12345|CS|3140|1|14:00-15:15|

### Enrollments
- pairs StudentIDs and CourseIDs to track student enrollment
- this pattern is called a “Bridge Table” and is used to handle “many-to-many” connections:
	1. many students can enroll in 1 course  
	2. 1 student can enroll in many courses

```sqlite
CREATE TABLE Enrollments(
    EnrollmentID INTEGER PRIMARY KEY,
    StudentID INTEGER NOT NULL,
    CRN INTEGER NOT NULL,
    UNIQUE (StudentID, CRN),
    FOREIGN KEY (StudentID) REFERENCES Students (StudentID) ON DELETE CASCADE,
    FOREIGN KEY (CRN) REFERENCES Courses (CRN) ON DELETE CASCADE
) STRICT;
```

- starting data of the table is as follows:

|EnrollmentID|StudentID|CRN|
|---|---|---|
|1|1|12345|
|2|2|12345|
## `FOREIGN KEY`
- a **reference** from a record in one table to a record in, typically, another table
- want to enforce **data integrity**
- example: for every record in **Enrollments**, StudentID is a _real_ student and CRN is a _real_ course
### Foreign key syntax:

```sqlite
FOREIGN KEY (StudentID) REFERENCES Students (StudentID)
```

- in the **Enrollments** Table the field `Enrollments.StudentID` is referencing `Students.StudentsID` in the `Students` Table
- **every** `Enrollments.StudentID` for every record in our Enrollments table **must** exist in the `Students` table
- two columns having the same name immediately implies a foreign key relationship
### Enabling Foreign Keys in SQLite
- By default, foreign keys in SQLite **are not enforced**
- to **enable** foreign key enforcement **per connection**:

```sqlite
PRAGMA foreign_keys=ON;
```

- There is no “permanent” way to turn on foreign key enforcement for a database.
### ON DELETE CASCADE

```sqlite
FOREIGN KEY (StudentID) REFERENCES Students (StudentID) ON DELETE CASCADE,
```

- “If the referenced Student in the Students table is deleted, then _cascade_ that deletion to all records with that StudentID in Enrollments”.
- without `ON CASCADE DELETE`, an error will be thrown when deleting a record that is referenced in another table
# Joins
## Starting Database
**Students**

|StudentID|FirstName|LastName|ComputingID|
|---|---|---|---|
|1|John|Smith|abc2def|
|2|Debra|Jones|ghi3jk|
|3|Jack|Black|lmn4opq|
|4|Jack|White|rst5uvw|

**Courses**

| CRN   | Subject | CourseNumber | Section | MeetingTime       |
| ----- | ------- | ------------ | ------- | ----------------- |
| 12345 | CS      | 3140         | 1       | TR 14:00 - 15:15  |
| 54321 | CS      | 3140         | 2       | TR 15:30 - 16:45  |
| 98765 | APMA    | 2120         | 2       | MWF 12:00 - 12:50 |

**Enrollments**

|EnrollmentID|StudentID|CRN|
|---|---|---|
|1|1|12345|
|2|1|98765|
|3|2|54321|
|4|3|98765|
|5|3|54321|
|6|4|98765|
## Use Case
- “I want to get the names and computingIDs of all students in CS 3140, as well as the section they are enrolled in”
- Course’s table where that info is stored does not contain any information about what students are enrolled in the class.
- use `JOIN` to write as a single query
### Without Join

```sqlite
-- get the course registration numbers for CS 3140
SELECT Crn                                        
   FROM Courses                                    
   WHERE Subject = 'CS' AND CourseNumber = '3140';

-- get the StudentIDs for enrollment in first section
SELECT StudentID 
   FROM Enrollments
   WHERE Crn = 12345;     -- get 1

SELECT FirstName, LastName, ComputingID 
   FROM Students
   WHERE StudentID = 1;

FirstName  LastName  ComputingID
---------  --------  -----------
John       Smith     abc2def

-- get the StudentIDs for enrollment in second section
SELECT StudentID 
   FROM Enrollments
   WHERE Crn = 54321;    -- get 2, 3

SELECT FirstName, LastName, ComputingID
   FROM Students
   Where StudentID IN (2, 3);

FirstName  LastName  ComputingID
---------  --------  -----------
Debra      Jones     ghi3jk
Jack       Black     lmn4opq
```

### Example JOIN Query
- `JOIN` Courses and Enrollments table using the `Crn` attribute as the foreign key
- get `StudentId` from Enrollments and `Section` from Courses

```sqlite
SELECT Enrollments.StudentID, Courses.Section                                       
   FROM Enrollments                                           
   JOIN Courses ON Enrollments.Crn = Courses.Crn
   WHERE Courses.Subject = 'CS' AND Courses.CourseNumber = 3140;

StudentID  Section
---------  -------
1          1
2          2
3          2
```

### Multiple Joins
to get the names and computing IDs of the students:

```sqlite
SELECT Students.FirstName, Students.LastName, Students.ComputingID, Courses.Section   
   FROM Enrollments                                              
   JOIN Courses ON Enrollments.Crn = Courses.Crn
   JOIN Students ON Enrollments.StudentID = Students.StudentID
   WHERE Courses.Subject = 'CS' AND Courses.CourseNumber = 3140;

FirstName  LastName  ComputingID  Section
---------  --------  -----------  -------
John       Smith     abc2def      1
Debra      Jones     ghi3jk       2
Jack       Black     lmn4opq      2
```

## Types of Joins
### INNER Joins

```sqlite
FROM TableA JOIN TableB ON TableA.ColumnName = TableB.ColumnName
```

- used to **only** combine matching data when joining tables
### CROSS Join
- used to combine every record of one table with every record of another table
### LEFT OUTER JOIN

```sqlite
-- add a new section of CS 3140 that no student is enrolled in
INSERT INTO Courses (Subject, CourseNumber, Section, MeetingTime)
   VALUES('CS', 3140, 3, 'TR 12:30 - 13:45');

SELECT Courses.Section, Students.FirstName, Students.LastName, Students.ComputingID
  FROM Courses
  OUTER LEFT JOIN Enrollments ON Courses.Crn == Enrollments.Crn
  OUTER LEFT JOIN Students ON Enrollments.StudentID == Students.StudentID
  WHERE Courses.Subject = 'CS' and Courses.CourseNumber = 3140;

Section  FirstName  LastName  ComputingID
-------  ---------  --------  -----------
1        John       Smith     abc2def
2        Debra      Jones     ghi3jk
2        Jack       Black     lmn4opq
3
-- section 3 would not show up using an INNER JOIN
-- LEFT OUTER JOIN guarantees at least one record for each record in TableA
```
