

# SQL Notes (Full Version)

## 目錄 (Table of Contents)

1.  [Data Definition Language (DDL) - 資料定義語言](#1-data-definition-language-ddl---資料定義語言)
    *   [CREATE DATABASE](#create-database)
    *   [USE](#use)
    *   [CREATE TABLE](#create-table)
    *   [ALTER TABLE](#alter-table)
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
5.  [Functions (函數)](#5-functions-函數)
    *   [Aggregate Functions (聚合函數)](#aggregate-functions-聚合函數)
    *   [Text Functions (文字函數)](#text-functions-文字函數)
    *   [Date Functions (日期函數)](#date-functions-日期函數)
6.  [Advanced Querying (進階查詢)](#6-advanced-querying-進階查詢)
    *   [GROUP BY](#group-by)
    *   [HAVING](#having)
    *   [Table Joins (資料表連接)](#table-joins-資料表連接)
    *   [Subqueries (子查詢)](#subqueries-子查詢)
        *   [Subquery with `IN`](#subquery-with-in)
        *   [Subquery with `EXISTS`](#subquery-with-exists)
        *   [Subquery with `ANY`](#subquery-with-any)
        *   [Subquery with `ALL`](#subquery-with-all)
    *   [Set Operators (集合運算子)](#set-operators-集合運算子)
7.  [Views & Indexes (視圖與索引)](#7-views--indexes-視圖與索引)
    *   [CREATE VIEW](#create-view)
    *   [CREATE INDEX](#create-index)

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
*   **作用**: 在當前資料庫中建立一個新的資料表 (Create a new table)。

#### 基本語法與資料類型 (Basic Syntax & Data Types)
*   **作用**: 定義資料表名稱、欄位名稱及每個欄位的資料類型 (Define table name, column names, and the data type for each column)。

| Data Type | Description (描述) | Example (例子) |
| :--- | :--- | :--- |
| `VARCHAR(n)` | 可變長度字串，最多n個字元 (Variable-length string) | `VARCHAR(50)` |
| `CHAR(n)` | 固定長度字串，總是n個字元 (Fixed-length string) | `CHAR(8)` |
| `INT` | 整數 (Integer) | `INT` |
| `FLOAT` | 浮點數 (Floating-point number) | `FLOAT` |
| `DATE` | 日期 (YYYY-MM-DD) | `DATE` |
| `BOOL` | 布林值 (TRUE/FALSE) | `BOOL` |

*   **SQL 例子 (建立資料庫中的所有表格)**:
    ```sql
    CREATE TABLE Class (
        ClassID CHAR(3) PRIMARY KEY,
        ClassName VARCHAR(20) NOT NULL,
        TeacherName VARCHAR(50)
    );

    CREATE TABLE Student (
        SID CHAR(3) PRIMARY KEY,
        SName VARCHAR(50) NOT NULL,
        ClassID CHAR(3),
        DOB DATE,
        Score INT CHECK (Score >= 0 AND Score <= 100),
        FOREIGN KEY (ClassID) REFERENCES Class(ClassID)
    );
    ```
*   **執行後結果**:
    (建立了 `Class` 和 `Student` 兩個資料表，並定義了主鍵、外鍵、非空和檢查約束)

### ALTER TABLE
*   **作用**: 修改現有資料表的結構 (Modify an existing table structure)。
*   **參考資料表 (Student)** (初始結構):
    | SID | SName | ClassID | DOB | Score |
    | :-- | :---- | :------ | :-- | :---- |
*   **SQL 例子 (新增欄位 `ADD`)**:
    ```sql
    ALTER TABLE Student ADD Gender CHAR(1);
    ```
*   **執行後結果 (結構改變)**:
    | SID | SName | ClassID | DOB | Score | Gender |
    | :-- | :---- | :------ | :-- | :---- | :----- |
*   **SQL 例子 (修改欄位 `MODIFY`)**:
    ```sql
    ALTER TABLE Student MODIFY SName VARCHAR(100);
    ```
*   **執行後結果 (結構改變)**:
    (`SName` 欄位的最大長度變為 100)
*   **SQL 例子 (刪除欄位 `DROP COLUMN`)**:
    ```sql
    ALTER TABLE Student DROP COLUMN Gender;
    ```
*   **執行後結果 (結構改變)**:
    | SID | SName | ClassID | DOB | Score |
    | :-- | :---- | :------ | :-- | :---- |

### DROP TABLE
*   **作用**: 永久刪除一個資料表及其所有資料 (Permanently delete a table and all its data)。
*   **SQL 例子**: `DROP TABLE Student;`
*   **執行後結果**: (整個 `Student` 資料表被永久刪除)

### TRUNCATE TABLE
*   **作用**: 快速刪除資料表中的所有資料，但保留資料表結構 (Quickly delete all data from a table, but keep the table structure)。
*   **SQL 例子**: `TRUNCATE TABLE Student;`
*   **執行後結果**: (`Student` 資料表變為空的，但表結構仍然存在)

> **注意 (Notes): `DROP` vs. `TRUNCATE` vs. `DELETE`**
> *   `DROP TABLE`: 永久刪除整個資料表結構和所有資料，無法復原。
> *   `TRUNCATE TABLE`: 快速刪除資料表內所有資料，但保留表結構，比 `DELETE` 快，通常無法復原。
> *   `DELETE FROM table`: 逐行刪除資料，可以配合 `WHERE` 刪除特定行，速度較慢，可以被復原 (rollback)。

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

### INSERT INTO
*   **作用**: 向資料表中插入新的記錄 (Insert new records into a table)。
*   **參考資料表 (Class & Student)** (空的)
*   **SQL 例子**:
    ```sql
    INSERT INTO Class (ClassID, ClassName, TeacherName) VALUES
    ('C01', 'Class 1A', 'Mr. Bao'),
    ('C02', 'Class 1B', 'Mrs. Au'),
    ('C03', 'Class 1C', 'Ms. Chan');

    INSERT INTO Student (SID, SName, ClassID, DOB, Score) VALUES
    ('S01', 'Ada', 'C01', '2005-03-10', 95),
    ('S02', 'Clem', 'C02', '2005-08-22', 78),
    ('S03', 'Eva', 'C02', '2004-11-05', 88),
    ('S04', 'Gabe', 'C03', '2005-01-15', 65),
    ('S05', 'Dio', 'C03', '2005-06-30', 72),
    ('S06', 'Ben', NULL, '2004-09-01', 85);
    ```
*   **執行後結果**:
    **Class Table**:
    | ClassID | ClassName | TeacherName |
    | :------ | :-------- | :---------- |
    | C01     | Class 1A  | Mr. Bao     |
    | C02     | Class 1B  | Mrs. Au     |
    | C03     | Class 1C  | Ms. Chan    |
    
    **Student Table**:
    
    | SID | SName | ClassID | DOB        | Score |
    | :-- | :---- | :------ | :--------- | :---- |
    | S01 | Ada   | C01     | 2005-03-10 | 95    |
    | S02 | Clem  | C02     | 2005-08-22 | 78    |
    | S03 | Eva   | C02     | 2004-11-05 | 88    |
    | S04 | Gabe  | C03     | 2005-01-15 | 65    |
    | S05 | Dio   | C03     | 2005-06-30 | 72    |
    | S06 | Ben   | NULL    | 2004-09-01 | 85    |

### UPDATE
*   **作用**: 修改資料表中的現有記錄 (Modify existing records in a table)。
*   **參考資料表 (Student)**: (如上)
*   **SQL 例子**:
    ```sql
    UPDATE Student
    SET Score = 80
    WHERE SID = 'S02';
    ```
*   **執行後結果 (Student 表中 Clem 的記錄)**:
    | SID | SName | ClassID | DOB        | Score |
    | :-- | :---- | :------ | :--------- | :---- |
    | S02 | Clem  | C02     | 2005-08-22 | 80    |

### DELETE
*   **作用**: 從資料表中刪除記錄 (Delete records from a table)。
*   **參考資料表 (Student)**: (如上)
*   **SQL 例子**:
    ```sql
    DELETE FROM Student
    WHERE SID = 'S06';
    ```
*   **執行後結果**:
    (SID 為 'S06' 的 Ben 的記錄被刪除)

---

## 3. Data Query Language (DQL) - 資料查詢語言

### SELECT...FROM
*   **作用**: 從一個或多個資料表中檢索資料 (Retrieve data from one or more tables)。
*   **參考資料表 (Student)**
*   **SQL 例子**: `SELECT SName, Score FROM Student;`
*   **執行後結果**:
    | SName | Score |
    | :---- | :---- |
    | Ada   | 95    |
    | Clem  | 78    |
    | Eva   | 88    |
    | Gabe  | 65    |
    | Dio   | 72    |

### SELECT DISTINCT
*   **作用**: 只返回唯一的值，過濾掉重複的行 (Return only distinct (different) values)。
*   **參考資料表 (Student)**
*   **SQL 例子**: `SELECT DISTINCT ClassID FROM Student;`
*   **執行後結果**:
    | ClassID |
    | :------ |
    | C01     |
    | C02     |
    | C03     |
    | NULL    |

### AS (Alias)
*   **作用**: 為欄位或資料表指定一個臨時的別名 (Give a table or a column a temporary name (alias))。
*   **參考資料表 (Student)**
*   **SQL 例子**: `SELECT SName AS 'Student Name', Score * 1.05 AS 'Adjusted Score' FROM Student;`
*   **執行後結果**:
    | Student Name | Adjusted Score |
    | :----------- | :------------- |
    | Ada          | 99.75          |
    | Clem         | 81.9           |
    | ...          | ...            |

### WHERE
*   **作用**: 根據指定的條件過濾記錄 (Filter records based on specified criteria)。
*   **參考資料表 (Student)**
*   **SQL 例子**: `SELECT SName, Score FROM Student WHERE Score > 80;`
*   **執行後結果**:
    | SName | Score |
    | :---- | :---- |
    | Ada   | 95    |
    | Eva   | 88    |

### ORDER BY
*   **作用**: 對結果集按一個或多個欄位進行排序 (Sort the result-set by one or more columns)。 `ASC` (升序，預設) 或 `DESC` (降序)。
*   **參考資料表 (Student)**
*   **SQL 例子**: `SELECT SName, Score FROM Student ORDER BY Score DESC;`
*   **執行後結果**:
    | SName | Score |
    | :---- | :---- |
    | Ada   | 95    |
    | Eva   | 88    |
    | Clem  | 78    |
    | Dio   | 72    |
    | Gabe  | 65    |

---

## 4. Operators (運算子)

### Arithmetic Operators (算術運算子)
*   **作用**: 執行數學運算 (Perform mathematical operations)。
*   **參考資料表 (Student)**
*   **SQL 例子**: `SELECT SName, Score, Score + 5 AS BonusScore FROM Student WHERE SID = 'S01';`
*   **執行後結果**:
    | SName | Score | BonusScore |
    | :---- | :---- | :--------- |
    | Ada   | 95    | 100        |

### Logical Operators (邏輯運算子)
*   **AND, OR, NOT**: 在 `WHERE` 子句中組合條件。
    *   **SQL 例子**: `SELECT SName FROM Student WHERE Score > 75 AND ClassID = 'C02';`
    *   **執行後結果**:
        | SName |
        | :---- |
        | Clem  |
        | Eva   |

*   **BETWEEN**: 選取指定範圍內的值。
    *   **SQL 例子**: `SELECT SName, Score FROM Student WHERE Score BETWEEN 70 AND 80;`
    *   **執行後結果**:
        | SName | Score |
        | :---- | :---- |
        | Clem  | 78    |
        | Dio   | 72    |

*   **IN**: 指定 `WHERE` 子句中的多個可能值。
    *   **SQL 例子**: `SELECT SName, ClassID FROM Student WHERE ClassID IN ('C01', 'C03');`
    *   **執行後結果**:
        | SName | ClassID |
        | :---- | :------ |
        | Ada   | C01     |
        | Gabe  | C03     |
        | Dio   | C03     |

*   **LIKE**: 搜索欄位中的特定模式。
    *   **SQL 例子**: `SELECT SName FROM Student WHERE SName LIKE 'A%';`
    *   **執行後結果**:
        | SName |
        | :---- |
        | Ada   |

*   **IS NULL**: 測試一個值是否為 `NULL`。
    *   **SQL 例子**: `SELECT SName FROM Student WHERE ClassID IS NULL;`
    *   **執行後結果**:
        | SName |
        | :---- |
        | Ben   |

---
## 5. Functions (函數)

### Aggregate Functions (聚合函數)
聚合函數對一組值進行計算，並返回單個值。

*   **COUNT()**: 計算行數。
    *   **SQL 例子**: `SELECT COUNT(*) AS TotalStudents FROM Student;`
    *   **執行後結果**:
        | TotalStudents |
        | :------------ |
        | 5             |

*   **SUM()**: 計算總和。
    *   **SQL 例子**: `SELECT SUM(Score) AS TotalScore FROM Student;`
    *   **執行後結果**:
        | TotalScore |
        | :--------- |
        | 398        |

*   **AVG()**: 計算平均值。
    *   **SQL 例子**: `SELECT AVG(Score) AS AverageScore FROM Student;`
    *   **執行後結果**:
        | AverageScore |
        | :----------- |
        | 79.6         |

*   **MAX()**: 找出最大值。
    *   **SQL 例子**: `SELECT MAX(Score) AS HighestScore FROM Student;`
    *   **執行後結果**:
        | HighestScore |
        | :----------- |
        | 95           |

*   **MIN()**: 找出最小值。
    *   **SQL 例子**: `SELECT MIN(Score) AS LowestScore FROM Student;`
    *   **執行後結果**:
        | LowestScore |
        | :---------- |
        | 65          |

### Text Functions (文字函數)
*   **作用**: 處理文字字串 (Manipulate text strings)。
*   **SQL 例子**: `SELECT UPPER(SName) as UCName, LENGTH(SName) as NameLen FROM Student WHERE SID = 'S01';`
*   **執行後結果**:
    | UCName | NameLen |
    | :----- | :------ |
    | ADA    | 3       |

### Date Functions (日期函數)
*   **作用**: 處理日期和時間 (Manipulate dates and times)。
*   **SQL 例子**: `SELECT SName, YEAR(DOB) as BirthYear FROM Student WHERE SID = 'S03';`
*   **執行後結果**:
    | SName | BirthYear |
    | :---- | :-------- |
    | Eva   | 2004      |

---

## 6. Advanced Querying (進階查詢)

### GROUP BY
*   **作用**: 結合聚合函數，根據一個或多個欄位對結果集進行分組 (Group rows that have the same values into summary rows)。
*   **參考資料表 (Student)**
*   **SQL 例子**: `SELECT ClassID, AVG(Score) AS AvgScore FROM Student GROUP BY ClassID;`
*   **執行後結果**:
    | ClassID | AvgScore |
    | :------ | :------- |
    | C01     | 95.0     |
    | C02     | 83.0     |
    | C03     | 68.5     |
    | NULL    | 85.0     |

### HAVING
*   **作用**: 過濾由 `GROUP BY` 產生的分組結果 (Filter group results created by `GROUP BY`)。
*   **SQL 例子**: `SELECT ClassID, AVG(Score) AS AvgScore FROM Student GROUP BY ClassID HAVING AVG(Score) > 80;`
*   **執行後結果**:
    | ClassID | AvgScore |
    | :------ | :------- |
    | C01     | 95.0     |
    | C02     | 83.0     |

> **注意 (Notes): `WHERE` vs. `HAVING`**
> *   `WHERE` 係喺分組 (`GROUP BY`) 前用嚟過濾單筆記錄 (filters individual rows **before** grouping)。
> *   `HAVING` 係喺分組後用嚟過濾聚合後嘅結果 (filters groups **after** aggregation)。你唔可以喺 `WHERE` 子句度用聚合函數 (e.g., `COUNT()`, `SUM()`)。

### Table Joins (資料表連接)
*   **參考資料表 (Student & Class)**

#### INNER JOIN
*   **作用**: 只返回兩個資料表中符合連接條件的記錄 (Returns records that have matching values in both tables)。
*   **SQL 例子**: `SELECT S.SName, C.ClassName FROM Student S INNER JOIN Class C ON S.ClassID = C.ClassID;`
*   **執行後結果**:
    | SName | ClassName |
    | :---- | :-------- |
    | Ada   | Class 1A  |
    | Clem  | Class 1B  |
    | Eva   | Class 1B  |
    | Gabe  | Class 1C  |
    | Dio   | Class 1C  |

#### LEFT JOIN
*   **作用**: 返回左邊資料表的所有記錄，即使右邊沒有匹配的記錄 (Returns all records from the left table, and the matched records from the right table)。
*   **SQL 例子**: `SELECT S.SName, C.ClassName FROM Student S LEFT JOIN Class C ON S.ClassID = C.ClassID;`
*   **執行後結果**:
    | SName | ClassName |
    | :---- | :-------- |
    | Ada   | Class 1A  |
    | Clem  | Class 1B  |
    | Eva   | Class 1B  |
    | Gabe  | Class 1C  |
    | Dio   | Class 1C  |
    | Ben   | NULL      |

#### RIGHT JOIN
*   **作用**: 返回右邊資料表的所有記錄，即使左邊沒有匹配的記錄 (Returns all records from the right table, and the matched records from the left table)。
*   **SQL 例子**: (假設我們新增了一個沒有學生的班級 `C04`) `SELECT S.SName, C.ClassName FROM Student S RIGHT JOIN Class C ON S.ClassID = C.ClassID;`
*   **執行後結果**:
    | SName | ClassName |
    | :---- | :-------- |
    | Ada   | Class 1A  |
    | Clem  | Class 1B  |
    | Eva   | Class 1B  |
    | Gabe  | Class 1C  |
    | Dio   | Class 1C  |
    | NULL  | Class 1D  |

> **注意 (Notes): `JOIN` 的分別 (Difference between JOINS)**
> *   `INNER JOIN` (可簡寫為 `JOIN`): 只會顯示兩張表都有對應嘅資料。就好似 Venn diagram 中間交集嘅部分。
> *   `LEFT JOIN`: 會顯示晒左邊表 (FROM 後面第一張表) 嘅所有資料，就算右邊表冇對應資料都照出，冇嘅部分會用 `NULL` 顯示。
> *   `RIGHT JOIN`: 同 `LEFT JOIN` 相反，會顯示晒右邊表嘅所有資料。

### Subqueries (子查詢)
*   **作用**: 在另一個 SQL 查詢中嵌套查詢 (A query nested inside another query)。子查詢的結果可以被主查詢使用。

#### Subquery with `IN`
*   **作用**: 判斷某個值是否存在於子查詢返回的結果列表中 (Checks if a value exists in the list of results returned by the subquery)。
*   **參考資料表 (Student & Class)**
*   **SQL 例子**: `SELECT SName FROM Student WHERE ClassID IN (SELECT ClassID FROM Class WHERE TeacherName = 'Mr. Bao');`
*   **執行後結果**:
    | SName |
    | :---- |
    | Ada   |

#### Subquery with `EXISTS`
*   **作用**: 判斷子查詢是否返回任何記錄。如果子查詢返回至少一行，`EXISTS` 返回 `TRUE` (Checks if the subquery returns any records. Returns `TRUE` if the subquery returns one or more rows)。
*   **參考資料表 (Student & Class)**
*   **SQL 例子 (找出有學生的班級)**:
    ```sql
    SELECT ClassName FROM Class c
    WHERE EXISTS (SELECT 1 FROM Student s WHERE s.ClassID = c.ClassID);
    ```
*   **執行後結果**:
    | ClassName |
    | :-------- |
    | Class 1A  |
    | Class 1B  |
    | Class 1C  |

> **注意 (Notes): `EXISTS` vs. `IN`**
> *   `IN` 將主查詢的值與子查詢返回的**值列表**進行比較 (compares a value against a **list of values**)。
> *   `EXISTS` 只是檢查子查詢是否返回**任何行**，它不關心返回的具體值是什麼，因此子查詢的 `SELECT` 列表通常寫成 `SELECT 1` 或 `SELECT *`，效率可能更高 (simply checks for the **existence of rows** and doesn't care about the values returned)。

#### Subquery with `ANY`
*   **作用**: 將一個值與子查詢返回的結果列表中的**任何一個**值進行比較。必須與比較運算子 (`=`, `>`, `<`, etc.) 一起使用 (Compares a value to **any** of the values in the list returned by the subquery)。
*   **參考資料表 (Student)**
*   **SQL 例子 (找出分數比 C03 班任何一個學生高的學生)**:
    ```sql
    SELECT SName, Score FROM Student
    WHERE Score > ANY (SELECT Score FROM Student WHERE ClassID = 'C03');
    ```
*   **執行後結果**: (因為 C03 班最低分是 65，所以會找出所有分數 > 65 的學生)
    | SName | Score |
    | :---- | :---- |
    | Ada   | 95    |
    | Clem  | 78    |
    | Eva   | 88    |
    | Dio   | 72    |
    | Ben   | 85    |

> **注意 (Notes): `= ANY` vs. `IN`**
> *   `= ANY` 在功能上等同於 `IN`。例如 `WHERE ClassID = ANY (SELECT ClassID ...)` 和 `WHERE ClassID IN (SELECT ClassID ...)` 的結果是一樣的。

#### Subquery with `ALL`
*   **作用**: 將一個值與子查詢返回的結果列表中的**所有**值進行比較。必須與比較運算子 (`=`, `>`, `<`, etc.) 一起使用 (Compares a value to **all** of the values in the list returned by the subquery)。
*   **參考資料表 (Student)**
*   **SQL 例子 (找出分數比 C03 班所有學生都高的學生)**:
    ```sql
    SELECT SName, Score FROM Student
    WHERE Score > ALL (SELECT Score FROM Student WHERE ClassID = 'C03');
    ```
*   **執行後結果**: (因為 C03 班最高分是 72，所以會找出所有分數 > 72 的學生)
    | SName | Score |
    | :---- | :---- |
    | Ada   | 95    |
    | Clem  | 78    |
    | Eva   | 88    |
    | Ben   | 85    |

### Set Operators (集合運算子)
*   **參考資料表 (Student)**: `SID`s: {'S01', 'S02', 'S03', 'S04', 'S05'}
*   **參考資料表 (OldStudent)** (一個假設的舊生表): `SID`s: {'S02', 'S04'}

#### UNION
*   **作用**: 合併結果集並移除重複項 (Combines result-sets and removes duplicates)。
*   **SQL 例子**: `SELECT ClassID FROM Student UNION SELECT ClassID FROM OldStudent;`
*   **執行後結果**:
    | ClassID |
    |:--------|
    | C01     |
    | C02     |
    | C03     |
    | NULL    |
#### UNION ALL
*   **作用**: 合併結果集並保留重複項 (Combines result-sets and includes duplicates)。
*   **SQL 例子**: `SELECT ClassID FROM Student UNION ALL SELECT ClassID FROM OldStudent;`
*   **執行後結果**:
    | ClassID |
    |:--------|
    | C01     |
    | C02     |
    | C02     |
    | C03     |
    | C03     |
    | NULL    |
    | C02     |
    | C03     |

> **注意 (Notes): `UNION` vs. `UNION ALL`**
> *   `UNION` 會好似 `DISTINCT` 咁，自動篩走重複嘅記錄。
> *   `UNION ALL` 就唔會篩走重複記錄，全部顯示晒，所以執行速度會比 `UNION` 快。

---
## 7. Views & Indexes (視圖與索引)

### CREATE VIEW
*   **作用**: 建立一個虛擬資料表，它是基於一個SQL語句的結果集 (Create a virtual table based on the result-set of an SQL statement)。
*   **SQL 例子**:
    ```sql
    CREATE VIEW StudentClassView AS
    SELECT S.SName, C.ClassName, C.TeacherName
    FROM Student S
    LEFT JOIN Class C ON S.ClassID = C.ClassID;
    ```
*   **執行後結果**: (一個名為 `StudentClassView` 的 view 被建立。你可以用 `SELECT * FROM StudentClassView;` 來查詢它)

### CREATE INDEX
*   **作用**: 在資料表上建立索引，可以大大加快查詢速度 (Create an index on a table to speed up searches and queries dramatically)。
*   **SQL 例子**: `CREATE INDEX idx_sname ON Student (SName);`
*   **執行後結果**: (在 `Student` 表的 `SName` 欄位上建立了一個索引，加快按姓名搜索的速度)
