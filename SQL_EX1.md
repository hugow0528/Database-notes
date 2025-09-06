

### **Part 1: Data Definition Language (DDL)**
This section tests your ability to create and manage database structures.

**Question 1: Database and Table Creation**

*   **Scenario:** You are tasked with creating a new database for a university. Inside this database, you need to create a `Students` table and a `Courses` table.
*   **Database to be Created:** `UniversityDB`
*   **`Students` Table Structure:**
    *   `StudentID` (Integer, should be the unique primary key)
    *   `FirstName` (Text, max 100 characters, cannot be empty)
    *   `LastName` (Text, max 100 characters, cannot be empty)
    *   `EnrollmentDate` (Date)
*   **`Courses` Table Structure:**
    *   `CourseID` (Text, max 10 characters, should be the unique primary key)
    *   `CourseName` (Text, max 150 characters, cannot be empty)
    *   `Credits` (Integer, must be a value greater than 0)
*   **Task:** Write the SQL statements to:
    1.  Create the `UniversityDB` database.
    2.  Select the `UniversityDB` database for use.
    3.  Create the `Students` table with the specified columns and constraints.
    4.  Create the `Courses` table with the specified columns and constraints.

**Answer & Explanation: Question 1**

```sql
-- 1. Create the UniversityDB database
CREATE DATABASE UniversityDB;

-- 2. Select the UniversityDB database for use
USE UniversityDB;

-- 3. Create the Students table
CREATE TABLE Students (
    StudentID INT PRIMARY KEY,
    FirstName VARCHAR(100) NOT NULL,
    LastName VARCHAR(100) NOT NULL,
    EnrollmentDate DATE
);

-- 4. Create the Courses table
CREATE TABLE Courses (
    CourseID VARCHAR(10) PRIMARY KEY,
    CourseName VARCHAR(150) NOT NULL,
    Credits INT CHECK (Credits > 0)
);
```

*   **Explanation:**
    *   **`CREATE DATABASE`**: This command creates a new, empty database.
    *   **`USE`**: This command selects a database, so all subsequent commands (like `CREATE TABLE`) are executed within that database.
    *   **`CREATE TABLE`**: This defines a new table.
    *   **`INT`, `VARCHAR`, `DATE`**: These are data types that specify the kind of data a column can hold.
    *   **`PRIMARY KEY`**: This constraint uniquely identifies each record in the table. It ensures no two rows can have the same `StudentID` or `CourseID`.
    *   **`NOT NULL`**: This constraint ensures that a column cannot have an empty (NULL) value.
    *   **`CHECK`**: This constraint enforces a specific condition on the values in a column. Here, it ensures that a course must have at least 1 credit.

---

**Question 2: Modifying Table Structure (`ALTER TABLE`)**

*   **Scenario:** After initial setup, you need to make some changes. A `Major` needs to be added for students, and you want to ensure the `CourseID` is always stored in uppercase. You also need to add a foreign key to link students to courses. For that, you will create a new table called `Enrollments`.
*   **Database for this question:** `UniversityDB` (from Question 1)
*   **`Enrollments` Table to be created:**
    *   `EnrollmentID` (Integer, Primary Key)
    *   `StudentID` (Integer, Foreign Key to `Students` table)
    *   `CourseID` (Text, max 10 characters, Foreign Key to `Courses` table)
    *   `Grade` (Text, max 2 characters)
*   **Task:** Write the SQL statements to:
    1.  Add a `Major` column with a data type of text (max 50 characters) to the `Students` table.
    2.  Create the `Enrollments` table with the appropriate primary and foreign key constraints.

**Answer & Explanation: Question 2**

```sql
-- 1. Add a Major column to the Students table
ALTER TABLE Students
ADD Major VARCHAR(50);

-- 2. Create the Enrollments table with foreign keys
CREATE TABLE Enrollments (
    EnrollmentID INT PRIMARY KEY,
    StudentID INT,
    CourseID VARCHAR(10),
    Grade VARCHAR(2),
    FOREIGN KEY (StudentID) REFERENCES Students(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses(CourseID)
);
```

*   **Explanation:**
    *   **`ALTER TABLE`**: This command is used to add, delete, or modify columns in an existing table.
    *   **`ADD`**: This clause within `ALTER TABLE` specifies that you are adding a new column.
    *   **`FOREIGN KEY`**: This constraint is crucial for creating relationships between tables. It ensures that a value in the `StudentID` column of the `Enrollments` table must already exist in the `StudentID` column of the `Students` table (and the same for `CourseID`). This prevents "orphan" records.

---

### **Part 2: Data Manipulation Language (DML)**
This section tests your ability to insert, query, update, and delete data.

**Database for this section:** `CompanyDB`

**`Employees` Table:**

| EmployeeID | FirstName | LastName | Department | Salary | HireDate |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 101 | Sarah | Chen | Sales | 70000 | 2022-05-20 |
| 102 | David | Lee | IT | 85000 | 2021-08-15 |
| 103 | Emily | Wang | Marketing | 62000 | 2023-01-10 |
| 104 | Brian | Kim | IT | 95000 | 2020-03-12 |
| 105 | Jessica | Tran | Sales | 72000 | 2022-05-22 |

**`Departments` Table:**

| DepartmentName | Location | ManagerID |
| :--- | :--- | :--- |
| Sales | New York | 101 |
| IT | San Francisco| 104 |
| Marketing | New York | 103 |
| HR | Chicago | NULL |

**Question 3: Inserting and Updating Data**

*   **Task:** Write the SQL statements to:
    1.  Insert a new employee into the `Employees` table: `EmployeeID` 106, `FirstName` 'Michael', `LastName` 'Park', `Department` 'HR', `Salary` 58000, `HireDate` '2023-11-01'.
    2.  The company has decided to give all employees in the 'IT' department a 10% salary increase. Update their salaries accordingly.

**Answer & Explanation: Question 3**

```sql
-- 1. Insert a new employee
INSERT INTO Employees (EmployeeID, FirstName, LastName, Department, Salary, HireDate)
VALUES (106, 'Michael', 'Park', 'HR', 58000, '2023-11-01');

-- 2. Update salaries for the IT department
UPDATE Employees
SET Salary = Salary * 1.10
WHERE Department = 'IT';
```

*   **Explanation:**
    *   **`INSERT INTO`**: This command adds a new row of data. The `VALUES` clause provides the data for each column listed.
    *   **`UPDATE`**: This command modifies existing records.
    *   **`SET`**: Specifies which column to change and defines its new value. Here, we use a calculation to increase the existing salary.
    *   **`WHERE`**: This clause is critical for both `UPDATE` and `DELETE`. It specifies *which* rows should be affected. Without it, every employee's salary would have been updated.

---

**Question 4: Selecting and Filtering Data (`SELECT`, `WHERE`)**

*   **Task:** Write `SELECT` statements to retrieve the following:
    1.  The `FirstName`, `LastName`, and `Salary` of all employees, sorted by salary in descending order.
    2.  All information about employees who work in the 'Sales' department AND have a salary greater than 71000.
    3.  The `FirstName` of employees whose last name is 'Lee' OR 'Kim'.
    4.  All employees hired in the year 2022.

**Answer & Explanation: Question 4**

```sql
-- 1. Select and sort all employees by salary
SELECT FirstName, LastName, Salary
FROM Employees
ORDER BY Salary DESC;

-- 2. Select employees from 'Sales' with a salary > 71000
SELECT *
FROM Employees
WHERE Department = 'Sales' AND Salary > 71000;

-- 3. Select employees with last name 'Lee' or 'Kim'
SELECT FirstName
FROM Employees
WHERE LastName IN ('Lee', 'Kim');

-- 4. Select employees hired in 2022
SELECT *
FROM Employees
WHERE YEAR(HireDate) = 2022;
```

*   **Explanation:**
    *   **`SELECT`**: The fundamental command for retrieving data.
    *   **`ORDER BY`**: Sorts the result set. `DESC` specifies descending order; `ASC` (or omitting it) specifies ascending order.
    *   **`AND` / `OR`**: Logical operators used to combine multiple conditions in a `WHERE` clause.
    *   **`IN`**: A shorthand way to check if a value matches any value in a list. It's cleaner than writing `WHERE LastName = 'Lee' OR LastName = 'Kim'`.
    *   **`YEAR()`**: A common SQL function that extracts the year from a date value, making it easy to filter by year.

---

### **Part 3: Advanced Queries**
This section tests your knowledge of joins, aggregate functions, and subqueries.

**Question 5: Aggregate Functions (`COUNT`, `AVG`, `GROUP BY`)**

*   **Database for this question:** `CompanyDB` (from Part 2)
*   **Task:** Write `SELECT` statements to find:
    1.  The total number of employees in the company.
    2.  The average salary of employees in the 'Sales' department.
    3.  The number of employees in each department. The result should show the department name and the count of employees.

**Answer & Explanation: Question 5**

```sql
-- 1. Count the total number of employees
SELECT COUNT(*) AS TotalEmployees FROM Employees;

-- 2. Find the average salary for the Sales department
SELECT AVG(Salary) AS AverageSalesSalary
FROM Employees
WHERE Department = 'Sales';

-- 3. Count the number of employees in each department
SELECT Department, COUNT(EmployeeID) AS NumberOfEmployees
FROM Employees
GROUP BY Department;
```

*   **Explanation:**
    *   **`COUNT()`, `AVG()`**: These are aggregate functions. `COUNT()` counts the number of rows, and `AVG()` calculates the average of a numeric column.
    *   **`AS`**: This keyword creates an alias, which is a temporary, more readable name for a column in the result set (e.g., `TotalEmployees`).
    *   **`GROUP BY`**: This clause is essential when using aggregate functions. It groups rows that have the same values in specified columns into summary rows. In query #3, it groups all 'Sales' employees together to be counted, all 'IT' employees together, and so on.

---

**Question 6: Joining Tables (`INNER JOIN`, `LEFT JOIN`)**

*   **Database for this question:** `CompanyDB` (from Part 2)
*   **Task:** Write `SELECT` statements to:
    1.  List the `FirstName` of each employee along with their department's `Location`.
    2.  List *all* department names and the `FirstName` of their manager. Include departments that do not have a manager assigned yet.

**Answer & Explanation: Question 6**

```sql
-- 1. List employee first names and their department location
SELECT E.FirstName, D.Location
FROM Employees E
INNER JOIN Departments D ON E.Department = D.DepartmentName;

-- 2. List all departments and their manager's first name
SELECT D.DepartmentName, E.FirstName AS ManagerFirstName
FROM Departments D
LEFT JOIN Employees E ON D.ManagerID = E.EmployeeID;
```

*   **Explanation:**
    *   **`INNER JOIN`**: This returns only the rows where the join condition is met in *both* tables. It combines an employee with their department based on the matching department name.
    *   **`LEFT JOIN`**: This returns *all* rows from the left table (`Departments`) and the matched rows from the right table (`Employees`). If there is no match (like the 'HR' department which has a `NULL` ManagerID), the columns from the right table (`FirstName`) will be NULL. This is perfect for finding what's in one table but doesn't have a corresponding entry in another.
    *   **`E` and `D`**: These are table aliases used to make the query shorter and easier to read.

---

**Question 7: Subqueries**

*   **Database for this question:** `CompanyDB` (from Part 2)
*   **Task:** Write a `SELECT` statement to find the `FirstName` and `Salary` of all employees who earn more than the company's average salary.

**Answer & Explanation: Question 7**

```sql
SELECT FirstName, Salary
FROM Employees
WHERE Salary > (SELECT AVG(Salary) FROM Employees);
```

*   **Explanation:**
    *   **Subquery**: The part in the parentheses `(SELECT AVG(Salary) FROM Employees)` is a subquery. It is a query nested inside another query.
    *   **Execution Flow**: The database first executes the inner subquery to calculate the single average salary value for the entire company. Then, the outer query runs and compares each employee's salary to that calculated average, returning only those who earn more. This is a powerful way to filter data based on a dynamically calculated value.