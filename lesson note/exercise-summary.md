# SQL Practice Exercises

This repository contains a series of SQL exercises designed to test and improve your database querying skills. The exercises cover fundamental to advanced topics including simple queries, joins, aggregate functions, subqueries, set operators, and views.

## Table of Contents
- [Exercise 1: Library System](#exercise-1-library-system)
- [Exercise 2: Course Enrollment System](#exercise-2-course-enrollment-system)
- [Exercise 3: Set Operators & Views](#exercise-3-set-operators--views)
- [Exercise 4: School Sports Events](#exercise-4-school-sports-events)

---

## Exercise 1: Library System

### Database Schema and Data

**`Member` Table**
| Member_ID | Name | JoinDate |
| :--- | :--- | :--- |
| M0001 | Alice Wong | 2023-01-05 |
| M0002 | Ben Chan | 2023-02-10 |
| M0003 | Cathy Lee | 2023-03-12 |
| M0004 | David Lam | 2023-03-25 |
| M0005 | Emily Ng | 2023-04-08 |
| M0006 | Frank Ho | 2023-05-16 |
| M0007 | Grace Cheung| 2023-06-01 |
| M0008 | Henry Lee | 2023-06-22 |
| M0009 | Ivy Tang | 2023-07-10 |
| M0010 | Jack Lau | 2023-08-02 |

**`Book` Table**
| Book_ID | Title | Author |
| :--- | :--- | :--- |
| B001 | The Art of AI | John Smith |
| B002 | Python Basics | Laura Ng |
| B003 | Data Science 101 | Michael Chan |
| B004 | Learning SQL | Peter Wong |
| B005 | The Cloud Era | Angela Lee |
| B006 | Introduction to Robotics | David Ho |
| B007 | Creative Coding | Sarah Lam |
| B008 | Digital Photography | Chris Cheung |
| B009 | Cybersecurity Fundamentals | Raymond Kwok |
| B010 | Machine Learning Made Easy | Rachel Lee |
| B011 | Introduction to Databases | Pauline Yip |
| B012 | Understanding Algorithms | Ken Lau |
| B013 | Tech Entrepreneurship | Amy Wong |
| B014 | Blockchain Simplified | Nelson Cheng |
| B015 | AI and Society | Winnie Ng |

**`Borrow` Table**
| Member_ID | Book_ID | BorrowDate |
| :--- | :--- | :--- |
| M0001 | B001 | 2024-01-10 |
| M0001 | B003 | 2024-01-15 |
| M0002 | B002 | 2024-02-05 |
| M0002 | B010 | 2024-02-15 |
| M0003 | B005 | 2024-03-01 |
| M0003 | B011 | 2024-03-07 |
| M0004 | B006 | 2024-03-20 |
| M0005 | B015 | 2024-04-08 |
| M0006 | B007 | 2024-04-20 |
| M0006 | B008 | 2024-05-05 |
| M0007 | B012 | 2024-05-15 |
| M0008 | B014 | 2024-06-10 |
| M0009 | B001 | 2024-06-18 |
| M0010 | B002 | 2024-07-05 |
| M0010 | B003 | 2024-07-12 |
| M0004 | B010 | 2024-07-25 |
| M0005 | B011 | 2024-08-03 |
| M0007 | B005 | 2024-08-12 |
| M0008 | B015 | 2024-08-20 |
| M0009 | B006 | 2024-08-29 |

### Questions and Answers

#### Part 1 — Simple SQL Queries
1.  **List all books.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT * FROM Book;
    ```
    **Note:** This query selects all columns and all rows from the `Book` table.
    </details>

2.  **Display the names of all members who joined in 2023.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Name
    FROM Member
    WHERE JoinDate >= '2023-01-01' AND JoinDate <= '2023-12-31';
    
    -- Alternative using EXTRACT (more portable)
    -- SELECT Name FROM Member WHERE EXTRACT(YEAR FROM JoinDate) = 2023;
    ```
    **Note:** This query filters the `Member` table to find records where the `JoinDate` falls within the year 2023.
    </details>

3.  **Show the titles of books written by ‘Rachel Lee’.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Title
    FROM Book
    WHERE Author = 'Rachel Lee';
    ```
    </details>

4.  **Find the book ID of books borrowed after 1 July 2024.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT DISTINCT Book_ID
    FROM Borrow
    WHERE BorrowDate > '2024-07-01';
    ```
    **Note:** `DISTINCT` is used to ensure each `Book_ID` appears only once, even if it was borrowed multiple times after the specified date.
    </details>

#### Part 2 — Join Tables
5.  **List the titles of all books together with the member names who borrowed them.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT b.Title, m.Name
    FROM Borrow br
    JOIN Book b ON br.Book_ID = b.Book_ID
    JOIN Member m ON br.Member_ID = m.Member_ID;
    ```
    **Note:** This requires an `INNER JOIN` across all three tables to link books to borrows and borrows to members.
    </details>

6.  **Show all book titles and borrower names, including books not yet borrowed.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT b.Title, m.Name
    FROM Book b
    LEFT JOIN Borrow br ON b.Book_ID = br.Book_ID
    LEFT JOIN Member m ON br.Member_ID = m.Member_ID;
    ```
    **Note:** A `LEFT JOIN` starting from the `Book` table ensures that all books are listed, even if they have no matching entry in the `Borrow` table. The member's name will be `NULL` for unborrowed books.
    </details>

7.  **Display each member and the number of books they borrowed.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT m.Name, COUNT(br.Book_ID) AS NumberOfBooksBorrowed
    FROM Member m
    LEFT JOIN Borrow br ON m.Member_ID = br.Member_ID
    GROUP BY m.Member_ID, m.Name
    ORDER BY NumberOfBooksBorrowed DESC;
    ```
    **Note:** A `LEFT JOIN` ensures all members are listed, even those who haven't borrowed any books (their count will be 0). We `GROUP BY` member to count the books for each one.
    </details>

#### Part 3 — Aggregate Functions
8.  **Count how many books are in the library.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT COUNT(*) AS TotalBooks
    FROM Book;
    ```
    </details>

9.  **Find the earliest and latest borrow dates.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT MIN(BorrowDate) AS EarliestDate, MAX(BorrowDate) AS LatestDate
    FROM Borrow;
    ```
    </details>

10. **Show how many distinct members have borrowed at least one book.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT COUNT(DISTINCT Member_ID) AS NumberOfBorrowers
    FROM Borrow;
    ```
    </details>

11. **List each author and how many of their books have been borrowed.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT bk.Author, COUNT(br.Book_ID) AS BorrowCount
    FROM Book bk
    JOIN Borrow br ON bk.Book_ID = br.Book_ID
    GROUP BY bk.Author
    ORDER BY BorrowCount DESC;
    ```
    **Note:** This joins `Book` and `Borrow` tables, groups by author, and then counts the number of borrows for each author's books.
    </details>

#### Part 4 — Subqueries
12. **Find members who borrowed the book with ID 'B001'.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Name
    FROM Member
    WHERE Member_ID IN (SELECT Member_ID FROM Borrow WHERE Book_ID = 'B001');
    ```
    </details>

13. **List books that have never been borrowed.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Title
    FROM Book
    WHERE Book_ID NOT IN (SELECT DISTINCT Book_ID FROM Borrow);
    ```
    </details>

14. **Show members who borrowed more than one book.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Name
    FROM Member
    WHERE Member_ID IN (
        SELECT Member_ID
        FROM Borrow
        GROUP BY Member_ID
        HAVING COUNT(Book_ID) > 1
    );
    ```
    </details>

15. **Find the titles of books borrowed by 'Alice Wong'.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Title
    FROM Book
    WHERE Book_ID IN (
        SELECT Book_ID
        FROM Borrow
        WHERE Member_ID = (SELECT Member_ID FROM Member WHERE Name = 'Alice Wong')
    );
    ```
    </details>

---

## Exercise 2: Course Enrollment System

### Database Schema and Data

**`Student` Table**
| StudentID | Name | GradeLevel | JoinedOn |
| :--- | :--- | :--- | :--- |
| S1001 | Alice Wong | 10 | 2024-09-02 |
| S1002 | Ben Chan | 11 | 2024-09-02 |
| S1003 | Cathy Lee | 10 | 2024-09-03 |
| S1004 | David Lam | 12 | 2024-08-30 |
| S1005 | Emily Ng | 9 | 2024-09-04 |
| S1006 | Frank Ho | 11 | 2024-09-01 |
| S1007 | Grace Cheung| 9 | 2024-09-05 |
| S1008 | Henry Lee | 12 | 2024-08-29 |
| S1009 | Ivy Tang | 10 | 2024-09-03 |
| S1010 | Jack Lau | 11 | 2024-09-02 |

**`Course` Table**
| CourseID | Title | Category | Difficulty |
| :--- | :--- | :--- | :--- |
| C2001 | Intro to Python | Programming | Beginner |
| C2002 | Web Design Basics | Design | Beginner |
| C2003 | Data Analysis 101 | Data | Beginner |
| C2004 | JavaScript Essentials | Programming | Intermediate|
| C2005 | Database Fundamentals | Data | Beginner |
| C2006 | Cybersecurity Awareness | Security | Beginner |
| C2007 | UI/UX Principles | Design | Intermediate|
| C2008 | Networks & Routing | Infrastructure| Intermediate|
| C2009 | Machine Learning Intro| Data | Intermediate|
| C2010 | Mobile App Prototyping| Design | Beginner |
| C2011 | Cloud Computing Basics | Infrastructure| Beginner |
| C2012 | Algorithms & Logic | Programming | Advanced |

**`Enroll` Table**
| StudentID | CourseID | EnrolledOn |
| :--- | :--- | :--- |
| S1001 | C2001 | 2024-09-10 |
| S1001 | C2003 | 2024-09-12 |
| S1002 | C2002 | 2024-09-11 |
| S1002 | C2004 | 2024-09-15 |
| S1003 | C2005 | 2024-09-13 |
| S1003 | C2011 | 2024-09-20 |
| S1004 | C2006 | 2024-09-14 |
| S1005 | C2010 | 2024-09-18 |
| S1005 | C2007 | 2024-09-22 |
| S1006 | C2008 | 2024-09-19 |
| S1006 | C2009 | 2024-09-25 |
| S1007 | C2003 | 2024-09-21 |
| S1008 | C2011 | 2024-09-23 |
| S1008 | C2009 | 2024-09-27 |
| S1009 | C2001 | 2024-09-16 |
| S1010 | C2002 | 2024-09-17 |
| S1010 | C2003 | 2024-09-24 |
| S1010 | C2004 | 2024-09-28 |

### Questions and Answers

#### Part 1 — Simple SQL
1.  **List all students who joined on or after 2024-09-02 (show Name and JoinedOn).**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Name, JoinedOn
    FROM Student
    WHERE JoinedOn >= '2024-09-02';
    ```
    </details>

2.  **Show the titles of courses in the 'Data' category.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Title
    FROM Course
    WHERE Category = 'Data';
    ```
    </details>

3.  **Display CourseID values that were enrolled on 2024-09-23.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT CourseID
    FROM Enroll
    WHERE EnrolledOn = '2024-09-23';
    ```
    </details>

4.  **List all courses whose Title contains the word 'Intro'.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Title
    FROM Course
    WHERE Title LIKE '%Intro%';
    ```
    **Note:** The `%` is a wildcard character that matches any sequence of characters.
    </details>

#### Part 2 — Join Tables
5.  **Show each enrolled course Title and the Student Name (inner join).**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT c.Title, s.Name
    FROM Enroll e
    JOIN Course c ON e.CourseID = c.CourseID
    JOIN Student s ON e.StudentID = s.StudentID;
    ```
    </details>

6.  **List all courses and the names of students enrolled, including courses with no enrollments (left join from Course).**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT c.Title, s.Name
    FROM Course c
    LEFT JOIN Enroll e ON c.CourseID = e.CourseID
    LEFT JOIN Student s ON e.StudentID = s.StudentID;
    ```
    </details>

7.  **For each student, display Name and the number of courses enrolled (0 for none). Order by the count descending.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT s.Name, COUNT(e.CourseID) AS CoursesEnrolled
    FROM Student s
    LEFT JOIN Enroll e ON s.StudentID = e.StudentID
    GROUP BY s.StudentID, s.Name
    ORDER BY CoursesEnrolled DESC;
    ```
    </details>

#### Part 3 — Aggregate Functions
8.  **Count the total number of courses.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT COUNT(*) AS TotalCourses
    FROM Course;
    ```
    </details>

9.  **Show the earliest and latest EnrolledOn dates.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT MIN(EnrolledOn) AS EarliestEnrollment, MAX(EnrolledOn) AS LatestEnrollment
    FROM Enroll;
    ```
    </details>

10. **Count how many distinct students have at least one enrollment.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT COUNT(DISTINCT StudentID) AS DistinctEnrolledStudents
    FROM Enroll;
    ```
    </details>

11. **For each Category, show how many enrollments exist (0 if none).**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT c.Category, COUNT(e.StudentID) AS EnrollmentCount
    FROM Course c
    LEFT JOIN Enroll e ON c.CourseID = e.CourseID
    GROUP BY c.Category
    ORDER BY EnrollmentCount DESC;
    ```
    </details>

#### Part 4 — Subquery
12. **List Names of students who enrolled in course 'C2001'. (Use IN with a subquery.)**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Name
    FROM Student
    WHERE StudentID IN (SELECT StudentID FROM Enroll WHERE CourseID = 'C2001');
    ```
    </details>

13. **Show Titles of courses that have never been enrolled. (Use NOT IN with a subquery.)**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Title
    FROM Course
    WHERE CourseID NOT IN (SELECT DISTINCT CourseID FROM Enroll);
    ```
    </details>

14. **List Names of students who enrolled in more than one course. (Use a single subquery with GROUP BY/HAVING.)**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Name
    FROM Student
    WHERE StudentID IN (
        SELECT StudentID
        FROM Enroll
        GROUP BY StudentID
        HAVING COUNT(CourseID) > 1
    );
    ```
    </details>

15. **Show Titles of courses taken by 'Alice Wong'. (Use a single subquery.)**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT Title
    FROM Course
    WHERE CourseID IN (
        SELECT CourseID
        FROM Enroll
        WHERE StudentID = (SELECT StudentID FROM Student WHERE Name = 'Alice Wong')
    );
    ```
    </details>

#### Bonus
16. **For each student, show Name, total courses enrolled, and the most recent EnrolledOn date (use join + GROUP BY).**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    SELECT
        s.Name,
        COUNT(e.CourseID) AS TotalCoursesEnrolled,
        MAX(e.EnrolledOn) AS MostRecentEnrollment
    FROM Student s
    LEFT JOIN Enroll e ON s.StudentID = e.StudentID
    GROUP BY s.StudentID, s.Name
    ORDER BY s.Name;
    ```
    **Note:** This query aggregates enrollment data for each student, providing both a count of their courses and their latest enrollment date. A `LEFT JOIN` is used to include students with no enrollments.
    </details>
    
---

## Exercise 3: Set Operators & Views

### Database Schema and Data

**`Student` Table**
| SID | Name | GradeLevel |
| :--- | :--- | :--- |
| 24001| Chan Tsz Hin | 10 |
| 24002| Lee Ka Man | 10 |
| 24003| Wong Hoi Yan | 11 |
| 24004| Ng Chun Kit | 11 |
| 24005| Cheung Wing | 12 |
| 24006| Ho Nga Ting | 12 |
| 24007| Au Cheuk Hin | 11 |
| 24008| Yip Lok Hei | 10 |
| 24009| Lau Pui Yan | 12 |
| 24010| Fong Tze Long| 9 |

**`Course` Table**
| Course_ID | Title | Category |
| :--- | :--- | :--- |
| ICT1 | ICT Fundamentals| ICT |
| AI01 | Intro to AI | AI |
| M101 | Core Mathematics| Math |
| PHY1 | Physics Basics | Science|
| CS01 | Python Basics | CS |

**`Enrol` Table**
| SID | Course_ID | EnrolDate |
| :--- | :--- | :--- |
| 24001| ICT1 | *current_date* |
| 24001| AI01 | *current_date* |
| 24002| ICT1 | *current_date* |
| 24002| M101 | *current_date* |
| 24003| AI01 | *current_date* |
| 24003| M101 | *current_date* |
| 24004| ICT1 | *current_date* |
| 24004| AI01 | *current_date* |
| 24005| M101 | *current_date* |
| 24005| PHY1 | *current_date* |
| 24006| ICT1 | *current_date* |
| 24006| CS01 | *current_date* |
| 24007| AI01 | *current_date* |
| 24007| CS01 | *current_date* |
| 24008| ICT1 | *current_date* |
| 24008| PHY1 | *current_date* |
| 24009| AI01 | *current_date* |
| 24009| ICT1 | *current_date* |
| 24010| M101 | *current_date* |
| 24010| PHY1 | *current_date* |

### Views
*   `v_ict_students`: Shows students enrolled in 'ICT1'.
*   `v_ai_students`: Shows students enrolled in 'AI01'.
*   `v_senior_students`: Shows students in Grade 11 or higher.
*   `v_course_enrol_counts`: Shows the count of enrolled students for each course.

### Questions and Answers

1.  **List student Names who are in ICT or AI (unique).**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    -- Using the views
    SELECT s.Name
    FROM Student s
    WHERE s.SID IN (
        SELECT SID FROM v_ict_students
        UNION
        SELECT SID FROM v_ai_students
    );
    ```
    **Note:** This query uses `UNION` to combine the lists of student IDs from the two views, automatically removing duplicates. It then joins back to the `Student` table to get their names.
    </details>

2.  **Show the overlap (students in both ICT and AI) with Names and GradeLevel.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    -- Using the views
    SELECT s.Name, s.GradeLevel
    FROM Student s
    WHERE s.SID IN (
        SELECT SID FROM v_ict_students
        INTERSECT
        SELECT SID FROM v_ai_students
    );
    ```
    **Note:** `INTERSECT` is perfect for finding the common records between two queries. Here, it finds the `SID`s that exist in both views.
    </details>

3.  **Show seniors (GradeLevel ≥ 11) who are NOT taking AI.**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    -- Note: MINUS is Oracle-specific. EXCEPT is the ANSI standard.
    -- Using EXCEPT (Standard SQL)
    SELECT Name, GradeLevel FROM v_senior_students
    EXCEPT
    SELECT Name, GradeLevel FROM v_ai_students;
    
    -- Using MINUS (Oracle)
    -- SELECT Name, GradeLevel FROM v_senior_students
    -- MINUS
    -- SELECT Name, GradeLevel FROM v_ai_students;
    ```
    **Note:** The `EXCEPT` (or `MINUS`) operator returns records from the first query that are not present in the second query.
    </details>

4.  **Create a new VIEW that lists all students and how many courses each student takes (SID, Name, Count).**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    CREATE VIEW v_student_course_counts AS
    SELECT s.SID, s.Name, COUNT(e.Course_ID) AS CourseCount
    FROM Student s
    LEFT JOIN Enrol e ON s.SID = e.SID
    GROUP BY s.SID, s.Name;
    
    -- You can then query it like a table:
    -- SELECT * FROM v_student_course_counts ORDER BY CourseCount DESC;
    ```
    **Note:** This view simplifies the process of checking how many courses any student is enrolled in. The `LEFT JOIN` ensures all students are included, even if they have 0 enrollments.
    </details>

5.  **Create a VIEW that exposes only SID and Name from Student (security-focused public view).**
    <details>
    <summary>Show Answer</summary>
    
    ```sql
    CREATE VIEW v_public_student_list AS
    SELECT SID, Name
    FROM Student;
    
    -- You can then query it:
    -- SELECT * FROM v_public_student_list;
    ```
    **Note:** This is a common security practice. The view acts as an abstraction layer, hiding sensitive or unnecessary columns (like `GradeLevel`) from certain database users.
    </details>
    
---

## Exercise 4: School Sports Events

### Database Schema and Data

**`Student` Table**
| StudentID | Name | Class | JoinedOn |
| :--- | :--- | :--- | :--- |
| ST1001 | Alice Wong | S3A | 2024-09-02 |
| ST1002 | Ben Chan | S3B | 2024-09-02 |
| ST1003 | Cathy Lee | S4A | 2024-09-03 |
| ST1004 | David Lam | S5C | 2024-08-30 |
| ST1005 | Emily Ng | S2A | 2024-09-04 |
| ST1006 | Frank Ho | S5A | 2024-09-01 |
| ST1007 | Grace Cheung| S2B | 2024-09-05 |
| ST1008 | Henry Lee | S6A | 2024-08-29 |
| ST1009 | Ivy Tang | S4B | 2024-09-03 |
| ST1010 | Jack Lau | S3C | 2024-09-02 |

**`SportEvent` Table**
| EventID | EventName | Category | EventDate | Location |
| :--- | :--- | :--- | :--- | :--- |
| E3001 | 100m Sprint | Track | 2024-10-12 | Main Stadium |
| E3002 | 200m Sprint | Track | 2024-10-12 | Main Stadium |
| E3003 | 4x100m Relay | Track | 2024-10-13 | Main Stadium |
| E3004 | Long Jump | Field | 2024-10-14 | Field A |
| E3005 | High Jump | Field | 2024-10-14 | Field A |
| E3006 | Shot Put | Field | 2024-10-15 | Field B |
| E3007 | Basketball 3v3 | Ball | 2024-10-16 | Gym Court 1|
| E3008 | Table Tennis Singles| Ball | 2024-10-16 | Hall |
| E3009 | Badminton Doubles | Ball | 2024-10-17 | Hall |
| E3010 | Swimming 50m Freestyle | Aquatic | 2024-10-18 | Aquatic Center|
| E3011 | Swimming 100m Breaststroke| Aquatic | 2024-10-18 | Aquatic Center|
| E3012 | Cross Country 5km | Track | 2024-10-20 | Country Park|

**`Enrolled` Table**
| StudentID | EventID | RegisteredOn |
| :--- | :--- | :--- |
| ST1001 | E3001 | 2024-09-20 |
| ST1001 | E3003 | 2024-09-22 |
| ST1002 | E3002 | 2024-09-21 |
| ST1002 | E3007 | 2024-09-25 |
| ST1003 | E3004 | 2024-09-23 |
| ST1003 | E3010 | 2024-09-29 |
| ST1004 | E3006 | 2024-09-24 |
| ST1005 | E3008 | 2024-09-26 |
| ST1005 | E3009 | 2024-09-28 |
| ST1006 | E3011 | 2024-09-27 |
| ST1006 | E3005 | 2024-10-01 |
| ST1007 | E3003 | 2024-09-30 |
| ST1008 | E3010 | 2024-10-02 |
| ST1008 | E3012 | 2024-10-05 |
| ST1009 | E3001 | 2024-10-03 |
| ST1010 | E3002 | 2024-10-04 |
| ST1010 | E3003 | 2024-10-06 |
| ST1010 | E3007 | 2024-10-07 |

### Questions and Answers

#### Part 1 — Simple SQL
1.  **List all students in class 'S3A' (show StudentID and Name).**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT StudentID, Name
    FROM Student
    WHERE Class = 'S3A';
    ```
    </details>

2.  **Show the names of events in the 'Track' category.**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT EventName
    FROM SportEvent
    WHERE Category = 'Track';
    ```
    </details>

3.  **Display EventID values that students registered for on 2024-10-03.**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT EventID
    FROM Enrolled
    WHERE RegisteredOn = '2024-10-03';
    ```
    </details>

4.  **List all events whose EventName contains the word 'Swim'.**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT EventName
    FROM SportEvent
    WHERE EventName LIKE '%Swim%';
    ```
    </details>

#### Part 2 — Join Tables
5.  **Show each enrolled EventName and the Student Name (inner join across tables).**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT se.EventName, s.Name
    FROM Enrolled e
    JOIN SportEvent se ON e.EventID = se.EventID
    JOIN Student s ON e.StudentID = s.StudentID;
    ```
    </details>

6.  **List all events and student names, including events with no enrollments (left join from SportEvent).**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT se.EventName, s.Name
    FROM SportEvent se
    LEFT JOIN Enrolled e ON se.EventID = e.EventID
    LEFT JOIN Student s ON e.StudentID = s.StudentID;
    ```
    </details>

7.  **For each student, display Name and the number of events enrolled (0 for none). Order by the count descending.**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT s.Name, COUNT(e.EventID) AS EventsEnrolled
    FROM Student s
    LEFT JOIN Enrolled e ON s.StudentID = e.StudentID
    GROUP BY s.StudentID, s.Name
    ORDER BY EventsEnrolled DESC;
    ```
    </details>

#### Part 3 — Aggregate Functions
8.  **Count the total number of sport events.**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT COUNT(*) AS TotalSportEvents
    FROM SportEvent;
    ```
    </details>

9.  **Show the earliest and latest RegisteredOn dates.**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT MIN(RegisteredOn) AS EarliestRegistration, MAX(RegisteredOn) AS LatestRegistration
    FROM Enrolled;
    ```
    </details>

10. **Count how many distinct students have at least one enrollment.**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT COUNT(DISTINCT StudentID) AS DistinctEnrolledStudents
    FROM Enrolled;
    ```
    </details>

11. **For each Category, show how many enrollments exist (0 if none).**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT se.Category, COUNT(e.StudentID) AS EnrollmentCount
    FROM SportEvent se
    LEFT JOIN Enrolled e ON se.EventID = e.EventID
    GROUP BY se.Category
    ORDER BY EnrollmentCount DESC;
    ```
    </details>

#### Part 4 — Subquery (Single Subquery Only)
12. **List Names of students who enrolled in event 'E3001'. (Use IN with a subquery.)**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT Name
    FROM Student
    WHERE StudentID IN (SELECT StudentID FROM Enrolled WHERE EventID = 'E3001');
    ```
    </details>

13. **Show EventNames for events that have never been enrolled. (Use NOT IN with a subquery.)**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT EventName
    FROM SportEvent
    WHERE EventID NOT IN (SELECT DISTINCT EventID FROM Enrolled);
    ```
    </details>

14. **List Names of students who enrolled in more than one event. (Use a single subquery with GROUP BY/HAVING.)**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT Name
    FROM Student
    WHERE StudentID IN (
        SELECT StudentID
        FROM Enrolled
        GROUP BY StudentID
        HAVING COUNT(EventID) > 1
    );
    ```
    </details>

15. **Show EventNames taken by 'Alice Wong'. (Use a single subquery.)**
    <details>
    <summary>Show Answer</summary>

    ```sql
    SELECT EventName
    FROM SportEvent
    WHERE EventID IN (
        SELECT EventID
        FROM Enrolled
        WHERE StudentID = (SELECT StudentID FROM Student WHERE Name = 'Alice Wong')
    );
    ```
    </details>