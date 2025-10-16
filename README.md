
# SQL Notes (Full Version) v1

## 目錄 (Table of Contents)

1.  [Data Definition Language (DDL) - 資料定義語言](#1-data-definition-language-ddl---資料定義語言)
    *   [CREATE DATABASE](#create-database)
    *   [USE](#use)
    *   [CREATE TABLE](#create-table)
        *   [基本語法 (Basic Syntax)](#基本語法-basic-syntax)
        *   [資料類型 (Data Types)](#資料類型-data-types)
        *   [約束 (Constraints)](#約束-constraints)
            *   `PRIMARY KEY` (主鍵)
            *   `FOREIGN KEY` (外鍵)
            *   `NOT NULL` (非空)
            *   `UNIQUE` (唯一)
            *   `DEFAULT` (默認值)
            *   `CHECK` (檢查)
    *   [ALTER TABLE](#alter-table)
        *   `ADD` (新增欄位)
        *   `DROP COLUMN` (刪除欄位)
        *   `MODIFY` (修改欄位資料類型)
        *   `ADD CONSTRAINT` (新增約束)
        *   `DROP CONSTRAINT` (刪除約束)
    *   [DROP TABLE](#drop-table)
    *   [TRUNCATE TABLE](#truncate-table)
    *   [RENAME TABLE](#rename-table)
    *   [DROP DATABASE](#drop-database)
2.  [Data Manipulation Language (DML) - 資料操作語言](#2-data-manipulation-language-dml---資料操作語言)
    *   [INSERT INTO](#insert-into)
    *   [UPDATE](#update)
    *   [DELETE](#delete)
3.  [Data Query Language (DQL) - 資料查詢語言](#3-data-query-language-dql---資料查詢語言)
    *   [SELECT...FROM](#selectfrom)
    *   [SELECT DISTINCT](#select-distinct)
    *   [AS (Alias)](#as-alias)
    *   [WHERE](#where)
    *   [ORDER BY](#order-by)
4.  [Operators (運算子)](#4-operators-運算子)
    *   [Arithmetic Operators (算術運算子)](#arithmetic-operators-算術運算子)
    *   [Logical Operators (邏輯運算子)](#logical-operators-邏輯運算子)
        *   [AND, OR, NOT](#and-or-not)
        *   [BETWEEN](#between)
        *   [IN](#in)
        *   [LIKE](#like)
        *   [IS NULL](#is-null)
5.  [Functions (函數)](#5-functions-函數)
    *   [Aggregate Functions (聚合函數)](#aggregate-functions-聚合函數)
        *   [COUNT()](#count)
        *   [SUM()](#sum)
        *   [AVG()](#avg)
        *   [MAX()](#max)
        *   [MIN()](#min)
    *   [Text Functions (文字函數)](#text-functions-文字函數)
    *   [Date Functions (日期函數)](#date-functions-日期函數)
6.  [Advanced Querying (進階查詢)](#6-advanced-querying-進階查詢)
    *   [GROUP BY](#group-by)
    *   [HAVING](#having)
    *   [Table Joins (資料表連接)](#table-joins-資料表連接)
        *   [INNER JOIN](#inner-join)
        *   [LEFT JOIN](#left-join)
        *   [RIGHT JOIN](#right-join)
        *   [SELF JOIN](#self-join)
        *   [CROSS JOIN](#cross-join)
    *   [Set Operators (集合運算子)](#set-operators-集合運算子)
        *   [UNION](#union)
        *   [UNION ALL](#union-all)
        *   [INTERSECT](#intersect)
        *   [EXCEPT](#except)
    *   [Subquery (子查詢)](#subquery-子查詢)
7.  [Views & Indexes (視圖與索引)](#7-views--indexes-視圖與索引)
    *   [CREATE VIEW](#create-view)
    *   [CREATE INDEX](#create-index)
8.  [重點比較 (Key Comparisons)](#8-重點比較-key-comparisons)

---

## 1. Data Definition Language (DDL) - 資料定義語言

DDL 用於定義和管理資料庫的結構，例如建立、修改或刪除資料表，而不是處理資料表內的資料。

### CREATE DATABASE
*   **作用**: 建立一個新的資料庫 (Create a new database)。
*   **SQL 例子**:
    ```sql
    CREATE DATABASE SchoolDB;
    ```
*   **執行後結果**:
    (一個名為 `SchoolDB` 的新資料庫被建立)

### USE
*   **作用**: 選擇要操作的資料庫 (Select a database to use)。
*   **SQL 例子**:
    ```sql
    USE SchoolDB;
    ```
*   **執行後結果**:
    (之後所有的操作都會在 `SchoolDB` 資料庫中進行)

### CREATE TABLE

#### 基本語法 (Basic Syntax)
*   **作用**: 在當前資料庫中建立一個新的資料表 (Create a new table)。
*   **SQL 例子**:
    ```sql
    CREATE TABLE Student (
        SID VARCHAR(5),
        Name VARCHAR(30),
        DOB DATE
    );
    ```
*   **執行後結果**:
    (一個名為 `Student` 的新資料表被建立，包含 `SID`, `Name`, `DOB` 三個欄位)

#### 資料類型 (Data Types)
*   **作用**: 為欄位指定可以儲存的資料類型 (Specify the type of data a column can hold)。

| Data Type | Description (描述) | Example (例子) |
| :--- | :--- | :--- |
| `VARCHAR(n)` | 可變長度字串，最多n個字元 (Variable-length string) | `VARCHAR(50)` |
| `CHAR(n)` | 固定長度字串，總是n個字元 (Fixed-length string) | `CHAR(8)` |
| `INT` | 整數 (Integer) | `INT` |
| `FLOAT` | 浮點數 (Floating-point number) | `FLOAT` |
| `DATE` | 日期 (YYYY-MM-DD) | `DATE` |
| `DATETIME` | 日期和時間 (YYYY-MM-DD HH:MI:SS) | `DATETIME` |
| `BOOL` / `BOOLEAN` | 布林值 (TRUE/FALSE) | `BOOL` |

*   **SQL 例子**:
    ```sql
    CREATE TABLE PRODUCT (
        PID CHAR(6),
        PNAME VARCHAR(80),
        STOCK INT,
        PRICE FLOAT,
        EXPIRY DATE
    );
    ```
*   **執行後結果**:
    (建立了 `PRODUCT` 表，每個欄位都有指定的資料類型)

#### 約束 (Constraints)
*   **作用**: 為資料表中的資料定義規則 (Define rules for the data in a table)。
*   **`PRIMARY KEY` (主鍵)**: 唯一標識資料表中的每一行，不能包含 NULL 值。
    *   **SQL 例子**:
        ```sql
        CREATE TABLE Course (
            Course_ID VARCHAR(3) PRIMARY KEY,
            CourseName VARCHAR(30)
        );
        ```
*   **`FOREIGN KEY` (外鍵)**: 一個表中的一個欄位，指向另一個表的主鍵，用於建立和強制兩個表之間的連結。
    *   **SQL 例子**:
        ```sql
        CREATE TABLE Enrollment (
            SID VARCHAR(5),
            Course_ID VARCHAR(3),
            PRIMARY KEY (SID, Course_ID),
            FOREIGN KEY (SID) REFERENCES Student(SID),
            FOREIGN KEY (Course_ID) REFERENCES Course(Course_ID)
        );
        ```
*   **`NOT NULL` (非空)**: 確保欄位不能有 NULL 值。
    *   **SQL 例子**:
        ```sql
        CREATE TABLE Student (
            SID VARCHAR(5) PRIMARY KEY,
            Name VARCHAR(30) NOT NULL,
            DOB DATE
        );
        ```*   **`UNIQUE` (唯一)**: 確保欄位中的所有值都是不同的。
    *   **SQL 例子**:
        ```sql
        CREATE TABLE Student (
            SID VARCHAR(5) PRIMARY KEY,
            Email VARCHAR(50) UNIQUE,
            Name VARCHAR(30) NOT NULL
        );
        ```
*   **`DEFAULT` (默認值)**: 如果沒有指定值，則為欄位提供一個默認值。
    *   **SQL 例子**:
        ```sql
        CREATE TABLE Orders (
            OrderID INT PRIMARY KEY,
            OrderDate DATE DEFAULT GETDATE()
        );
        ```
*   **`CHECK` (檢查)**: 確保欄位中的值滿足特定條件。
    *   **SQL 例子**:
        ```sql
        CREATE TABLE Student (
            SID VARCHAR(5) PRIMARY KEY,
            Age INT CHECK (Age >= 18)
        );
        ```
### ALTER TABLE
*   **作用**: 修改現有資料表的結構 (Modify an existing table structure)。
*   **參考資料表 (Student)**:
    | SID | Name | DOB |
    | :--- | :--- | :--- |
    | 'S001'| Peter | 1999-05-10 |

*   **`ADD` (新增欄位)**
    *   **SQL 例子**: `ALTER TABLE Student ADD Gender CHAR(1);`
    *   **執行後結果 (結構)**:
        | SID | Name | DOB | Gender |
        | :--- | :--- | :--- | :--- |

*   **`DROP COLUMN` (刪除欄位)**
    *   **SQL 例子**: `ALTER TABLE Student DROP COLUMN DOB;`
    *   **執行後結果 (結構)**:
        | SID | Name | Gender |
        | :--- | :--- | :--- |

*   **`MODIFY` (修改欄位資料類型)**
    *   **SQL 例子**: `ALTER TABLE Student MODIFY Name VARCHAR(50);`
    *   **執行後結果 (結構)**: (`Name` 欄位的最大長度變為 50)

*   **`ADD CONSTRAINT` (新增約束)**
    *   **SQL 例子 (新增主鍵)**: `ALTER TABLE Student ADD CONSTRAINT PK_Student PRIMARY KEY (SID);`
    *   **執行後結果 (結構)**: (`SID` 欄位現在是主鍵)

*   **`DROP CONSTRAINT` (刪除約束)**
    *   **SQL 例子 (刪除主鍵)**: `ALTER TABLE Student DROP CONSTRAINT PK_Student;`
    *   **執行後結果 (結構)**: (`SID` 欄位不再是主鍵)

### DROP TABLE
*   **作用**: 永久刪除一個資料表及其所有資料 (Permanently delete a table and all its data)。
*   **SQL 例子**: `DROP TABLE Enrollment;`
*   **執行後結果**: (整個 `Enrollment` 資料表被永久刪除)

### TRUNCATE TABLE
*   **作用**: 快速刪除資料表中的所有資料，但保留資料表結構 (Quickly delete all data from a table, but keep the table structure)。
*   **SQL 例子**: `TRUNCATE TABLE Student;`
*   **執行後結果**: (`Student` 資料表變為空的，但表結構仍然存在)

### RENAME TABLE
*   **作用**: 重新命名一個現有的資料表 (Rename an existing table)。
*   **SQL 例子**: `RENAME TABLE Student TO Pupil;`
*   **執行後結果**: (資料表 `Student` 的名稱變為 `Pupil`)

### DROP DATABASE
*   **作用**: 永久刪除整個資料庫 (Permanently delete an entire database)。
*   **SQL 例子**: `DROP DATABASE SchoolDB;`
*   **執行後結果**: (整個 `SchoolDB` 資料庫及其包含的所有物件都被永久刪除)

---

## 2. Data Manipulation Language (DML) - 資料操作語言
DML 用於管理資料庫中的資料，包括插入、更新和刪除。

### INSERT INTO
*   **作用**: 向資料表中插入新的記錄 (Insert new records into a table)。
*   **參考資料表 (Student)** (空的):
    | SID | Name | CLASS |
    | :-- | :--- | :---- |
*   **SQL 例子**:
    ```sql
    INSERT INTO Student (SID, Name, CLASS)
    VALUES ('S001', 'Ada', '1A'), ('S002', 'Clem', '1B');
    ```
*   **執行後結果 (Student)**:
    | SID | Name | CLASS |
    | :--- | :--- | :---- |
    | S001 | Ada | 1A |
    | S002 | Clem | 1B |

### UPDATE
*   **作用**: 修改資料表中的現有記錄 (Modify existing records in a table)。
*   **參考資料表 (Student)**:
    | SID | Name | CLASS |
    | :--- | :--- | :---- |
    | S001 | Ada | 1A |
*   **SQL 例子**:
    ```sql
    UPDATE Student
    SET CLASS = '1C'
    WHERE SID = 'S001';
    ```
*   **執行後結果 (Student)**:
    | SID | Name | CLASS |
    | :--- | :--- | :---- |
    | S001 | Ada | 1C |

### DELETE
*   **作用**: 從資料表中刪除記錄 (Delete records from a table)。
*   **參考資料表 (Student)**:
    | SID | Name | CLASS |
    | :--- | :--- | :---- |
    | S001 | Ada | 1C |
    | S002 | Clem | 1B |
*   **SQL 例子**:
    ```sql
    DELETE FROM Student
    WHERE SID = 'S001';
    ```
*   **執行後結果 (Student)**:
    | SID | Name | CLASS |
    | :--- | :--- | :---- |
    | S002 | Clem | 1B |

---

## 3. Data Query Language (DQL) - 資料查詢語言
DQL 用於從資料庫中檢索資料。

### SELECT...FROM
*   **作用**: 從一個或多個資料表中檢索資料 (Retrieve data from one or more tables)。
*   **參考資料表 (Student)**:
    | SID | Name | CLASS | SCORE1 |
    | :--- | :--- | :---- | :----- |
    | S002 | Clem | 1B | 81 |
    | S003 | Eva | 1B | 62 |
*   **SQL 例子**: `SELECT Name, CLASS FROM Student;`
*   **執行後結果**:
    | Name | CLASS |
    | :--- | :---- |
    | Clem | 1B |
    | Eva | 1B |

### SELECT DISTINCT
*   **作用**: 只返回唯一的值，過濾掉重複的行 (Return only distinct (different) values)。
*   **參考資料表 (Student)**:
    | CLASS |
    | :---- |
    | 1A |
    | 1B |
    | 1B |
*   **SQL 例子**: `SELECT DISTINCT CLASS FROM Student;`
*   **執行後結果**:
    | CLASS |
    | :---- |
    | 1A |
    | 1B |

### AS (Alias)
*   **作用**: 為欄位或資料表指定一個臨時的別名 (Give a table or a column a temporary name (alias))。
*   **參考資料表 (Student)**:
    | Name | SCORE1 | SCORE2 |
    | :--- | :----- | :----- |
    | Hans | 82 | 77 |
*   **SQL 例子**: `SELECT Name, (SCORE1 * 0.4 + SCORE2 * 0.6) AS FinalScore FROM Student;`
*   **執行後結果**:
    | Name | FinalScore |
    | :--- | :--------- |
    | Hans | 79.0 |

### WHERE
*   **作用**: 根據指定的條件過濾記錄 (Filter records based on specified criteria)。
*   **參考資料表 (Student)**:
    | Name | SCORE1 |
    | :--- | :----- |
    | Ivy | 46 |
    | Hans | 82 |
*   **SQL 例子**: `SELECT Name, SCORE1 FROM Student WHERE SCORE1 < 60;`
*   **執行後結果**:
    | Name | SCORE1 |
    | :--- | :----- |
    | Ivy | 46 |

### ORDER BY
*   **作用**: 對結果集按一個或多個欄位進行排序 (Sort the result-set by one or more columns)。 `ASC` (升序，預設) 或 `DESC` (降序)。
*   **參考資料表 (Student)**:
    | Name | CNO |
    | :--- | :-- |
    | Clem | 15 |
    | Eva | 28 |
    | Finn | 7 |
*   **SQL 例子**: `SELECT Name, CNO FROM Student ORDER BY CNO DESC;`
*   **執行後結果**:
    | Name | CNO |
    | :--- | :-- |
    | Eva | 28 |
    | Clem | 15 |
    | Finn | 7 |

---

## 4. Operators (運算子)

### Arithmetic Operators (算術運算子)
*   **作用**: 執行數學運算 (Perform mathematical operations)。
*   **參考資料表 (TEMP)**:
    | Name | DEGREE_F |
    | :--- | :------- |
    | Jack | 98.6 |
*   **SQL 例子**: `SELECT Name, (DEGREE_F - 32) * 5 / 9 AS DEGREE_C FROM TEMP;`
*   **執行後結果**:
    | Name | DEGREE_C |
    | :--- | :------- |
    | Jack | 37.0 |

### Logical Operators (邏輯運算子)
#### AND, OR, NOT
*   **作用**: 在 `WHERE` 子句中組合條件 (Combine conditions in the `WHERE` clause)。
*   **參考資料表 (Student)**:
    | Name | SCORE1 | CLASS |
    | :--- | :----- | :---- |
    | Ivy | 46 | 2A |
    | Clem | 81 | 1B |
*   **SQL 例子**: `SELECT Name FROM Student WHERE SCORE1 < 60 OR CLASS = '1B';`
*   **執行後結果**:
    | Name |
    | :--- |
    | Ivy |
    | Clem |
#### BETWEEN
*   **作用**: 選取指定範圍內的值 (Select values within a given range)。
*   **參考資料表 (Student)**:
    | Name | DOB |
    | :--- | :----------|
    | Ada | 2013-09-21 |
    | Gabe | 2013-08-23 |
*   **SQL 例子**: `SELECT Name FROM Student WHERE DOB BETWEEN '2013-08-01' AND '2013-08-31';`
*   **執行後結果**:
    | Name |
    | :--- |
    | Gabe |
#### IN
*   **作用**: 指定 `WHERE` 子句中的多個可能值 (Specify multiple possible values in a `WHERE` clause)。
*   **參考資料表 (Student)**:
    | Name | CLASS |
    | :--- | :---- |
    | Hans | 2C |
    | Ada | 1A |
*   **SQL 例子**: `SELECT Name FROM Student WHERE CLASS IN ('1A', '2C');`
*   **執行後結果**:
    | Name |
    | :--- |
    | Hans |
    | Ada |
#### LIKE
*   **作用**: 搜索欄位中的特定模式 (Search for a specific pattern in a column)。`%` 代表零個或多個字符，`_` 代表一個字符。
*   **參考資料表 (Student)**:
    | Name |
    | :--- |
    | Hans |
    | Jean |
    | Ben |
*   **SQL 例子**: `SELECT Name FROM Student WHERE Name LIKE '%an%';`
*   **執行後結果**:
    | Name |
    | :--- |
    | Hans |
    | Jean |
#### IS NULL
*   **作用**: 測試一個值是否為 `NULL` (Test if a value is `NULL`)。
*   **參考資料表 (Student)**:
    | Name | SCORE1 |
    | :--- | :----- |
    | Clem | NULL |
    | Eva | 83 |
*   **SQL 例子**: `SELECT Name FROM Student WHERE SCORE1 IS NULL;`
*   **執行後結果**:
    | Name |
    | :--- |
    | Clem |

---
## 5. Functions (函數)

### Aggregate Functions (聚合函數)
聚合函數對一組值進行計算，並返回單個值。

#### COUNT()
*   **作用**: 計算行數 (Count the number of rows)。
*   **參考資料表 (Student)**: 3條記錄
*   **SQL 例子**: `SELECT COUNT(*) FROM Student;`
*   **執行後結果**:
    | COUNT(*) |
    | :--- |
    | 3 |
#### SUM()
*   **作用**: 計算總和 (Calculate the sum)。
*   **參考資料表 (Student)**:
    | SCORE |
    | :---- |
    | 31 |
    | 90 |
*   **SQL 例子**: `SELECT SUM(SCORE) FROM Student;`
*   **執行後結果**:
    | SUM(SCORE) |
    | :--- |
    | 121 |
#### AVG()
*   **作用**: 計算平均值 (Calculate the average)。
*   **參考資料表 (Student)**:
    | SCORE |
    | :---- |
    | 80 |
    | 90 |
*   **SQL 例子**: `SELECT AVG(SCORE) FROM Student;`
*   **執行後結果**:
    | AVG(SCORE) |
    | :--- |
    | 85 |
#### MAX()
*   **作用**: 找出最大值 (Find the maximum value)。
*   **參考資料表 (Student)**:
    | SCORE |
    | :---- |
    | 80 |
    | 90 |
*   **SQL 例子**: `SELECT MAX(SCORE) FROM Student;`
*   **執行後結果**:
    | MAX(SCORE) |
    | :--- |
    | 90 |
#### MIN()
*   **作用**: 找出最小值 (Find the minimum value)。
*   **參考資料表 (Student)**:
    | SCORE |
    | :---- |
    | 80 |
    | 90 |
*   **SQL 例子**: `SELECT MIN(SCORE) FROM Student;`
*   **執行後結果**:
    | MIN(SCORE) |
    | :--- |
    | 80 |
### Text Functions (文字函數)
*   **作用**: 處理文字字串 (Manipulate text strings)。
*   **SQL 例子**: `SELECT UPPER('hello') as uc, LOWER('WORLD') as lc, LENGTH('SQL') as len;`
*   **執行後結果**:
    | uc | lc | len |
    | :--- | :--- | :--- |
    | HELLO | world | 3 |
### Date Functions (日期函數)
*   **作用**: 處理日期和時間 (Manipulate dates and times)。
*   **SQL 例子**: `SELECT YEAR('2025-10-16') as yr, MONTH('2025-10-16') as mth, DAY('2025-10-16') as dy;`
*   **執行後結果**:
    | yr | mth | dy |
    | :--- | :--- | :--- |
    | 2025 | 10 | 16 |
---

## 6. Advanced Querying (進階查詢)

### GROUP BY
*   **作用**: 結合聚合函數，根據一個或多個欄位對結果集進行分組 (Group rows that have the same values into summary rows, often used with aggregate functions)。
*   **參考資料表 (Student)**:
    | CLASS |
    | :---- |
    | 1B |
    | 1B |
    | 2A |
*   **SQL 例子**: `SELECT CLASS, COUNT(*) FROM Student GROUP BY CLASS;`
*   **執行後結果**:
    | CLASS | COUNT(*) |
    | :---- | :--- |
    | 1B | 2 |
    | 2A | 1 |
### HAVING
*   **作用**: 過濾由 `GROUP BY` 產生的分組結果 (Filter group results created by `GROUP BY`)。
*   **SQL 例子**: `SELECT CLASS, COUNT(*) FROM Student GROUP BY CLASS HAVING COUNT(*) > 1;`
*   **執行後結果**:
    | CLASS | COUNT(*) |
    | :---- | :--- |
    | 1B | 2 |

### Table Joins (資料表連接)
*   **參考資料表 (STUDENT)**:
    | SID | SNAME | CID |
    | :-- | :---- | :-- |
    | S01 | Ada   | C02 |
    | S02 | Clem  | C01 |
    | S03 | Ben   |NULL |
*   **參考資料表 (CLUB)**:
    | CID | CNAME        |
    | :-- | :----------- |
    | C01 | Chess Club   |
    | C02 | Art Club     |
    | C03 | English Club |

#### INNER JOIN
*   **作用**: 只返回兩個資料表中符合連接條件的記錄 (Returns records that have matching values in both tables)。
*   **SQL 例子**: `SELECT S.SNAME, C.CNAME FROM STUDENT S INNER JOIN CLUB C ON S.CID = C.CID;`
*   **執行後結果**:
    | SNAME | CNAME      |
    | :---- | :--------- |
    | Ada   | Art Club   |
    | Clem  | Chess Club |

#### LEFT JOIN
*   **作用**: 返回左邊資料表的所有記錄，即使右邊沒有匹配的記錄 (Returns all records from the left table, and the matched records from the right table)。
*   **SQL 例子**: `SELECT S.SNAME, C.CNAME FROM STUDENT S LEFT JOIN CLUB C ON S.CID = C.CID;`
*   **執行後結果**:
    | SNAME | CNAME      |
    | :---- | :--------- |
    | Ada   | Art Club   |
    | Clem  | Chess Club |
    | Ben   | NULL       |

#### RIGHT JOIN
*   **作用**: 返回右邊資料表的所有記錄，即使左邊沒有匹配的記錄 (Returns all records from the right table, and the matched records from the left table)。
*   **SQL 例子**: `SELECT S.SNAME, C.CNAME FROM STUDENT S RIGHT JOIN CLUB C ON S.CID = C.CID;`
*   **執行後結果**:
    | SNAME | CNAME        |
    | :---- | :----------- |
    | Clem  | Chess Club   |
    | Ada   | Art Club     |
    | NULL  | English Club |

#### SELF JOIN
*   **作用**: 將資料表與其自身進行連接 (Join a table to itself)。
*   **參考資料表 (Employee)**:
    | EmpID | Name  | ManagerID |
    | :---- | :---- | :-------- |
    | 1     | John  | 3         |
    | 2     | Mike  | 3         |
    | 3     | Sally | NULL      |
*   **SQL 例子**: `SELECT A.Name as Employee, B.Name as Manager FROM Employee A JOIN Employee B ON A.ManagerID = B.EmpID;`
*   **執行後結果**:
    | Employee | Manager |
    | :------- | :------ |
    | John     | Sally   |
    | Mike     | Sally   |

#### CROSS JOIN
*   **作用**: 返回兩個資料表行組合的笛卡爾積 (Returns the Cartesian product of the sets of records from the two joined tables)。
*   **參考資料表 (T1)**:
    | C1 |
    | :--|
    | A  |
    | B  |
*   **參考資料表 (T2)**:
    | C2 |
    |:---|
    | 1  |
    | 2  |
*   **SQL 例子**: `SELECT * FROM T1 CROSS JOIN T2;`
*   **執行後結果**:
    | C1 | C2 |
    | :--|:---|
    | A  | 1  |
    | A  | 2  |
    | B  | 1  |
    | B  | 2  |

### Set Operators (集合運算子)
*   **參考資料表 (STUDENT)**: `SID`: {'S01', 'S02', 'S03'}
*   **參考資料表 (NEW_STUDENT)**: `SID`: {'S03', 'S04'}

#### UNION
*   **作用**: 合併結果集並移除重複項 (Combines result-sets and removes duplicates)。
*   **SQL 例子**: `SELECT SID FROM STUDENT UNION SELECT SID FROM NEW_STUDENT;`
*   **執行後結果**:
    | SID  |
    |:-----|
    | S01  |
    | S02  |
    | S03  |
    | S04  |
#### UNION ALL
*   **作用**: 合併結果集並保留重複項 (Combines result-sets and includes duplicates)。
*   **SQL 例子**: `SELECT SID FROM STUDENT UNION ALL SELECT SID FROM NEW_STUDENT;`
*   **執行後結果**:
    | SID  |
    |:-----|
    | S01  |
    | S02  |
    | S03  |
    | S03  |
    | S04  |
#### INTERSECT
*   **作用**: 返回兩個結果集的交集 (Returns the intersection of two result-sets)。
*   **SQL 例子**: `SELECT SID FROM STUDENT INTERSECT SELECT SID FROM NEW_STUDENT;`
*   **執行後結果**:
    | SID  |
    |:-----|
    | S03  |
#### EXCEPT
*   **作用**: 返回第一個結果集中有，但第二個結果集中沒有的行 (Returns rows from the first query that are not present in the second query)。
*   **SQL 例子**: `SELECT SID FROM STUDENT EXCEPT SELECT SID FROM NEW_STUDENT;`
*   **執行後結果**:
    | SID  |
    |:-----|
    | S01  |
    | S02  |

### Subquery (子查詢)
*   **作用**: 在另一個 SQL 查詢中嵌套查詢 (A query nested inside another query)。
*   **參考資料表 (Student)**:
    | Name | Score |
    | :--- | :---- |
    | Clem | 89    |
    | Eva  | 83    |
*   **SQL 例子**: `SELECT Name, Score FROM Student WHERE Score = (SELECT MAX(Score) FROM Student);`
*   **執行後結果**:
    | Name | Score |
    | :--- | :---- |
    | Clem | 89    |
---

## 7. Views & Indexes (視圖與索引)

### CREATE VIEW
*   **作用**: 建立一個虛擬資料表，它是基於一個SQL語句的結果集 (Create a virtual table based on the result-set of an SQL statement)。
*   **SQL 例子**:
    ```sql
    CREATE VIEW TopStudents AS
    SELECT Name, Score
    FROM Student
    WHERE Score > 80;
    ```*   **執行後結果**:
    (一個名為 `TopStudents` 的 view 被建立。你可以用 `SELECT * FROM TopStudents;` 來查詢它)

### CREATE INDEX
*   **作用**: 在資料表上建立索引，可以大大加快查詢速度 (Create an index on a table to speed up searches and queries dramatically)。
*   **SQL 例子**: `CREATE INDEX idx_name ON Student (Name);`
*   **執行後結果**: (在 `Student` 表的 `Name` 欄位上建立了一個索引)

---
## 8. 重點比較 (Key Comparisons)

> **注意 (Notes):**
> *   **`JOIN` 的分別 (Difference between JOINS):**
>     *   `INNER JOIN`: 只顯示兩張表都有對應嘅資料。就好似 Venn diagram 中間交集嘅部分。
>     *   `LEFT JOIN`: 會顯示晒左邊表 (FROM 後面第一張表) 嘅所有資料，就算右邊表冇對應資料都照出，冇嘅部分會用 `NULL` 顯示。
>     *   `RIGHT JOIN`: 同 `LEFT JOIN` 相反，會顯示晒右邊表嘅所有資料。
>
> *   **`WHERE` vs. `HAVING`:**
>     *   `WHERE` 係喺分組 (`GROUP BY`) 前用嚟過濾單筆記錄 (filters individual rows **before** grouping)。
>     *   `HAVING` 係喺分組後用嚟過濾聚合後嘅結果 (filters groups **after** aggregation)。你唔可以喺 `WHERE` 子句度用聚合函數 (e.g., `COUNT()`, `SUM()`)。
>
> *   **`DROP` vs. `TRUNCATE` vs. `DELETE`:**
>     *   `DROP TABLE`: 永久刪除整個資料表結構和所有資料，無法復原。
>     *   `TRUNCATE TABLE`: 快速刪除資料表內所有資料，但保留表結構，比 `DELETE` 快，通常無法復原。
>     *   `DELETE FROM table`: 逐行刪除資料，可以配合 `WHERE` 刪除特定行，速度較慢，可以被復原 (rollback)。
>
> *   **`UNION` vs. `UNION ALL`:**
>     *   `UNION`: 合併結果時會自動篩走重複嘅記錄。
>     *   `UNION ALL`: 合併結果時會保留所有記錄，包括重複嘅，所以執行速度會比 `UNION` 快。
