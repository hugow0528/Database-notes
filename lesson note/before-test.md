
- Normalisation is one of the most possibly tested parts. 
- ER-Diagrams
- ⁠⁠Subqueries
- ⁠EXCEPT / MINUS
- ⁠UPDATE / INSERT / DELETE
- ⁠JOIN (you are so far quite good on this part)
Based on your working on SQL exercises, take care of the below:

- Confusion between WHERE and HAVING (WHERE -> row-level before grouping, HAVING -> group-level after grouping)
- ⁠Confusion between LEFT JOIN / OUTER JOIN / RIGHT JOIN
- ⁠remember to study well on the operators (p.97-98 of your textbook)(attachment).
Remember to spend more time on studying“normalization” and “ER Digram”.

---

### 1. High Priority Topics (重點溫習)

#### **Normalization (正規化)**
*   **Goal:** Reduce data redundancy and improve data integrity.
    *   **目標：** 減少資料重複，確保資料完整性。
*   **1NF (First Normal Form):** Atomic values. No repeating groups.
    *   **第一正規化：** 每一格只可以有一個數值 (Atomic)。唔可以有一格有多個電話號碼。
*   **2NF (Second Normal Form):** Must be in 1NF + No **Partial Dependency**.
    *   **第二正規化：** 必須符合 1NF + 消除**部分依賴**。
    *   *Check:* Does a non-key column depend on *only part* of a Composite Primary Key? If yes, fix it. (所有非主鍵欄位必須完全依賴整個主鍵)。
*   **3NF (Third Normal Form):** Must be in 2NF + No **Transitive Dependency**.
    *   **第三正規化：** 必須符合 2NF + 消除**傳遞依賴**。
    *   *Check:* Does Column A depend on Column B, and Column B depends on the ID? (A -> B -> ID). If yes, move A and B to a new table. (非主鍵欄位唔可以依賴其他非主鍵欄位)。

#### **ER-Diagrams (實體關係圖)**
*   **Entity (Rectangles):** Objects (e.g., Student, Course).
    *   **實體 (長方形)：** 代表物件 (例如：學生、課程)。
*   **Attribute (Ovals):** Characteristics (e.g., Name, Age). Underline the Primary Key.
    *   **屬性 (橢圓形)：** 代表特徵 (例如：名、年齡)。主鍵 (PK) 要畫底線。
*   **Relationship (Diamonds):** How entities interact (e.g., Enrolls).
    *   **關係 (菱形)：** 實體之間嘅互動 (例如：報讀)。
*   **Cardinality (基數):**
    *   **1:1 (One-to-One)**
    *   **1:M (One-to-Many):** Most common. PK of the "1" side becomes FK in the "M" side. (最常見。 "1" 嗰邊嘅 PK 會變成 "M" 嗰邊嘅 FK)。
    *   **M:N (Many-to-Many):** **Crucial!** This usually requires creating a new intermediate table (Junction Table) in the physical database. (重點！多對多關係通常要開一張新嘅中間表)。

---

### 2. SQL Logic & Common Confusions (SQL 邏輯與常見混淆位)

#### **WHERE vs. HAVING**
*   **WHERE:** Filters data **row-by-row** *before* grouping.
    *   **WHERE：** 喺分組 (Grouping) **之前**篩選每一行資料。
    *   *Example:* `SELECT * FROM Students WHERE Age > 18` (Filter raw data).
*   **HAVING:** Filters **aggregated data** *after* grouping.
    *   **HAVING：** 喺分組 (Grouping) **之後**篩選統計結果。
    *   *Example:* `SELECT Team, COUNT(*) FROM Contestant GROUP BY Team HAVING COUNT(*) > 2` (Filter the result of the count).

#### **JOIN Types (連接類型)**
*   **INNER JOIN:** Returns records that have matching values in **both** tables.
    *   兩邊表格都有對應資料先會顯示。
*   **LEFT JOIN:** Returns **all** records from the left table, and the matched records from the right table. (If no match, result is NULL).
    *   顯示左邊表格所有資料，右邊有對應就顯示，冇就係 NULL。
*   **RIGHT JOIN:** Returns **all** records from the right table, and the matched records from the left table.
    *   顯示右邊表格所有資料，左邊有對應就顯示，冇就係 NULL。
*   **FULL OUTER JOIN:** Returns all records when there is a match in **either** left or right table.
    *   顯示兩邊所有資料，無論有冇對應。

---

### 3. Advanced SQL & Operators (Based on Textbook p.97-98)

#### **Set Operations**
*   **EXCEPT / MINUS:** Returns rows in the first query that are **NOT** in the second query.
    *   **意思：** 喺第一個結果入面，但係**唔**喺第二個結果入面嘅資料 (A - B)。
    *   *Note:* Some databases use `MINUS`, others use `EXCEPT`.
*   **Subqueries (子查詢):** A query nested inside another query (usually in the `WHERE` clause).
    *   **技巧：** Run the inner query in your head first to get the list of values. (試下喺腦入面行咗裡面個括號先)。
    *   *Example:* `SELECT Name FROM Students WHERE ID IN (SELECT StudentID FROM Marks WHERE Score > 90)`

#### **Key Operators (重點運算符)**
*   **LIKE / Wildcards:** Used for pattern matching.
    *   `%`: Represents zero or more characters. (代表任何長度嘅字元).
        *   `LIKE 'S%'` -> Starts with S.
    *   `_`: Represents exactly **one** character. (代表剛好一個字元).
        *   `LIKE '_a%'` -> Second letter is 'a'.
*   **IN:** Matches any value in a list.
    *   `WHERE ID IN (1, 2, 3)` is same as `ID = 1 OR ID = 2 OR ID = 3`.
*   **BETWEEN:** Values within a range (inclusive).
    *   `WHERE Age BETWEEN 10 AND 20`.
*   **IS NULL / IS NOT NULL:**
    *   **Important:** Never use `= NULL`. Always use `IS NULL`. (千祈唔好用 `= NULL`，一定要用 `IS NULL`).

#### **Functions (函數)**
*   **Aggregation (統計):** `COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()`.
    *   *Note:* Usually require `GROUP BY` if you select other columns too.
*   **String Functions:**
    *   `SUBSTRING` / `MID`: Extract part of a text.
    *   `UPPER` / `LOWER`: Convert case.
    *   `TRIM`: Remove spaces.

---

### 4. Data Manipulation (DML) - Don't forget syntax!

*   **INSERT:** Add new rows.
    *   `INSERT INTO table_name (column1, column2) VALUES (value1, value2);`
*   **UPDATE:** Modify existing data.
    *   `UPDATE table_name SET column1 = value1 WHERE condition;`
    *   **Warning:** If you forget `WHERE`, you update **all** rows! (如果唔記得加 WHERE，就會改晒全表資料！)
*   **DELETE:** Remove rows.
    *   `DELETE FROM table_name WHERE condition;`
    *   **Warning:** If you forget `WHERE`, you delete **everything**! (如果唔記得加 WHERE，就會刪除所有資料！)

---

### Quick Exam Tips (考試小貼士):
1.  **Read carefully (睇清楚題目):** Does it ask for "Starts with A" (`LIKE 'A%'`) or "Contains A" (`LIKE '%A%'`)?
2.  **Quotations (引號):** Strings and Dates usually need single quotes `'Text'`, Numbers do not.
3.  **Order (次序):**
    `SELECT` -> `FROM` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `ORDER BY`.

