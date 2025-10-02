

### **Study Notes for ICT Paper 2**

---

### **Section A: Database (數據庫)**

This section focuses on how we design, create, and manage databases using SQL.

#### **1. ER Diagrams and Cardinality (實體關係圖與基數)**

An **Entity-Relationship (ER) Diagram** is like a blueprint for a database. It shows the main components and how they are related.

*   **Entity (實體):** An object or concept you want to store information about. Represented by a **rectangle**.
    *   In your exam: `DOCTOR`, `NURSE`, `PATIENT`, `VREC`.
*   **Relationship (關係):** Shows how two entities are connected. Represented by a **diamond**.
    *   In your exam: A `PATIENT` *has* a `VREC` (record).
*   **Cardinality (基數):** Describes the numerical relationship between entities. It answers "how many?".
    *   **1-to-1:** One doctor has one office; one office has one doctor.
    *   **1-to-Many (1:M):** One doctor can have many patients, but each patient in this context has only one primary doctor.
    *   **Many-to-Many (M:N):** One student can take many courses; one course can have many students.

**Let's analyze the relationships in Question 1:**

1.  **Doctor  relación Nurse:**
    *   "each doctor has one to three nurses" (1 Doctor -> 1..3 Nurses)
    *   "Each nurse works with one doctor or without any doctor" (1 Nurse -> 0..1 Doctor)
    *   This is a **One-to-Many** relationship. Draw a line between `DOCTOR` and `NURSE` with a diamond in the middle. The cardinality is 1 on the doctor's side and N (or M) on the nurse's side.

2.  **Doctor / Patient relación VREC:**
    *   "Each patient and doctor have at least one record in VREC". This means both entities are connected to VREC.
    *   This implies a **One-to-Many** relationship for both. One doctor can have many VRECs, and one patient can have many VRECs.

> **(廣東話解釋)**
> ER圖係數據庫嘅設計圖。`Entity` (實體) 係你想儲存資料嘅嘢 (好似醫生、病人)。`Relationship` (關係) 係佢哋之間嘅關聯。`Cardinality` (基數) 就係講數量上嘅關係，例如「一個醫生可以對應幾多個護士」，呢個就係 "1-to-Many" (一對多) 關係。

---

#### **2. SQL Basics: `CREATE`, `INSERT`, `ROLLBACK`**

**SQL (Structured Query Language)** is the language we use to communicate with databases.

*   **`CREATE TABLE` (建立資料表):** This command builds a new table.
    *   To make sure every record is unique, we use a **`PRIMARY KEY`** (主鍵). A primary key cannot be empty and its value must be unique for each record.
    *   **For Question 2(a):** To prevent two records from having the same `TRANID`, you must declare it as the primary key.
        ```sql
        CREATE TABLE TRAN (
            TRANID CHAR(7) PRIMARY KEY, -- This ensures TRANID is always unique.
            TYPE INTEGER,
            SIZE INTEGER
        );
        ```

*   **`INSERT INTO` (插入資料):** This adds a new row of data into a table.
    *   Remember to put text values inside single quotes (`' '`).
    *   **For Question 2(b)(i):**
        ```sql
        INSERT INTO TRAN (TRANID, TYPE, SIZE) VALUES ('0208822', 3, 43);
        ```

*   **`ROLLBACK` (復原交易):** Imagine an online purchase. Several steps happen: you pay, the stock is reduced, an invoice is created. This is a "transaction". If one step fails (e.g., payment declined), the whole transaction must be cancelled to keep the data consistent.
    *   `ROLLBACK` is the command to undo all changes made during a transaction, as if it never happened.
    *   **For Question 2(b)(ii):** The shoes were out of stock, so the transaction was invalid. The correct action is to **`ROLLBACK`** to cancel the `INSERT` command and maintain data consistency (保持數據一致性).

---

#### **3. Database Concepts: Derived Attributes & Anomalies**

*   **Derived Attribute (衍生屬性):** An attribute whose value can be calculated from other attributes.
    *   **For Question 3(a):** `AGE` is a derived attribute because you can always calculate it from the `DOB` (Date of Birth) and the current date.
    *   **Disadvantages of storing it (Question 3(b)):**
        1.  **Data Inconsistency (資料不一致):** The stored age will become wrong the next day or after the person's birthday. You would need to constantly update it.
        2.  **Wasted Storage Space (浪費儲存空間):** Since it can be calculated, storing it is redundant and takes up unnecessary space.

*   **Anomalies (異常):** These are problems in a poorly designed database.
    *   **Update Anomaly (更新異常):** When you update information, you have to change it in multiple places. If you miss one, the data becomes inconsistent.
    *   **For Question 4(a):** The command `UPDATE JOBAPP SET AGE = 18 WHERE AID = 1;` is executed. In the table, the user Jamie (`UID` 1) has two applications (`AID` 1 and `AID` 2). The update only changes the age for the first application. Now, Jamie is listed as being 18 in one record and 17 in another. This is a classic **update anomaly**.

*   **Normalization (正規化):** The process of organizing tables to reduce data redundancy and avoid anomalies. We do this by splitting a large table into smaller, well-structured tables.
    *   **For Question 4(b):** The `JOBAPP` table mixes user information with job application information. To fix this, you should split it into:
        1.  A `USER` table to store user details.
        2.  An `APPLICATION` table to store job and application details.

    *   **Corrected Structure:**
        *   `USER` (**`UID`**, `NAME`, `AGE`)
            *   Primary Key: `UID`
        *   `JOBAPP` (**`AID`**, `UID`, `JOB`, `INDUSTRY`, `MINAGE`, `HOUR`, `EMPLOYER`)
            *   Primary Key: `AID`
            *   **Foreign Key (外來鍵):** `UID` (This links the `JOBAPP` table back to the `USER` table).

> **(廣東話解釋)**
> `Derived Attribute` (衍生屬性) 係可以由其他資料計出嚟嘅嘢，例如「年齡」可以由「出生日期」計出。最好唔好直接儲存佢，因為佢會變，好易搞到資料唔一致。
> `Normalization` (正規化) 就係將一張好大好亂嘅表，拆成幾張細啲、清晰啲嘅表，解決 Update Anomaly (更新異常) 呢啲問題。

---

#### **4. Advanced SQL: `GROUP BY`, `HAVING`, Subqueries, and Views**

*   **`GROUP BY` (分組):** Groups rows that have the same values in specified columns into summary rows. It's often used with aggregate functions like `COUNT()`, `AVG()` (average), `SUM()` (sum).
*   **`HAVING` (分組篩選):** Used with `GROUP BY` to filter groups. `WHERE` filters rows *before* grouping; `HAVING` filters groups *after* grouping.
    *   **For Question 5(c):** To find departments with an average step count over 9000.
        ```sql
        SELECT p.DEPT
        FROM PAR p, READING r
        WHERE p.PID = r.PID
        GROUP BY p.DEPT        -- First, group all readings by department
        HAVING AVG(r.STEP) > 9000; -- Then, only keep the groups that meet the condition
        ```

*   **Subquery (子查詢):** A query nested inside another query. The inner query runs first.
    *   **For Question 5(d)(i):**
        `... WHERE STEP >= ALL (SELECT STEP FROM READING WHERE RDATE = '20/03/2024')`
        *   The inner query gets all the step counts for that day.
        *   `>= ALL` means "greater than or equal to every value" in the list, which is the same as being greater than or equal to the **maximum** value.
        *   **Purpose:** To find the participant(s) who achieved the maximum number of steps on 20/03/2024.

*   **View (視圖):** A virtual table based on the result of an SQL query.
    *   **Advantages (Question 5(d)(ii)):**
        1.  **Simplicity (簡化複雜性):** Hides complex SQL logic. You can write a difficult query once, save it as a view, and then query the view easily.
        2.  **Security (增強安全性):** You can give users access to a view that only shows certain columns or rows, protecting sensitive data in the original tables.

### **Section B: Algorithm and Programming (演算法與編程)**

This section tests your logical thinking and knowledge of programming concepts and data structures.

---

#### **5. Algorithms, Sensors, and Python**

*   **Algorithms (演算法):** A set of step-by-step instructions to solve a problem. In Question 6, the algorithm is a **check digit** calculation. It uses a formula on the first 7 digits to see if the result matches the 8th digit, verifying the ID is likely valid.

*   **Sensors (感應器) (Question 7):**
    *   **To detect someone entering a room:** Use a **Motion Sensor** or an **Infrared (IR) Sensor**. They detect movement or body heat.
    *   **To adjust room brightness:** Use a **Light Sensor**. It measures the ambient light. The system then compares this to a preset level and adjusts the artificial lights to make it brighter or dimmer, saving energy and keeping the room comfortable.

*   **Python Functions (Question 8):**
    *   The function `func(a, b)` finds the **Greatest Common Divisor (GCD)** (最大公因數) of two numbers. It works by checking every number `i` from 1 up to the smaller of the two inputs. If `i` divides both numbers perfectly (`a%i == 0 and b%i == 0`), it's a common divisor. The function keeps the last one it finds, which will be the largest.
    *   The `%` (modulo) operator gives you the remainder of a division. It's key to finding divisors.

---

#### **6. Data Structures: 2D Arrays and Stacks**

*   **2D Array (二維陣列) (Question 9):** A grid-like structure, perfect for representing a game board. In Python, this is usually a "list of lists". For example, `BD[1][2]` would access the element in the 1st row and 2nd column.

*   **Stack (堆疊) (Question 9):** A data structure that operates on a **Last-In, First-Out (LIFO)** principle. Think of a stack of books; you can only add a book to the top and take a book from the top.
    *   **`push`:** Add an item to the top.
    *   **`pop`:** Remove the top item.

*   **Using a Stack to Undo Moves (Question 9(c)):**
    *   The program `push`es every move (0 for up, 1 for right, etc.) onto a `History` stack.
    *   To reset the board, the `resetBD` program must `pop` the moves from the stack and perform the **opposite** move.
    *   Opposite moves are: `up (0) <-> down (2)` and `right (1) <-> left (3)`.
    *   Notice that the opposite of a move `d` is `(d + 2) % 4`. This is the logic used to find the reverse swap.

*   **Simplifying the History (Question 9(d)):**
    *   Sometimes you make moves that cancel each other out, like moving right and then immediately moving left.
    *   The `SimplifyH` subprogram is designed to find and remove these pairs of opposite, consecutive moves from the `History` stack to make the reset process more efficient. It pops two moves, and if they are opposites, it discards them. If not, it keeps them.

> **(廣東話解釋)**
> `Stack` (堆疊) 係一種「後入先出」嘅資料結構，好似一疊碟咁。你嘅程式用佢嚟記錄移動嘅歷史 (`History`)。要「復原」(reset) 嘅時候，就由 `History` stack 最頂攞返上一步出嚟，然後行相反嘅一步。例如，上一步係「向右」，復原就要「向左」。
