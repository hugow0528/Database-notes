Here is the revised guide, tailored specifically for students learning SQL in Microsoft Access. Commands that are not supported have been removed, and syntax has been corrected to be accurate for Access.

***

### **Using SQL in Microsoft Access: A Student's Guide**

This guide will teach you how to use SQL (Structured Query Language) directly within Microsoft Access. While many tasks in Access can be done through the visual designer, knowing SQL gives you more power and flexibility.

To run these SQL commands, go to the **Create** tab, click on **Query Design**, and then immediately close the "Show Table" pop-up. Finally, click on the **SQL View** button in the top-left corner to open the SQL editor.

---

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

### 1. Table Management (DDL - Data Definition Language)
(數據定義語言)

In Access, you will often create and delete databases and tables using the user interface. However, you can also manage tables directly with SQL.

#### `CREATE TABLE <table_name> (...)`
*   **English Explanation:** Creates a new table with specified columns and data types.
*   **廣東話解釋:** 建立一個新嘅表，並定義好欄位同數據類型。
*   **Access Note:** Access uses different data type names. `VARCHAR` and `CHAR` become `TEXT`. `INT` becomes `INTEGER`. `CHECK` constraints are not added in SQL; they are set as a "Validation Rule" in the table's Design View.
*   **廣東話注意:** Access 用嘅數據類型名唔同。`VARCHAR` 同 `CHAR` 變成 `TEXT`。`INT` 變成 `INTEGER`。`CHECK` 約束唔係用 SQL 加，而係喺表格嘅「設計檢視」入面設定為「驗證規則」。
*   **Sample Usage:**
    ```sql
    CREATE TABLE STUDENT (
        SID TEXT(5) PRIMARY KEY,
        NAME TEXT(40) NOT NULL,
        CLASS TEXT(2),
        CNO INTEGER,
        SCORE INTEGER
    );
    ```
*   **Execute Result:** The `STUDENT` table is created.
*   **執行結果:** `STUDENT` 表格被建立。

#### `ALTER TABLE <table_name> ADD COLUMN <field_name> <data_type>`
*   **English Explanation:** Adds a new column to an existing table. Note the keyword `COLUMN` is required in Access.
*   **廣東話解釋:** 為現有嘅表增加一個新嘅欄位。注意喺 Access 入面需要 `COLUMN` 呢個關鍵字。
*   **Sample Usage:**
    ```sql
    ALTER TABLE STUDENT ADD COLUMN EMAIL TEXT(100);
    ```
*   **Execute Result:** The `STUDENT` table now has an `EMAIL` column.
*   **執行結果:** `STUDENT` 表格而家多咗一個 `EMAIL` 欄位。

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
*   **English Explanation:** Deletes an entire table permanently.
*   **廣東話解釋:** 永久刪除成個表。
*   **Sample Usage:**
    ```sql
    DROP TABLE COURSE;
    ```
*   **Execute Result:** The `COURSE` table is permanently deleted.
*   **執行結果:** `COURSE` 表格被永久刪除。

---

### 2. Data Manipulation (DML - Data Manipulation Language)
(數據操縱語言)

These statements are used for adding, retrieving, changing, and deleting data. They work perfectly in the Access SQL editor.

#### `INSERT INTO <table_name> (...) VALUES (...)`
*   **English Explanation:** Adds new rows (records) of data into a table.
*   **廣東話解釋:** 將新嘅行（記錄）數據插入到一個表入面。
*   **Sample Usage:**
    ```sql
    INSERT INTO STUDENT (SID, NAME, CLASS, CNO, SCORE)
    VALUES ('S001', 'Alice', '1A', 1, 85);
    ```

#### `SELECT ... FROM <table_name>`
*   **English Explanation:** Retrieves data from one or more tables. This is the most frequently used SQL command.
*   **廣東話解釋:** 從一個或多個表入面檢索數據。呢個係最常用嘅 SQL 指令。

    *   **`SELECT *`**: Retrieves all columns.
        *   **廣東話解釋:** 檢索所有欄位。
        *   **Sample Usage:** `SELECT * FROM STUDENT;`

    *   **`SELECT <column1>, <column2>`**: Retrieves specific columns.
        *   **廣東話解釋:** 檢索特定欄位。
        *   **Sample Usage:** `SELECT NAME, SCORE FROM STUDENT;`

    *   **`WHERE <condition>`**: Filters rows based on criteria.
        *   **廣東話解釋:** 根據條件篩選行。
        *   **Sample Usage:** `SELECT NAME, SCORE FROM STUDENT WHERE CLASS = '1A';`

    *   **`ORDER BY <column> [ASC|DESC]`**: Sorts the results. `ASC` (ascending) is default, `DESC` is descending.
        *   **廣東話解釋:** 排序結果。`ASC`（升序）係預設，`DESC` 係降序。
        *   **Sample Usage:** `SELECT NAME, SCORE FROM STUDENT ORDER BY SCORE DESC;`

    *   **`DISTINCT <column>`**: Returns only unique values in a column.
        *   **廣東話解釋:** 只返回欄位中嘅唯一值。
        *   **Sample Usage:** `SELECT DISTINCT CLASS FROM STUDENT;`

    *   **`GROUP BY <column>`**: Groups rows with the same values into summary rows. Often used with aggregate functions.
        *   **廣東話解釋:** 將具有相同值嘅行分組為匯總行。通常與聚合函數一起使用。
        *   **Sample Usage:**
            ```sql
            SELECT CLASS, AVG(SCORE) AS AverageScore
            FROM STUDENT
            GROUP BY CLASS;
            ```

    *   **`HAVING <condition>`**: Filters groups based on conditions after `GROUP BY`.
        *   **廣東話解釋:** 喺 `GROUP BY` 之後，根據條件篩選分組。
        *   **Sample Usage:**
            ```sql
            SELECT CLASS, AVG(SCORE) AS AverageScore
            FROM STUDENT
            GROUP BY CLASS
            HAVING AVG(SCORE) > 80;
            ```

#### `UPDATE <table_name> SET <column> = <value> WHERE <condition>`
*   **English Explanation:** Modifies existing data in a table. If `WHERE` is omitted, all rows will be updated.
*   **廣東話解釋:** 修改表中現有嘅數據。如果省略 `WHERE` 子句，所有行都會被更新。
*   **Sample Usage:**
    ```sql
    UPDATE STUDENT SET SCORE = 90 WHERE SID = 'S001';
    ```

#### `DELETE FROM <table_name> WHERE <condition>`
*   **English Explanation:** Deletes rows from a table. If `WHERE` is omitted, all rows will be deleted.
*   **廣東話解釋:** 從表中刪除行。如果省略 `WHERE` 子句，所有行都會被刪除。
*   **Sample Usage:**
    ```sql
    DELETE FROM STUDENT WHERE SID = 'S004';
    ```

---

### 3. SQL Operators and Functions
(SQL 運算符同函數)

#### Comparison Operators (`=`, `<>`, `>`, `<`, `>=`, `<=`)
*   **English Explanation:** Used in `WHERE` clauses to compare values. Note: For "not equal," you can use `<>` in Access.
*   **廣東話解釋:** 喺 `WHERE` 子句入面用嚟比較數值。注意：喺 Access 入面，「不等於」可以用 `<>`。
*   **Sample Usage:** `SELECT NAME FROM STUDENT WHERE SCORE >= 85;`

#### Logical Operators (`AND`, `OR`, `NOT`)
*   **English Explanation:** Used to combine or negate conditions in `WHERE` clauses.
*   **廣東話解釋:** 喺 `WHERE` 子句入面用嚟組合或否定條件。
*   **Sample Usage:** `SELECT NAME FROM STUDENT WHERE CLASS = '1A' AND SCORE > 80;`

#### `IN` / `NOT IN`
*   **English Explanation:** Checks if a value matches any value in a list.
*   **廣東話解釋:** 檢查一個值係咪匹配列表中嘅任何值。
*   **Sample Usage:** `SELECT NAME FROM STUDENT WHERE CLASS IN ('1A', '1B');`

#### `BETWEEN ... AND ...`
*   **English Explanation:** Selects values within a specified range (inclusive).
*   **廣東話解釋:** 選取指定範圍內（包括兩端）嘅值。
*   **Sample Usage:** `SELECT NAME, SCORE FROM STUDENT WHERE SCORE BETWEEN 75 AND 85;`

#### `LIKE` (with wildcards `*`, `?`)
*   **English Explanation:** Searches for specific patterns in a column. Access uses different wildcards than standard SQL.
    *   `*`: Represents zero or more characters (like `%` in other SQLs).
    *   `?`: Represents a single character (like `_` in other SQLs).
*   **廣東話解釋:** 喺欄位中搜索特定模式。Access 用嘅萬用字元同標準 SQL 唔同。
    *   `*`: 代表零個或多個字符（等於其他 SQL 嘅 `%`）。
    *   `?`: 代表單個字符（等於其他 SQL 嘅 `_`）。
*   **Sample Usage:**
    ```sql
    SELECT NAME FROM STUDENT WHERE NAME LIKE 'A*'; -- Names starting with 'A'
    SELECT NAME FROM STUDENT WHERE NAME LIKE '?o?'; -- 3-letter names with 'o' as the second character
    ```

#### `IS NULL` / `IS NOT NULL`
*   **English Explanation:** Checks for empty (null) or non-empty values.
*   **廣東話解釋:** 檢查係咪為空值或者非空值。
*   **Sample Usage:** `SELECT SID, NAME FROM STUDENT WHERE CNO IS NULL;`

#### Aggregate Functions (`COUNT`, `MAX`, `MIN`, `SUM`, `AVG`)
*   **English Explanation:** Perform calculations on a set of rows and return a single value.
*   **廣東話解釋:** 對一組行執行計算並返回一個單一值。
*   **Sample Usage:**
    ```sql
    SELECT COUNT(*) AS TotalStudents, MAX(SCORE) AS HighestScore, AVG(SCORE) AS AverageScore
    FROM STUDENT;
    ```

---

### 4. Joining Tables
(連接表)

Joins are used to combine rows from two or more tables based on a related column.

#### `INNER JOIN`
*   **English Explanation:** Returns only the rows that have matching values in both tables.
*   **廣東話解釋:** 只返回兩個表中都有匹配值嘅行。
*   **Sample Usage:** (Create a `STUDENT_COURSE` table first for this example)
    ```sql
    SELECT S.NAME, SC.CID
    FROM STUDENT AS S
    INNER JOIN STUDENT_COURSE AS SC ON S.SID = SC.SID;
    ```

#### `LEFT JOIN`
*   **English Explanation:** Returns all rows from the left table, and only the matching rows from the right table. If there's no match, `NULL` is returned for the right table columns.
*   **廣東話解釋:** 返回左表嘅所有行，以及右表嘅匹配行。如果右表冇匹配項，則右表欄位會返回 `NULL` 值。
*   **Sample Usage:**
    ```sql
    SELECT S.NAME, SC.CID
    FROM STUDENT AS S
    LEFT JOIN STUDENT_COURSE AS SC ON S.SID = SC.SID;
    ```

---

### 5. Subqueries
(子查詢)

A query nested inside another SQL query.

*   **English Explanation:** Used to return data that will be used by the main query as a condition.
*   **廣東話解釋:** 用於返回數據，呢啲數據將被主查詢用作條件。
*   **Sample Usage:**
    ```sql
    SELECT NAME, SCORE
    FROM STUDENT
    WHERE SCORE > (SELECT AVG(SCORE) FROM STUDENT);
    ```

---

### 6. Combining Results (`UNION` operator)
(合併結果 (`UNION` 運算符))

The `UNION` operator combines the result sets of two or more `SELECT` statements.

#### `UNION` & `UNION ALL`
*   **English Explanation:** `UNION` combines results and removes duplicate rows. `UNION ALL` combines results but keeps all duplicate rows. The `SELECT` statements must have the same number of columns in the same order.
*   **廣東話解釋:** `UNION` 合併結果並移除重複嘅行。`UNION ALL` 合併結果但會保留所有重複行。`SELECT` 語句必須有相同數量同順序嘅欄位。
*   **Sample Usage:**
    ```sql
    SELECT ID, Value FROM TableA
    UNION
    SELECT ID, Value FROM TableB;
    ```

---

### 7. Saved Queries (Access's version of Views)
(已儲存的查詢 (Access 嘅視圖版本))

In Access, a "View" is called a **Saved Query**. It is a virtual table based on a `SELECT` statement.

#### Creating a Saved Query
*   **English Explanation:** Access does not use the `CREATE VIEW` command. Instead, you create a `SELECT` query and save it with a name. You can then use this saved query in other queries just like a real table.
*   **廣東話解釋:** Access 唔用 `CREATE VIEW` 指令。 你要建立一個 `SELECT` 查詢，然後用一個名儲存佢。 之後你就可以喺其他查詢入面，好似用一張真嘅表咁樣用呢個已儲存嘅查詢。
*   **How-To:**
    1.  Go to the **Create** tab and click **Query Design**.
    2.  Switch to **SQL View**.
    3.  Enter your `SELECT` statement:
        ```sql
        SELECT SID, NAME, SCORE
        FROM STUDENT
        WHERE SCORE >= 85;
        ```
    4.  Click the **Save** icon and name your query, for example, `HighAchievers`.
*   **Execute Result:** A new query named `HighAchievers` appears in the Navigation Pane. You can now use it like a table.
*   **執行結果:** 一個叫做 `HighAchievers` 嘅新查詢會出現喺導覽窗格。你而家可以好似一張表咁用佢。
*   **Using the Saved Query:**
    ```sql
    SELECT NAME FROM HighAchievers ORDER BY NAME;
    ```
