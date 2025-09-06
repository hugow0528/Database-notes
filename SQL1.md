
### **Example Database Tables**

To make the examples clear, we'll use these two simple tables:

**STUDENT Table**

| SID (學生ID) | NAME (姓名) | CLASS (班級) | CNO (班號) | SCORE (分數) |
| :----------: | :---------: | :----------: | :--------: | :----------: |
|    S001    |    Alice    |      1A      |     1      |      85      |
|    S002    |     Bob     |      1A      |     2      |      78      |
|    S003    |   Charlie   |      1B      |     1      |      92      |
|    S004    |    David    |      1B      |     2      |      70      |
|    S005    |     Eve     |      1A      |     1      |      85      |

**COURSE Table**

| CID (課程ID) | CNAME (課程名稱) | CREDITS (學分) |
| :----------: | :--------------: | :------------: |
|    CS101     |    Computer Sci    |       3        |
|    MA201     |      Maths       |       4        |
|    EN301     |      English     |       3        |

---

### 1. Database and Table Management (DDL - Data Definition Language)
(數據定義語言)

These statements are used to define, modify, or delete database objects like databases, tables, and indexes.
這些語句用於定義、修改或刪除數據庫對象，例如數據庫、表和索引。

#### `CREATE DATABASE <database_name>`
*   **English Explanation:** Creates a new database.
*   **廣東話解釋:** 建立一個新嘅數據庫。
*   **Sample Usage:**
    ```sql
    CREATE DATABASE SchoolDB;
    ```
*   **Execute Result:** A new database named `SchoolDB` is created.
*   **執行結果:** 建立咗一個叫做 `SchoolDB` 嘅新數據庫。

#### `USE <database_name>`
*   **English Explanation:** Selects which database to work with.
*   **廣東話解釋:** 揀選要操作嘅數據庫。
*   **Sample Usage:**
    ```sql
    USE SchoolDB;
    ```
*   **Execute Result:** All subsequent commands will operate within `SchoolDB`.
*   **執行結果:** 之後所有指令都會喺 `SchoolDB` 數據庫入面執行。

#### `CREATE TABLE <table_name> (...)`
*   **English Explanation:** Creates a new table within the current database with specified columns and data types. You can also define constraints here.
*   **廣東話解釋:** 喺目前數據庫入面建立一個新嘅表，並定義好欄位同數據類型。你亦可以喺度定義約束。
*   **Sample Usage:**
    ```sql
    CREATE TABLE STUDENT (
        SID CHAR(5) PRIMARY KEY,
        NAME VARCHAR(40) NOT NULL,
        CLASS CHAR(2),
        CNO INT,
        SCORE INT,
        CHECK (SCORE >= 0 AND SCORE <= 100)
    );
    ```
*   **Execute Result:** The `STUDENT` table is created.
*   **執行結果:** `STUDENT` 表格被建立。

#### `SHOW DATABASES` / `SHOW TABLES`
*   **English Explanation:** Lists all databases or all tables in the current database.
*   **廣東話解釋:** 列出所有數據庫，或者目前數據庫中嘅所有表。
*   **Sample Usage:**
    ```sql
    SHOW DATABASES;
    SHOW TABLES;
    ```
*   **Execute Result (Example):**
    ```
    DATABASES
    ----------
    SchoolDB
    mysql
    ...

    TABLES_IN_SCHOOLD
    -----------------
    STUDENT
    COURSE
    ```
*   **執行結果 (例子):**
    ```
    DATABASES
    ----------
    SchoolDB
    mysql
    ...

    TABLES_IN_SCHOOLD
    -----------------
    STUDENT
    COURSE
    ```

#### `ALTER TABLE <table_name> ADD <field_name> <data_type> [constraints]`
*   **English Explanation:** Adds a new column to an existing table.
*   **廣東話解釋:** 為現有嘅表增加一個新嘅欄位。
*   **Sample Usage:**
    ```sql
    ALTER TABLE STUDENT ADD EMAIL VARCHAR(100);
    ```
*   **Execute Result:** The `STUDENT` table now has an `EMAIL` column.
    **STUDENT Table (after ALTER ADD)**
    | SID | NAME | CLASS | CNO | SCORE | EMAIL |
    | :--: | :--: | :---: | :-: | :---: | :---: |
    | S001 | Alice | 1A | 1 | 85 | NULL |
    | ... | ... | ... | ... | ... | ... |
*   **執行結果:** `STUDENT` 表格而家多咗一個 `EMAIL` 欄位。
    **STUDENT 表格 (ALTER ADD 後)**
    | SID | NAME | CLASS | CNO | SCORE | EMAIL |
    | :--: | :--: | :---: | :-: | :---: | :---: |
    | S001 | Alice | 1A | 1 | 85 | NULL |
    | ... | ... | ... | ... | ... | ... |

#### `ALTER TABLE <table_name> DROP COLUMN <field_name>`
*   **English Explanation:** Deletes a column from an existing table.
*   **廣東話解釋:** 從現有嘅表刪除一個欄位。
*   **Sample Usage:**
    ```sql
    ALTER TABLE STUDENT DROP COLUMN EMAIL;
    ```
*   **Execute Result:** The `EMAIL` column is removed from the `STUDENT` table.
*   **執行結果:** `EMAIL` 欄位從 `STUDENT` 表格中被刪除。

#### `DROP TABLE <table_name>`
*   **English Explanation:** Deletes an entire table.
*   **廣東話解釋:** 刪除成個表。
*   **Sample Usage:**
    ```sql
    DROP TABLE COURSE;
    ```
*   **Execute Result:** The `COURSE` table is permanently deleted.
*   **執行結果:** `COURSE` 表格被永久刪除。

#### `DROP DATABASE <database_name>`
*   **English Explanation:** Deletes an entire database. Use with extreme caution!
*   **廣東話解釋:** 刪除成個數據庫。請務必小心使用！
*   **Sample Usage:**
    ```sql
    DROP DATABASE SchoolDB;
    ```
*   **Execute Result:** The `SchoolDB` database and all its tables are deleted.
*   **執行結果:** `SchoolDB` 數據庫及其所有表格都被刪除。

---

### 2. Data Manipulation (DML - Data Manipulation Language)
(數據操縱語言)

These statements are used for managing data within the schema objects.
這些語句用於管理模式對象內嘅數據。

#### `INSERT INTO <table_name> (...) VALUES (...)`
*   **English Explanation:** Adds new rows (records) of data into a table.
*   **廣東話解釋:** 將新嘅行（記錄）數據插入到一個表入面。
*   **Sample Usage:**
    ```sql
    INSERT INTO STUDENT (SID, NAME, CLASS, CNO, SCORE)
    VALUES ('S001', 'Alice', '1A', 1, 85);

    INSERT INTO STUDENT VALUES ('S002', 'Bob', '1A', 2, 78); -- If inserting all columns
    ```
*   **Execute Result:** New records are added to the `STUDENT` table.
    **STUDENT Table (after INSERT)**
    | SID | NAME | CLASS | CNO | SCORE |
    | :--: | :--: | :---: | :-: | :---: |
    | S001 | Alice | 1A | 1 | 85 |
    | S002 | Bob | 1A | 2 | 78 |
*   **執行結果:** 新記錄被添加到 `STUDENT` 表格。
    **STUDENT 表格 (INSERT 後)**
    | SID | NAME | CLASS | CNO | SCORE |
    | :--: | :--: | :---: | :-: | :---: |
    | S001 | Alice | 1A | 1 | 85 |
    | S002 | Bob | 1A | 2 | 78 |

#### `SELECT ... FROM <table_name>`
*   **English Explanation:** Retrieves data from one or more tables. This is the most frequently used SQL command.
*   **廣東話解釋:** 從一個或多個表入面檢索數據。呢個係最常用嘅 SQL 指令。

    *   **`SELECT *`**: Retrieves all columns.
        *   **廣東話解釋:** 檢索所有欄位。
        *   **Sample Usage:**
            ```sql
            SELECT * FROM STUDENT;
            ```
        *   **Execute Result:** Displays all data from the `STUDENT` table.
            **STUDENT Table**
            | SID | NAME | CLASS | CNO | SCORE |
            | :--: | :--: | :---: | :-: | :---: |
            | S001 | Alice | 1A | 1 | 85 |
            | S002 | Bob | 1A | 2 | 78 |
            | S003 | Charlie | 1B | 1 | 92 |
            | S004 | David | 1B | 2 | 70 |
            | S005 | Eve | 1A | 1 | 85 |
        *   **執行結果:** 顯示 `STUDENT` 表格嘅所有數據。
            **STUDENT 表格**
            | SID | NAME | CLASS | CNO | SCORE |
            | :--: | :--: | :---: | :-: | :---: |
            | S001 | Alice | 1A | 1 | 85 |
            | S002 | Bob | 1A | 2 | 78 |
            | S003 | Charlie | 1B | 1 | 92 |
            | S004 | David | 1B | 2 | 70 |
            | S005 | Eve | 1A | 1 | 85 |

    *   **`SELECT <column1>, <column2>`**: Retrieves specific columns.
        *   **廣東話解釋:** 檢索特定欄位。
        *   **Sample Usage:**
            ```sql
            SELECT NAME, SCORE FROM STUDENT;
            ```
        *   **Execute Result:**
            | NAME | SCORE |
            | :--: | :---: |
            | Alice | 85 |
            | Bob | 78 |
            | Charlie | 92 |
            | David | 70 |
            | Eve | 85 |
        *   **執行結果:**
            | NAME | SCORE |
            | :--: | :---: |
            | Alice | 85 |
            | Bob | 78 |
            | Charlie | 92 |
            | David | 70 |
            | Eve | 85 |

    *   **`WHERE <condition>`**: Filters rows based on criteria.
        *   **廣東話解釋:** 根據條件篩選行。
        *   **Sample Usage:**
            ```sql
            SELECT NAME, SCORE FROM STUDENT WHERE CLASS = '1A';
            ```
        *   **Execute Result:**
            | NAME | SCORE |
            | :--: | :---: |
            | Alice | 85 |
            | Bob | 78 |
            | Eve | 85 |
        *   **執行結果:**
            | NAME | SCORE |
            | :--: | :---: |
            | Alice | 85 |
            | Bob | 78 |
            | Eve | 85 |

    *   **`ORDER BY <column> [ASC|DESC]`**: Sorts the results. `ASC` is default (ascending), `DESC` is descending.
        *   **廣東話解釋:** 排序結果。`ASC` 係預設（升序），`DESC` 係降序。
        *   **Sample Usage:**
            ```sql
            SELECT NAME, SCORE FROM STUDENT ORDER BY SCORE DESC;
            ```
        *   **Execute Result:**
            | NAME | SCORE |
            | :------: | :---: |
            | Charlie | 92 |
            | Alice | 85 |
            | Eve | 85 |
            | Bob | 78 |
            | David | 70 |
        *   **執行結果:**
            | NAME | SCORE |
            | :------: | :---: |
            | Charlie | 92 |
            | Alice | 85 |
            | Eve | 85 |
            | Bob | 78 |
            | David | 70 |

    *   **`DISTINCT <column>`**: Returns only unique values in a column.
        *   **廣東話解釋:** 只返回欄位中嘅唯一值。
        *   **Sample Usage:**
            ```sql
            SELECT DISTINCT CLASS FROM STUDENT;
            ```
        *   **Execute Result:**
            | CLASS |
            | :---: |
            | 1A |
            | 1B |
        *   **執行結果:**
            | CLASS |
            | :---: |
            | 1A |
            | 1B |

    *   **`GROUP BY <column>`**: Groups rows that have the same values in specified columns into summary rows. Often used with aggregate functions.
        *   **廣東話解釋:** 將指定欄位中具有相同值嘅行分組為匯總行。通常與聚合函數一起使用。
        *   **Sample Usage:**
            ```sql
            SELECT CLASS, AVG(SCORE) AS AverageScore
            FROM STUDENT
            GROUP BY CLASS;
            ```
        *   **Execute Result:**
            | CLASS | AverageScore |
            | :---: | :----------: |
            | 1A | 82.66 |
            | 1B | 81 |
        *   **執行結果:**
            | CLASS | AverageScore |
            | :---: | :----------: |
            | 1A | 82.66 |
            | 1B | 81 |

    *   **`HAVING <condition>`**: Filters groups based on conditions after `GROUP BY`.
        *   **廣東話解釋:** 喺 `GROUP BY` 之後，根據條件篩選分組。
        *   **Sample Usage:**
            ```sql
            SELECT CLASS, AVG(SCORE) AS AverageScore
            FROM STUDENT
            GROUP BY CLASS
            HAVING AVG(SCORE) > 80;
            ```
        *   **Execute Result:**
            | CLASS | AverageScore |
            | :---: | :----------: |
            | 1A | 82.66 |
            | 1B | 81 |
        *   **執行結果:**
            | CLASS | AverageScore |
            | :---: | :----------: |
            | 1A | 82.66 |
            | 1B | 81 |

#### `UPDATE <table_name> SET <column> = <value> WHERE <condition>`
*   **English Explanation:** Modifies existing data in a table. If `WHERE` is omitted, all rows will be updated.
*   **廣東話解釋:** 修改表中現有嘅數據。如果省略 `WHERE` 子句，所有行都會被更新。
*   **Sample Usage:**
    ```sql
    UPDATE STUDENT SET SCORE = 90 WHERE SID = 'S001';
    ```
*   **Execute Result:** Alice's score is updated.
    **STUDENT Table (after UPDATE)**
    | SID | NAME | CLASS | CNO | SCORE |
    | :--: | :--: | :---: | :-: | :---: |
    | S001 | Alice | 1A | 1 | **90** |
    | S002 | Bob | 1A | 2 | 78 |
    | ... | ... | ... | ... | ... |
*   **執行結果:** Alice 嘅分數被更新。
    **STUDENT 表格 (UPDATE 後)**
    | SID | NAME | CLASS | CNO | SCORE |
    | :--: | :--: | :---: | :-: | :---: |
    | S001 | Alice | 1A | 1 | **90** |
    | S002 | Bob | 1A | 2 | 78 |
    | ... | ... | ... | ... | ... |

#### `DELETE FROM <table_name> WHERE <condition>`
*   **English Explanation:** Deletes rows from a table. If `WHERE` is omitted, all rows will be deleted.
*   **廣東話解釋:** 從表中刪除行。如果省略 `WHERE` 子句，所有行都會被刪除。
*   **Sample Usage:**
    ```sql
    DELETE FROM STUDENT WHERE SID = 'S004';
    ```
*   **Execute Result:** The record for David (S004) is deleted.
    **STUDENT Table (after DELETE)**
    | SID | NAME | CLASS | CNO | SCORE |
    | :--: | :--: | :---: | :-: | :---: |
    | S001 | Alice | 1A | 1 | 85 |
    | S002 | Bob | 1A | 2 | 78 |
    | S003 | Charlie | 1B | 1 | 92 |
    | S005 | Eve | 1A | 1 | 85 |
*   **執行結果:** David (S004) 嘅記錄被刪除。
    **STUDENT 表格 (DELETE 後)**
    | SID | NAME | CLASS | CNO | SCORE |
    | :--: | :--: | :---: | :-: | :---: |
    | S001 | Alice | 1A | 1 | 85 |
    | S002 | Bob | 1A | 2 | 78 |
    | S003 | Charlie | 1B | 1 | 92 |
    | S005 | Eve | 1A | 1 | 85 |

---

### 3. SQL Operators and Functions
(SQL 運算符同函數)

These enhance your ability to filter, calculate, and format data.
呢啲嘢可以增強你篩選、計算同格式化數據嘅能力。

#### Comparison Operators (`=`, `!=`, `>`, `<`, `>=`, `<=`)
*   **English Explanation:** Used in `WHERE` clauses to compare values.
*   **廣東話解釋:** 喺 `WHERE` 子句入面用嚟比較數值。
*   **Sample Usage:**
    ```sql
    SELECT NAME FROM STUDENT WHERE SCORE >= 85;
    ```
*   **Execute Result:**
    | NAME |
    | :------: |
    | Alice |
    | Charlie |
    | Eve |
*   **執行結果:**
    | NAME |
    | :------: |
    | Alice |
    | Charlie |
    | Eve |

#### Logical Operators (`AND`, `OR`, `NOT`)
*   **English Explanation:** Used to combine or negate conditions in `WHERE` clauses.
*   **廣東話解釋:** 喺 `WHERE` 子句入面用嚟組合或否定條件。
*   **Sample Usage:**
    ```sql
    SELECT NAME FROM STUDENT WHERE CLASS = '1A' AND SCORE > 80;
    ```
*   **Execute Result:**
    | NAME |
    | :--: |
    | Alice |
    | Eve |
*   **執行結果:**
    | NAME |
    | :--: |
    | Alice |
    | Eve |

#### `IN` / `NOT IN`
*   **English Explanation:** Checks if a value matches any value in a list.
*   **廣東話解釋:** 檢查一個值係咪匹配列表中嘅任何值。
*   **Sample Usage:**
    ```sql
    SELECT NAME FROM STUDENT WHERE CLASS IN ('1A', '1B');
    ```
*   **Execute Result:**
    | NAME |
    | :------: |
    | Alice |
    | Bob |
    | Charlie |
    | David |
    | Eve |
*   **執行結果:**
    | NAME |
    | :------: |
    | Alice |
    | Bob |
    | Charlie |
    | David |
    | Eve |

#### `BETWEEN ... AND ...`
*   **English Explanation:** Selects values within a specified range (inclusive).
*   **廣東話解釋:** 選取指定範圍內（包括兩端）嘅值。
*   **Sample Usage:**
    ```sql
    SELECT NAME, SCORE FROM STUDENT WHERE SCORE BETWEEN 75 AND 85;
    ```
*   **Execute Result:**
    | NAME | SCORE |
    | :--: | :---: |
    | Alice | 85 |
    | Bob | 78 |
    | Eve | 85 |
*   **執行結果:**
    | NAME | SCORE |
    | :--: | :---: |
    | Alice | 85 |
    | Bob | 78 |
    | Eve | 85 |

#### `LIKE` (with wildcards `%`, `_`)
*   **English Explanation:** Searches for specific patterns in a column.
    *   `%`: Represents zero or more characters.
    *   `_`: Represents a single character.
*   **廣東話解釋:** 喺欄位中搜索特定模式。
    *   `%`: 代表零個或多個字符。
    *   `_`: 代表單個字符。
*   **Sample Usage:**
    ```sql
    SELECT NAME FROM STUDENT WHERE NAME LIKE 'A%'; -- Names starting with 'A'
    SELECT NAME FROM STUDENT WHERE NAME LIKE '_o_'; -- Names with 'o' as the second character
    ```
*   **Execute Result (`'A%'`):**
    | NAME |
    | :--: |
    | Alice |
*   **執行結果 (`'A%'`):**
    | NAME |
    | :--: |
    | Alice |

#### `IS NULL` / `IS NOT NULL`
*   **English Explanation:** Checks for null or non-null values.
*   **廣東話解釋:** 檢查係咪為空值或者非空值。
*   **Sample Usage:**
    ```sql
    SELECT SID, NAME FROM STUDENT WHERE CNO IS NULL; -- Assuming some CNOs could be NULL
    ```
*   **Execute Result:** If no `CNO` is `NULL` in the sample data, this would return an empty set.
*   **執行結果:** 如果樣本數據中冇 `CNO` 係 `NULL`，咁會返回空集。

#### Aggregate Functions (`COUNT`, `MAX`, `MIN`, `SUM`, `AVG`)
*   **English Explanation:** Perform calculations on a set of rows and return a single value.
*   **廣東話解釋:** 對一組行執行計算並返回一個單一值。
*   **Sample Usage:**
    ```sql
    SELECT COUNT(*) AS TotalStudents, MAX(SCORE) AS HighestScore, AVG(SCORE) AS AverageScore
    FROM STUDENT;
    ```
*   **Execute Result:**
    | TotalStudents | HighestScore | AverageScore |
    | :-----------: | :----------: | :----------: |
    | 5 | 92 | 82 |
*   **執行結果:**
    | TotalStudents | HighestScore | AverageScore |
    | :-----------: | :----------: | :----------: |
    | 5 | 92 | 82 |

---

### 4. Joining Tables
(連接表)

Joins are used to combine rows from two or more tables, based on a related column between them.
連接用於根據表之間嘅相關欄位，將兩個或多個表嘅行組合起嚟。

#### `INNER JOIN` (or just `JOIN`)
*   **English Explanation:** Returns only the rows that have matching values in both tables.
*   **廣東話解釋:** 只返回兩個表中都有匹配值嘅行。
*   **Example Database Tables (new for join example):**
    **STUDENT_COURSE Table**
    | SID (學生ID) | CID (課程ID) |
    | :----------: | :----------: |
    |    S001    |    CS101     |
    |    S001    |    MA201     |
    |    S002    |    CS101     |
    |    S003    |    EN301     |

*   **Sample Usage:** (Joining `STUDENT` with `STUDENT_COURSE`)
    ```sql
    SELECT S.NAME, SC.CID
    FROM STUDENT S
    INNER JOIN STUDENT_COURSE SC ON S.SID = SC.SID;
    ```
*   **Execute Result:**
    | NAME | CID |
    | :--: | :--: |
    | Alice | CS101 |
    | Alice | MA201 |
    | Bob | CS101 |
    | Charlie | EN301 |
*   **執行結果:**
    | NAME | CID |
    | :--: | :--: |
    | Alice | CS101 |
    | Alice | MA201 |
    | Bob | CS101 |
    | Charlie | EN301 |

#### `LEFT [OUTER] JOIN`
*   **English Explanation:** Returns all rows from the left table, and the matching rows from the right table. If there's no match in the right table, `NULL` values are returned for right table columns.
*   **廣東話解釋:** 返回左表嘅所有行，以及右表嘅匹配行。如果右表冇匹配項，則右表欄位會返回 `NULL` 值。
*   **Sample Usage:**
    ```sql
    SELECT S.NAME, SC.CID
    FROM STUDENT S
    LEFT JOIN STUDENT_COURSE SC ON S.SID = SC.SID;
    ```
*   **Execute Result:**
    | NAME | CID |
    | :------: | :---: |
    | Alice | CS101 |
    | Alice | MA201 |
    | Bob | CS101 |
    | Charlie | EN301 |
    | David | NULL |
    | Eve | NULL |
*   **執行結果:**
    | NAME | CID |
    | :------: | :---: |
    | Alice | CS101 |
    | Alice | MA201 |
    | Bob | CS101 |
    | Charlie | EN301 |
    | David | NULL |
    | Eve | NULL |

---

### 5. Subqueries
(子查詢)

A query nested inside another SQL query.
一個嵌套喺另一個 SQL 查詢內部嘅查詢。

*   **English Explanation:** Used to return data that will be used by the main query as a condition to further restrict the data to be retrieved.
*   **廣東話解釋:** 用於返回數據，呢啲數據將被主查詢用作條件，以進一步限制要檢索嘅數據。
*   **Sample Usage:**
    ```sql
    SELECT NAME, SCORE
    FROM STUDENT
    WHERE SCORE > (SELECT AVG(SCORE) FROM STUDENT);
    ```
*   **Execute Result:** (Assuming `AVG(SCORE)` is 82 from earlier calculation)
    | NAME | SCORE |
    | :------: | :---: |
    | Alice | 85 |
    | Charlie | 92 |
    | Eve | 85 |
*   **執行結果:** (假設 `AVG(SCORE)` 係 82，根據之前嘅計算)
    | NAME | SCORE |
    | :------: | :---: |
    | Alice | 85 |
    | Charlie | 92 |
    | Eve | 85 |

---

### 6. Combining Results (Set Operators)
(合併結果 (集合運算符))

These operators combine the result sets of two or more `SELECT` statements.
呢啲運算符將兩個或多個 `SELECT` 語句嘅結果集合併起嚟。

**Important Note:** For `UNION`, `UNION ALL`, `INTERSECT`, `EXCEPT`, the `SELECT` statements must have the same number of columns, and those columns must have similar data types, and be in the same order.
**重要提示:** 對於 `UNION`、`UNION ALL`、`INTERSECT`、`EXCEPT`，`SELECT` 語句必須具有相同數量嘅欄位，並且呢啲欄位必須具有相似嘅數據類型，並按相同順序排列。

#### `UNION`
*   **English Explanation:** Combines the result sets of two or more `SELECT` statements and removes duplicate rows.
*   **廣東話解釋:** 合併兩個或多個 `SELECT` 語句嘅結果集，並刪除重複嘅行。
*   **Example Data:**
    **Table A**           **Table B**
    | ID | Value |       | ID | Value |
    | :-: | :---: |       | :-: | :---: |
    | 1 | Apple |       | 1 | Apple |
    | 2 | Banana |       | 3 | Cherry |
*   **Sample Usage:**
    ```sql
    SELECT ID, Value FROM TableA
    UNION
    SELECT ID, Value FROM TableB;
    ```
*   **Execute Result:**
    | ID | Value |
    | :-: | :----: |
    | 1 | Apple |
    | 2 | Banana |
    | 3 | Cherry |
*   **執行結果:**
    | ID | Value |
    | :-: | :----: |
    | 1 | Apple |
    | 2 | Banana |
    | 3 | Cherry |

#### `UNION ALL`
*   **English Explanation:** Combines the result sets of two or more `SELECT` statements, including all duplicate rows.
*   **廣東話解釋:** 合併兩個或多個 `SELECT` 語句嘅結果集，包括所有重複嘅行。
*   **Sample Usage:**
    ```sql
    SELECT ID, Value FROM TableA
    UNION ALL
    SELECT ID, Value FROM TableB;
    ```
*   **Execute Result:**
    | ID | Value |
    | :-: | :----: |
    | 1 | Apple |
    | 2 | Banana |
    | 1 | Apple |
    | 3 | Cherry |
*   **執行結果:**
    | ID | Value |
    | :-: | :----: |
    | 1 | Apple |
    | 2 | Banana |
    | 1 | Apple |
    | 3 | Cherry |

---

### 7. Views
(視圖)

A virtual table based on the result-set of an SQL statement.
一個基於 SQL 語句結果集嘅虛擬表。

#### `CREATE VIEW <view_name> AS <SELECT_statement>`
*   **English Explanation:** Creates a view, which can be queried just like a real table. Views do not store data themselves, but present data from underlying tables.
*   **廣東話解釋:** 創建一個視圖，佢可以像真實表一樣被查詢。視圖本身唔存儲數據，但係展示底層表嘅數據。
*   **Sample Usage:**
    ```sql
    CREATE VIEW HighAchievers AS
    SELECT SID, NAME, SCORE
    FROM STUDENT
    WHERE SCORE >= 85;
    ```
*   **Execute Result:** A view named `HighAchievers` is created. You can now query it:
    ```sql
    SELECT * FROM HighAchievers;
    ```
    | SID | NAME | SCORE |
    | :--: | :--: | :---: |
    | S001 | Alice | 85 |
    | S003 | Charlie | 92 |
    | S005 | Eve | 85 |
*   **執行結果:** 創建咗一個叫做 `HighAchievers` 嘅視圖。而家你可以查詢佢：
    ```sql
    SELECT * FROM HighAchievers;
    ```
    | SID | NAME | SCORE |
    | :--: | :--: | :---: |
    | S001 | Alice | 85 |
    | S003 | Charlie | 92 |
    | S005 | Eve | 85 |
