Hello! It's great that you're so passionate about ICT and are taking the time to prepare thoroughly for your exam. I can see you've got a mock paper, and the best way to revise is to understand the core concepts behind the questions.

Let's break down all the topics in this paper into a comprehensive set of revision notes. I will teach you everything from the ground up, using examples that are different from the ones in your exam paper so you can learn the principles and apply them yourself. I'll add key terms in Cantonese (繁體中文) to help you connect the concepts.

Let's begin our ICT revision session!

***

### **Part 1: Database Management (數據庫管理)**

Databases are at the heart of almost every modern application, from social media to online shopping. This section covers how we design and interact with them.

---

#### **Topic 1.1: Entity-Relationship Diagrams (ERD) (實體關係圖)**

Before we build a database, we need to plan its structure. An ERD is the blueprint.

*   **What is it?** A visual way to show the structure of a database.
*   **Core Components:**
    *   **Entity (實體):** A real-world object or concept about which we store data. Think of it as a noun. For example, `Student`, `Course`, `Book`. In an ERD, we draw this as a rectangle.
    *   **Attribute (屬性):** A property or characteristic of an entity. For example, a `Student` entity might have attributes like `StudentID`, `Name`, and `DateOfBirth`. We usually don't need to draw these in a simple ERD unless specified.
    *   **Relationship (關係):** How two or more entities are connected. Think of it as a verb. For example, a `Student` *enrolls in* a `Course`. In an ERD, we draw this as a diamond.

*   **Cardinality (基數):** This is the most important part of a relationship. It defines the *number* of instances of one entity that can be related to instances of another entity.

    *   **One-to-One (1:1):** Each record in Table A relates to one, and only one, record in Table B.
        *   *Example:* `Country` and `CapitalCity`. A country has only one capital city, and a capital city belongs to only one country.
        *   `Country` ---< `has` >--- `CapitalCity` (Each side has a '1')

    *   **One-to-Many (1:M):** One record in Table A can relate to many records in Table B, but each record in Table B relates to only one record in Table A.
        *   *Example:* `Mother` and `Child`. One mother can have many children, but each child has only one biological mother.
        *   `Mother` ---< `has` >--- `Child` (The 'Many' side is marked with an 'M' or 'N')

    *   **Many-to-Many (M:N):** Many records in Table A can relate to many records in Table B.
        *   *Example:* `Student` and `Course`. One student can take many courses, and one course can be taken by many students.
        *   `Student` ---< `enrolls in` >--- `Course` (Both sides are marked with 'M' and 'N')

**ERD Sample:** Let's model a simple library system.

*   **Entities:** `Book`, `Member`
*   **Relationship:** A member can borrow books. So, the relationship is "borrows".
*   **Cardinality:** One member can borrow *many* books. One book can be borrowed by *one* member at a time. This is a **One-to-Many (1:M)** relationship from `Member` to `Book`.

Here's how you'd draw it:

```
+-----------+        +-----------+        +----------+
|  MEMBER   |----1---|  borrows  |----M---|   BOOK   |
+-----------+        +-----------+        +----------+
```

---

#### **Topic 1.2: SQL - Data Definition Language (DDL) (數據定義語言)**

DDL commands are used to build and define the database structure. The most common one is `CREATE TABLE`.

*   **`CREATE TABLE` Statement:** This command creates a new table.
    *   **Syntax:**
        ```sql
        CREATE TABLE TableName (
            Column1 DataType Constraint,
            Column2 DataType Constraint,
            ...
        );
        ```

*   **Data Types (數據類型):** Specify what kind of data a column can hold.
    *   `CHAR(n)`: Fixed-length string of `n` characters. Good for things like ID codes (e.g., 'HKDBS01').
    *   `VARCHAR(n)`: Variable-length string up to `n` characters. Good for names, addresses.
    *   `INTEGER` or `INT`: Whole numbers.
    *   `DATE`: Stores a date (Year, Month, Day).

*   **Constraints (約束):** Rules to ensure data integrity.
    *   **`PRIMARY KEY` (主鍵):** This is a crucial constraint.
        *   It uniquely identifies each record in a table.
        *   It **cannot** contain NULL values.
        *   There can be only **one** primary key per table.
        *   Think of it like an HKID number for a person or a student ID for a student. It must be unique.
        *   **Example:** To make `ProductID` the primary key, you would write:
            ```sql
            CREATE TABLE Products (
                ProductID CHAR(5) PRIMARY KEY,
                ProductName VARCHAR(50),
                Price INTEGER
            );
            ```

---

#### **Topic 1.3: SQL - Data Manipulation Language (DML) (數據操縱語言)**

DML commands are for managing the data *within* the tables.

*   **`INSERT INTO`:** Adds a new row of data to a table.
    *   **Syntax:**
        ```sql
        INSERT INTO TableName (Column1, Column2, ...)
        VALUES (Value1, Value2, ...);
        ```
    *   **Example:** Let's add a product to our `Products` table.
        ```sql
        INSERT INTO Products (ProductID, ProductName, Price)
        VALUES ('P0001', 'USB Drive', 150);
        ```
        *Note: String values like 'P0001' and 'USB Drive' use single quotes.*

*   **`UPDATE`:** Modifies existing records.
    *   **Syntax:**
        ```sql
        UPDATE TableName
        SET Column1 = Value1, Column2 = Value2
        WHERE condition;
        ```
    *   **Warning:** The `WHERE` clause is critical! If you forget it, you will update **every single row** in the table.
    *   **Example:** Change the price of the USB Drive.
        ```sql
        UPDATE Products
        SET Price = 120
        WHERE ProductID = 'P0001';
        ```

---

#### **Topic 1.4: Database Transactions (數據庫事務)**

Imagine transferring money between bank accounts. You debit one account and credit another. What if the system crashes after the debit but before the credit? Transactions prevent this.

*   **Transaction:** A sequence of database operations that are treated as a *single logical unit of work*.
*   **ACID Properties:** All transactions should be:
    *   **A**tomic (原子性): All or nothing. The entire transaction succeeds, or none of it does.
    *   **C**onsistent (一致性): The transaction brings the database from one valid state to another.
    *   **I**solated (隔離性): Concurrent transactions do not interfere with each other.
    *   **D**urable (持久性): Once a transaction is successful, its changes are permanent.

*   **Key Commands:**
    *   **`COMMIT`:** Saves all the changes made in the transaction.
    *   **`ROLLBACK`:** Undoes all the changes made in the transaction, returning the database to its state before the transaction began. This is used when an error occurs, like in the bank transfer example.

---

#### **Topic 1.5: Advanced Database Design**

*   **Derived Attribute (衍生屬性):** An attribute whose value can be calculated or derived from another attribute.
    *   *Example:* `Age` can be derived from `DateOfBirth` and the current date.
    *   **Why not store it?**
        1.  **Saves Storage Space:** Why store something you can always calculate?
        2.  **Avoids Data Inconsistency (數據不一致):** If you store both `DateOfBirth` and `Age`, what happens when a year passes? You have to remember to update the `Age` for every single person. If you forget, the data becomes inconsistent. Calculating it on the fly ensures it's always correct.
        3.  **Reduces Update Overheads:** Every time the base value changes, the derived value needs updating, which is extra work for the database.

*   **Database Anomalies (數據庫異常):** Problems that occur in poorly designed (non-normalized) tables.
    *   **Update Anomaly:** To change one piece of information, you have to change it in multiple rows. *Example:* If a customer's address is stored with every order they make, changing their address means finding and updating every single order record.
    *   **Insertion Anomaly:** You cannot add a new record because some data is missing. *Example:* You can't add a new course to a table if no students have enrolled in it yet (if student information is required in the same table).
    *   **Deletion Anomaly:** Deleting one piece of information unintentionally deletes another. *Example:* If you delete the last student enrolled in a course, the entire record for that course might disappear from the database.

*   **Normalization (規範化):** The process of organizing tables to reduce data redundancy and eliminate anomalies. The goal is to have tables where each one describes a single entity.
    *   **How it works:** We break down large, problematic tables into smaller, well-structured ones and link them using **Foreign Keys (外鍵)**. A foreign key is a primary key from one table that is placed into another table to create a link.

---

#### **Topic 1.6: Advanced SQL Queries (DQL - Data Query Language)**

This is about retrieving information from the database.

*   **Basic `SELECT`:**
    `SELECT column_names FROM table_name WHERE condition;`

*   **Aggregate Functions (聚合函數):** Perform a calculation on a set of values and return a single value.
    *   `COUNT()`: Counts the number of rows. `COUNT(*)` counts all rows.
    *   `SUM()`: Calculates the sum of a numeric column.
    *   `AVG()`: Calculates the average of a numeric column.
    *   `MAX()` / `MIN()`: Gets the maximum/minimum value.

*   **`GROUP BY`:** Groups rows that have the same values in specified columns into summary rows. It's almost always used with aggregate functions.
    *   *Example:* To find the number of employees in each department.
        ```sql
        SELECT Department, COUNT(*)
        FROM Employees
        GROUP BY Department;
        ```

*   **`HAVING`:** Filters the results *after* `GROUP BY` has been applied. `WHERE` filters rows *before* grouping.
    *   *Example:* To find departments with more than 10 employees.
        ```sql
        SELECT Department, COUNT(*)
        FROM Employees
        GROUP BY Department
        HAVING COUNT(*) > 10;
        ```

*   **Subqueries (子查詢):** A query nested inside another query.
    *   *Example:* Find employees who have the highest salary.
        ```sql
        SELECT Name FROM Employees
        WHERE Salary = (SELECT MAX(Salary) FROM Employees);
        ```
    *   The inner query `(SELECT MAX(Salary) ...)` runs first and finds the maximum salary. The outer query then uses that value to find the employee(s).
    *   The `ALL` keyword is used to compare a value to *every* value in a list returned by a subquery. `> ALL (...)` means "greater than every value in the list".

*   **Database View (視圖):**
    *   A stored `SELECT` query that is treated like a virtual table.
    *   **Advantages:**
        1.  **Simplicity:** Hides complex SQL logic from users. A user can query a simple view instead of writing a complicated multi-table join.
        2.  **Security:** You can restrict access by showing users a view that only contains specific columns or rows, hiding sensitive data.
        3.  **Data Independence:** The view can remain the same even if you change the structure of the underlying tables.

    *   **Creating a View:**
        ```sql
        CREATE VIEW V_HighEarners AS
        SELECT Name, Department
        FROM Employees
        WHERE Salary > 50000;
        ```
    *   Now, you can just query the view: `SELECT * FROM V_HighEarners;`

***

### **Part 2: Algorithm and Programming (算法和編程)**

This section is about giving the computer a set of instructions to solve a problem.

---

#### **Topic 2.1: Fundamental Algorithms & Data Verification**

*   **Algorithm Tracing (算法追蹤):** Manually stepping through an algorithm with specific inputs to see what it does and what its output will be. This is a key skill for debugging.

*   **Key Mathematical Operators:**
    *   **Integer Division (`div` or `//` in Python):** Division that discards the remainder.
        *   `17 div 5 = 3`
        *   `9 div 10 = 0`
    *   **Modulo (`mod` or `%` in Python):** Gives the remainder of a division.
        *   `17 mod 5 = 2`
        *   `123 mod 10 = 3` (This is a great trick to get the last digit of a number!)

    *   **How to get the n-th digit of a number?** You can use a combination of these. To get the 2nd digit of `8752` (the '7'):
        1.  Integer divide by 10 until the digit is at the end: `8752 div 10 = 875`.
        2.  Then use modulo 10 to extract it: `875 mod 10 = 7`. (This is a bit different from the exam's logic, but it's a common way to think about it).

*   **Data Verification (數據驗證):** Checks if the data entered is reasonable and correct. It's different from validation (which just checks the format).
    *   **Check Digit (校驗碼):** An extra digit added to a number, calculated from the other digits. The computer can recalculate the check digit and see if it matches the one provided. This helps detect errors from typing, like swapping two digits. Your credit card number and HKID number both use check digits!

---

#### **Topic 2.2: Sensors in Smart Systems (傳感器)**

Sensors are devices that detect changes in the environment and send that information to a computer.

*   **Motion Sensor:** Detects movement.
    *   **Passive Infrared (PIR) Sensor (被動紅外傳感器):** The most common type for turning on lights. It detects the body heat (infrared energy) of a person moving in front of it. It's passive because it doesn't send out any energy itself.
    *   **Ultrasonic Sensor (超聲波傳感器):** Sends out high-frequency sound waves and measures how long they take to bounce back. If an object moves into the area, the bounce-back time changes.

*   **Light Sensor (光傳感器) / Photoresistor:** Detects the level of ambient (surrounding) light.
    *   **How it helps adjust brightness:** A smart lighting system uses a light sensor to measure how bright the room is.
        1.  If it's a sunny day, the sensor detects a lot of light, and the system can automatically dim the indoor lights to save energy.
        2.  If it gets cloudy or turns to evening, the sensor detects low light, and the system can increase the brightness of the indoor lights to maintain a constant, comfortable level.

---

#### **Topic 2.3: Programming Fundamentals (using Python)**

*   **Function (函數):** A reusable block of code that performs a specific task.
    *   **`def` keyword:** Used to define a function.
    *   **Parameters (參數):** Input values that the function receives.
    *   **`return` keyword:** Sends a value back from the function.

    ```python
    # This function takes two numbers and returns the larger one
    def find_max(p, q):
      if p > q:
        return p
      else:
        return q
    ```

*   **`for` loop:** Executes a block of code a specific number of times.
    *   `range(1, k+1)` in Python will loop through numbers starting from 1 up to (and including) `k`.

*   **Purpose of the `func(a,b)` in the exam:** The function is iterating from 1 up to the smaller of the two numbers (`k`). It checks if the current number `i` can perfectly divide both `a` and `b` (`a%i == 0 and b%i == 0`). It keeps overwriting `y` with the latest number that can. The last number that can divide both will be the largest one. This algorithm finds the **Greatest Common Divisor (GCD) (最大公因數)**.

---

#### **Topic 2.4: Data Structures (數據結構)**

Data structures are specialized formats for organizing and storing data.

*   **2D Array (二維陣列):** A grid-like structure, or a list of lists. It's perfect for representing boards, maps, or tables.
    *   **Indexing:** You access elements using two indices: `ArrayName[row][column]`. Remember that in programming, indices usually start from 0, but in this exam paper, they seem to start from 1. Be careful to follow the convention given in the question.
    *   **Swapping elements:** To swap two items, you need a temporary variable.
        ```python
        # Let's swap BD[x][y] and BD[x-1][y]
        temp = BD[x][y]
        BD[x][y] = BD[x-1][y]
        BD[x-1][y] = temp
        ```

*   **Stack (堆疊):** A data structure that follows the **LIFO** principle: **Last-In, First-Out**.
    *   Think of a stack of plates. You put a new plate on the **top**, and when you take one off, you also take it from the **top**. The last plate you put on is the first one you take off.
    *   **Core Operations:**
        *   **`push(item)`:** Adds an item to the top of the stack.
        *   **`pop()`:** Removes and returns the item from the top of the stack.
    *   **Use Case - Reversing/Undo:** A stack is perfect for "undoing" actions. When you perform an action (like a swap), you can `push` the action onto a `History` stack. To undo, you `pop` the last action from the stack and perform the *opposite* action.
        *   Opposite of "move up" is "move down".
        *   Opposite of "move right" is "move left".

---

#### **Topic 2.5: Algorithm Design with Data Structures**

The final question combines algorithms and stacks.

*   **Resetting a board using a History Stack:**
    1.  Start a loop that continues as long as the `History` stack is not empty.
    2.  Inside the loop, `pop` the last move from the stack.
    3.  Execute the *inverse* of that move. For example, if you pop a `0` (up), you need to perform a `2` (down) swap.
    4.  Repeat until the stack is empty. The board will be back to its original state.

*   **Simplifying a sequence of moves:** Sometimes, a sequence of moves is inefficient. For example, `right` then `left` gets you nowhere.
    *   An algorithm to simplify this could use a temporary stack (`TmpStack`).
    *   Process the `History` stack one move at a time.
    *   If `TmpStack` is empty, just push the move onto it.
    *   If `TmpStack` is not empty, compare the current move with the one at the top of `TmpStack`.
    *   If they are opposites (e.g., `right` and `left`), then they cancel each other out. Don't push the new move; instead, `pop` the move from `TmpStack`.
    *   If they are not opposites, push the new move onto `TmpStack`.
    *   At the end, `TmpStack` will contain the simplified, necessary sequence of moves.

***

Good luck with your revision! Go through these topics, try to create your own simple examples, and you'll be well-prepared to tackle any question the exam throws at you. If any concept is still unclear, feel free to ask! (如果任何概念仍然不清楚，隨時問我啦！)