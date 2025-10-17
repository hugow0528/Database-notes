

# SQL Notes  (Version2)

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
    *   [Subqueries (子查詢) - 詳細教學](#61-subqueries-子查詢---詳細教學)
        *   在 `WHERE` 子句中使用子查詢
        *   搭配 `IN` / `NOT IN` 的子查詢
        *   搭配 `EXISTS` / `NOT EXISTS` 的子查詢
        *   搭配比較運算子的子查詢 (`ANY`, `ALL`)
        *   在 `FROM` 子句中使用子查詢 (衍生資料表)
        *   在 `SELECT` 子句中使用子查詢 (純量-子查詢)
    *   [Table Joins (資料表連接)](#table-joins-資料表連接)
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
*   **執行後結果**: (一個名為 `SchoolDB` 的新資料庫被建立)

### USE
*   **作用**: 選擇要操作的資料庫 (Select a database to use)。
*   **SQL 例子**:
    ```sql
    USE SchoolDB;
    ```
*   **執行後結果**: (之後所有的操作都會在 `SchoolDB` 資料庫中進行)

### CREATE TABLE
*   **作用**: 在當前資料庫中建立一個新的資料表 (Create a new table)。
*   **SQL 例子**:
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
*   **執行後結果**: (建立了 `Class` 和 `Student` 兩個資料表，並定義了相關的約束)

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
*   **執行後結果 (結構改變)**: (`SName` 欄位的最大長度變為 100)
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
*接下來的所有例子，我們都會使用以下已填充資料的 `Class` 和 `Student` 表。*

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
---
## 2. Data Manipulation Language (DML) - 資料操作語言

### INSERT INTO
*   **作用**: 向資料表中插入新的記錄 (Insert new records into a table)。
*   **SQL 例子**:
    ```sql
    INSERT INTO Student (SID, SName, ClassID, DOB, Score) 
    VALUES ('S07', 'Finn', 'C01', '2005-04-12', 91);
    ```
*   **執行後結果 (Student Table 新增一行)**:
    | SID | SName | ClassID | DOB        | Score |
    |:--- |:---- |:------ |:--------- |:---- |
    | S07 | Finn  | C01     | 2005-04-12 | 91    |

### UPDATE
*   **作用**: 修改資料表中的現有記錄 (Modify existing records in a table)。
*   **SQL 例子**:
    ```sql
    UPDATE Student
    SET Score = 80, ClassID = 'C02'
    WHERE SID = 'S04';
    ```
*   **執行後結果 (Student 表中 Gabe 的記錄被更新)**:
    | SID | SName | ClassID | DOB        | Score |
    |:--- |:---- |:------ |:--------- |:---- |
    | S04 | Gabe  | C02     | 2005-01-15 | 80    |

### DELETE
*   **作用**: 從資料表中刪除記錄 (Delete records from a table)。
*   **SQL 例子**:
    ```sql
    DELETE FROM Student
    WHERE SID = 'S07';
    ```
*   **執行後結果**: (SID 為 'S07' 的 Finn 的記錄被刪除)

---

## 3. Data Query Language (DQL) - 資料查詢語言

### SELECT...FROM
*   **作用**: 從一個或多個資料表中檢索資料 (Retrieve data from one or more tables)。
*   **SQL 例子**: `SELECT SName, Score FROM Student;`
*   **執行後結果**:
    | SName | Score |
    | :---- | :---- |
    | Ada   | 95    |
    | Clem  | 78    |
    | Eva   | 88    |
    | Gabe  | 65    |
    | Dio   | 72    |
    | Ben   | 85    |

### SELECT DISTINCT
*   **作用**: 只返回唯一的值，過濾掉重複的行 (Return only distinct (different) values)。
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
*   **SQL 例子**: `SELECT SName AS 'Student Name', Score FROM Student;`
*   **執行後結果**:
    | Student Name | Score |
    | :----------- | :---- |
    | Ada          | 95    |
    | ...          | ...   |

### WHERE
*   **作用**: 根據指定的條件過濾記錄 (Filter records based on specified criteria)。
*   **SQL 例子**: `SELECT SName, Score FROM Student WHERE Score > 80;`
*   **執行後結果**:
    | SName | Score |
    | :---- | :---- |
    | Ada   | 95    |
    | Eva   | 88    |
    | Ben   | 85    |

### ORDER BY
*   **作用**: 對結果集按一個或多個欄位進行排序 (Sort the result-set by one or more columns)。 `ASC` (升序，預設) 或 `DESC` (降序)。
*   **SQL 例子**: `SELECT SName, Score FROM Student ORDER BY Score DESC;`
*   **執行後結果**:
    | SName | Score |
    | :---- | :---- |
    | Ada   | 95    |
    | Eva   | 88    |
    | Ben   | 85    |
    | Clem  | 78    |
    | Dio   | 72    |
    | Gabe  | 65    |

---

## 4. Operators (運算子)
(所有例子都基於 `Student` 表)

### Arithmetic Operators (算術運算子)
*   **作用**: 執行數學運算 (Perform mathematical operations)。
*   **SQL 例子**: `SELECT SName, Score, Score - 5 AS AdjustedScore FROM Student WHERE SID = 'S01';`
*   **執行後結果**:
    | SName | Score | AdjustedScore |
    | :---- | :---- | :------------ |
    | Ada   | 95    | 90            |

### Logical Operators (邏輯運算子)
*   **AND, OR, NOT**: `SELECT SName FROM Student WHERE Score > 80 AND ClassID = 'C02';`
    *   **結果**: Eva
*   **BETWEEN**: `SELECT SName, Score FROM Student WHERE Score BETWEEN 70 AND 80;`
    *   **結果**: Clem (78), Dio (72)
*   **IN**: `SELECT SName, ClassID FROM Student WHERE ClassID IN ('C01', 'C03');`
    *   **結果**: Ada (C01), Gabe (C03), Dio (C03)
*   **LIKE**: `SELECT SName FROM Student WHERE SName LIKE 'A%';`
    *   **結果**: Ada
*   **IS NULL**: `SELECT SName FROM Student WHERE ClassID IS NULL;`
    *   **結果**: Ben

---
## 5. Functions (函數)
(所有例子都基於 `Student` 表)

### Aggregate Functions (聚合函數)
*   **COUNT()**: `SELECT COUNT(SID) AS TotalStudents FROM Student;` -> **結果**: 6
*   **SUM()**: `SELECT SUM(Score) AS TotalScore FROM Student;` -> **結果**: 483
*   **AVG()**: `SELECT AVG(Score) AS AverageScore FROM Student;` -> **結果**: 80.5
*   **MAX()**: `SELECT MAX(Score) AS HighestScore FROM Student;` -> **結果**: 95
*   **MIN()**: `SELECT MIN(Score) AS LowestScore FROM Student;` -> **結果**: 65

### Text Functions (文字函數)
*   **SQL 例子**: `SELECT UPPER(SName) as UCName, LENGTH(SName) as NameLen FROM Student WHERE SID = 'S02';`
*   **執行後結果**:
    | UCName | NameLen |
    | :----- | :------ |
    | CLEM   | 4       |

### Date Functions (日期函數)
*   **SQL 例子**: `SELECT SName, YEAR(DOB) as BirthYear FROM Student WHERE SID = 'S03';`
*   **執行後結果**:
    | SName | BirthYear |
    | :---- | :-------- |
    | Eva   | 2004      |

---

## 6. Advanced Querying (進階查詢)

### GROUP BY
*   **作用**: 結合聚合函數，根據一個或多個欄位對結果集進行分組 (Group rows that have the same values into summary rows)。
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
*   **SQL 例子**: `SELECT ClassID, AVG(Score) AS AvgScore FROM Student GROUP BY ClassID HAVING AVG(Score) < 70;`
*   **執行後結果**:
    | ClassID | AvgScore |
    | :------ | :------- |
    | C03     | 68.5     |

> **注意 (Notes): `WHERE` vs. `HAVING`**
> *   `WHERE` 係喺分組 (`GROUP BY`) 前用嚟過濾單筆記錄 (filters individual rows **before** grouping)。
> *   `HAVING` 係喺分組後用嚟過濾聚合後嘅結果 (filters groups **after** aggregation)。你唔可以喺 `WHERE` 子句度用聚合函數 (e.g., `COUNT()`, `SUM()`)。

### 6.1 Subqueries (子查詢) - 詳細教學
子查詢 (Subquery) 或稱為內部查詢 (Inner Query)，是一個嵌套在另一個 SQL 查詢（例如 `SELECT`, `INSERT`, `UPDATE` 或 `DELETE`）中的查詢。它可以幫助我們解決需要分步解決的複雜問題。

#### 在 `WHERE` 子句中使用子查詢
這是最常見的用法。子查詢返回一個單一值或一個列表，供外部查詢的 `WHERE` 子句進行比較。

*   **作用**: 找出分數高於平均分的學生 (Find students whose score is higher than the average score)。
*   **SQL 例子**:
    ```sql
    SELECT SName, Score
    FROM Student
    WHERE Score > (SELECT AVG(Score) FROM Student);
    ```
*   **執行後結果**: (平均分是 80.5)
    | SName | Score |
    | :---- | :---- |
    | Ada   | 95    |
    | Eva   | 88    |
    | Ben   | 85    |

#### 搭配 `IN` / `NOT IN` 的子查詢
*   **作用**: 檢查某個值是否存在於子查詢返回的結果列表中 (Check if a value exists in the list returned by the subquery)。
*   **SQL 例子**: 找出所有由 'Mr. Bao' 教的班級中的學生 (Find all students in classes taught by 'Mr. Bao')。
    ```sql
    SELECT SName
    FROM Student
    WHERE ClassID IN (SELECT ClassID FROM Class WHERE TeacherName = 'Mr. Bao');
    ```
*   **執行後結果**:
    | SName |
    | :---- |
    | Ada   |

#### 搭配 `EXISTS` / `NOT EXISTS` 的子查詢
*   **作用**: 檢查子查詢是否返回任何記錄。如果子查詢返回至少一行，`EXISTS` 為 TRUE (Check if the subquery returns any rows)。這通常與**關聯子查詢 (Correlated Subquery)** 一起使用，即內部查詢依賴於外部查詢的值。
*   **SQL 例子**: 找出所有有學生的班級 (Find all classes that have at least one student)。
    ```sql
    SELECT ClassName
    FROM Class c
    WHERE EXISTS (SELECT 1 FROM Student s WHERE s.ClassID = c.ClassID);
    ```
*   **執行後結果**:
    | ClassName |
    | :-------- |
    | Class 1A  |
    | Class 1B  |
    | Class 1C  |

> **注意 (Notes): `IN` vs. `EXISTS`**
> *   `IN` 將外部查詢的值與子查詢返回的**結果列表**進行比較。子查詢只執行一次。
> *   `EXISTS` 檢查子查詢是否**返回任何行**，它不關心返回的是什麼值。對於關聯子查詢，它會為外部查詢的每一行都執行一次。在子查詢結果非常大時，`EXISTS` 通常更有效率，因為只要找到一行匹配，它就會停止。

#### 搭配比較運算子的子查詢 (`ANY`, `ALL`)
*   **作用**: 將一個值與子查詢返回的列表中的 `ANY`(任何一個) 或 `ALL`(所有) 值進行比較。
*   **SQL 例子 (`> ANY`)**: 找出分數比 `C03` 班**任何一個**學生高的學生 (Find students with a score higher than *any* student in class C03)。這等同於分數高於 C03 班的最低分 (65)。
    ```sql
    SELECT SName, Score
    FROM Student
    WHERE Score > ANY (SELECT Score FROM Student WHERE ClassID = 'C03');
    ```
*   **執行後結果**:
    | SName | Score |
    | :---- | :---- |
    | Ada   | 95    |
    | Clem  | 78    |
    | Eva   | 88    |
    | Dio   | 72    |
    | Ben   | 85    |
*   **SQL 例子 (`> ALL`)**: 找出分數比 `C03` 班**所有**學生都高的學生 (Find students with a score higher than *all* students in class C03)。這等同於分數高於 C03 班的最高分 (72)。
    ```sql
    SELECT SName, Score
    FROM Student
    WHERE Score > ALL (SELECT Score FROM Student WHERE ClassID = 'C03');
    ```
*   **執行後結果**:
    | SName | Score |
    | :---- | :---- |
    | Ada   | 95    |
    | Clem  | 78    |
    | Eva   | 88    |
    | Ben   | 85    |

#### 在 `FROM` 子句中使用子查詢 (衍生資料表)
*   **作用**: 將子查詢的結果作為一個臨時的、虛擬的資料表來使用，外部查詢可以像查詢普通資料表一樣查詢它。這個臨時表必須有一個別名 (alias)。
*   **SQL 例子**: 找出每個班級的平均分，並只顯示平均分高於 70 的班級。
    ```sql
    SELECT ClassID, AvgScore
    FROM (SELECT ClassID, AVG(Score) AS AvgScore FROM Student GROUP BY ClassID) AS ClassAverages
    WHERE AvgScore > 70;
    ```*   **執行後結果**:
    | ClassID | AvgScore |
    | :------ | :------- |
    | C01     | 95.0     |
    | C02     | 83.0     |
    | NULL    | 85.0     |

#### 在 `SELECT` 子句中使用子查詢 (純量子查詢 - Scalar Subquery)
*   **作用**: 子查詢返回一個單一值（一行一列），作為外部查詢結果集的一個欄位。
*   **SQL 例子**: 顯示每個學生的姓名和他們班級的老師姓名。
    ```sql
    SELECT 
        SName,
        (SELECT TeacherName FROM Class WHERE ClassID = Student.ClassID) AS Teacher
    FROM Student;
    ```
*   **執行後結果**:
    | SName | Teacher   |
    | :---- | :-------- |
    | Ada   | Mr. Bao   |
    | Clem  | Mrs. Au   |
    | Eva   | Mrs. Au   |
    | Gabe  | Ms. Chan  |
    | Dio   | Ms. Chan  |
    | Ben   | NULL      |
> **注意 (Notes):** 雖然純量子查詢很方便，但如果外部查詢返回很多行，它可能會導致效能問題，因為子查詢需要為每一行都執行一次。在這種情況下，使用 `JOIN` 通常是更好的選擇。

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

> **注意 (Notes): `JOIN` 的分別 (Difference between JOINS)**
> *   `INNER JOIN` (可簡寫為 `JOIN`): 只會顯示兩張表都有對應嘅資料。就好似 Venn diagram 中間交集嘅部分。
> *   `LEFT JOIN`: 會顯示晒左邊表 (FROM 後面第一張表) 嘅所有資料，就算右邊表冇對應資料都照出，冇嘅部分會用 `NULL` 顯示。
> *   `RIGHT JOIN`: 同 `LEFT JOIN` 相反，會顯示晒右邊表嘅所有資料。

### Set Operators (集合運算子)
*   **參考資料表 `Student`**: `SID`s: {'S01', 'S02', 'S03'}
*   **參考資料表 `Alumni` (舊生)**: `SID`s: {'S03', 'S04'}

#### UNION
*   **作用**: 合併結果集並移除重複項 (Combines result-sets and removes duplicates)。
*   **SQL 例子**: `SELECT SID FROM Student UNION SELECT SID FROM Alumni;`
*   **執行後結果**:
    | SID |
    |:----|
    | S01 |
    | S02 |
    | S03 |
    | S04 |
#### UNION ALL
*   **作用**: 合併結果集並保留重複項 (Combines result-sets and includes duplicates)。
*   **SQL 例子**: `SELECT SID FROM Student UNION ALL SELECT SID FROM Alumni;`
*   **執行後結果**:
    | SID |
    |:----|
    | S01 |
    | S02 |
    | S03 |
    | S03 |
    | S04 |
> **注意 (Notes): `UNION` vs. `UNION ALL`**
> *   `UNION` 會好似 `DISTINCT` 咁，自動篩走重複嘅記錄。
> *   `UNION ALL` 就唔會篩走重複記錄，全部顯示晒，所以執行速度會比 `UNION` 快。

---
## 7. Views & Indexes (視圖與索引)

### CREATE VIEW
*   **作用**: 建立一個虛擬資料表，它是基於一個SQL語句的結果集 (Create a virtual table based on the result-set of an SQL statement)。
*   **SQL 例子**:
    ```sql
    CREATE VIEW StudentDetails AS
    SELECT S.SName, C.ClassName, C.TeacherName, S.Score
    FROM Student S
    LEFT JOIN Class C ON S.ClassID = C.ClassID;
    ```
*   **執行後結果**: (一個名為 `StudentDetails` 的 view 被建立。你可以用 `SELECT * FROM StudentDetails WHERE Score > 90;` 來查詢它)

### CREATE INDEX
*   **作用**: 在資料表上建立索引，可以大大加快查詢速度 (Create an index on a table to speed up searches and queries dramatically)。
*   **SQL 例子**: `CREATE INDEX idx_sname ON Student (SName);`
*   **執行後結果**: (在 `Student` 表的 `SName` 欄位上建立了一個索引，加快按姓名搜索的速度)
