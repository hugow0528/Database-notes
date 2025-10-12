
# SQL Practice Exercise — Data Definition Language (DDL) using MySQL

Here are the complete solutions and explanations for the SQL practice exercise, tailored for a MySQL environment.

---

## Task A – Create Tables

### Question
Write SQL statements to create all three tables (`Member`, `Book`, `Borrow`) with appropriate primary keys and foreign keys.

### Solution
```sql
-- Create the Member table
CREATE TABLE Member (
    Member_ID VARCHAR(5) PRIMARY KEY,
    Name VARCHAR(30),
    JoinDate DATE
);

-- Create the Book table
CREATE TABLE Book (
    Book_ID VARCHAR(4) PRIMARY KEY,
    Title VARCHAR(50),
    Author VARCHAR(30)
);

-- Create the Borrow table
CREATE TABLE Borrow (
    Member_ID VARCHAR(5),
    Book_ID VARCHAR(4),
    BorrowDate DATE,
    CONSTRAINT fk_member FOREIGN KEY (Member_ID) REFERENCES Member(Member_ID),
    CONSTRAINT fk_book FOREIGN KEY (Book_ID) REFERENCES Book(Book_ID)
);
```

### Explanation
1.  **`CREATE TABLE Member`**: This statement creates the `Member` table. `Member_ID` is defined as the `PRIMARY KEY`, which ensures that every value in this column is unique and not null. This key uniquely identifies each member.
2.  **`CREATE TABLE Book`**: Similarly, this creates the `Book` table with `Book_ID` as its `PRIMARY KEY` to uniquely identify each book.
3.  **`CREATE TABLE Borrow`**: This statement creates the `Borrow` table, which records transactions.
    *   It includes `Member_ID` and `Book_ID` to link to the `Member` and `Book` tables, respectively.
    *   **`CONSTRAINT fk_member FOREIGN KEY (Member_ID) REFERENCES Member(Member_ID)`**: This clause defines a foreign key. It links the `Member_ID` column in the `Borrow` table to the `Member_ID` primary key in the `Member` table. This enforces referential integrity, meaning a `Member_ID` cannot exist in `Borrow` unless it already exists in `Member`.
    *   **`CONSTRAINT fk_book FOREIGN KEY (Book_ID) REFERENCES Book(Book_ID)`**: This does the same for `Book_ID`, linking it to the `Book` table.

---

## Task B – Insert Sample Data

### Question
Insert the provided sample records into each table.

### Solution
```sql
-- a) Insert data into Member table
INSERT INTO Member (Member_ID, Name, JoinDate) VALUES ('M001', 'Alice Lau', '2022-03-15');
INSERT INTO Member (Member_ID, Name, JoinDate) VALUES ('M002', 'Kelvin Ng', '2023-07-02');
INSERT INTO Member (Member_ID, Name, JoinDate) VALUES ('M003', 'May Lee', '2024-01-10');

-- b) Insert data into Book table
INSERT INTO Book (Book_ID, Title, Author) VALUES ('B001', 'Introduction to Python', 'J. Wong');
INSERT INTO Book (Book_ID, Title, Author) VALUES ('B002', 'Database Design Basics', 'C. Chan');
INSERT INTO Book (Book_ID, Title, Author) VALUES ('B003', 'Learn HTML and CSS', 'P. Ho');

-- c) Insert data into Borrow table
INSERT INTO Borrow (Member_ID, Book_ID, BorrowDate) VALUES ('M001', 'B001', '2024-09-05');
INSERT INTO Borrow (Member_ID, Book_ID, BorrowDate) VALUES ('M002', 'B002', '2024-09-07');
INSERT INTO Borrow (Member_ID, Book_ID, BorrowDate) VALUES ('M003', 'B003', '2024-09-08');
```

### Explanation
*   The `INSERT INTO TableName (column1, column2, ...)` statement is used to add new rows of data to a table.
*   The `VALUES (value1, value2, ...)` clause specifies the data to be inserted. The order of the values must correspond to the order of the columns listed.
*   MySQL is flexible with date formats and directly accepts the 'YYYY-MM-DD' string format for `DATE` columns, so a conversion function is not needed.

---

## Task C – Alter Table Practice

### Question
Perform the following modifications using `ALTER TABLE` statements:
1.  Add a new field `Phone` `VARCHAR(10)` to the `Member` table.
2.  Remove the foreign key constraint that links `Borrow` to `Book`.
3.  Add a new foreign key constraint again linking `Borrow.Book_ID` to `Book.Book_ID`.

### Solution
```sql
-- 1. Add a new field Phone VARCHAR(10) to the Member table
ALTER TABLE Member
ADD COLUMN Phone VARCHAR(10);

-- 2. Remove the foreign key constraint that links Borrow to Book
ALTER TABLE Borrow
DROP FOREIGN KEY fk_book;

-- 3. Add a new foreign key constraint again linking Borrow.Book_ID to Book.Book_ID
ALTER TABLE Borrow
ADD CONSTRAINT fk_book FOREIGN KEY (Book_ID) REFERENCES Book(Book_ID);
```

### Explanation
1.  **`ADD COLUMN`**: The `ALTER TABLE` statement combined with `ADD COLUMN` is used to add a new column to an existing table. Here, we add the `Phone` column with a `VARCHAR(10)` data type to the `Member` table.
2.  **`DROP FOREIGN KEY`**: This command removes a foreign key constraint from a table. You must specify the constraint's name (`fk_book` in this case, which we defined in Task A).
3.  **`ADD CONSTRAINT`**: This command is used to add new constraints. Here, we re-add the foreign key constraint we just dropped, re-establishing the link between the `Borrow` and `Book` tables.

---

## Task D – Challenge Yourself 💡

### Question
1.  Create a table named `Librarian` with the fields: `Librarian_ID` (VARCHAR(5), Primary Key), `Name` (VARCHAR(30)), and `HireDate` (DATE).
2.  Insert at least two librarian records.
3.  Add a foreign key in `Borrow` referencing `Librarian(Librarian_ID)`.

### Solution
```sql
-- 1. Create a table named Librarian
CREATE TABLE Librarian (
    Librarian_ID VARCHAR(5) PRIMARY KEY,
    Name VARCHAR(30),
    HireDate DATE
);

-- 2. Insert at least two librarian records
INSERT INTO Librarian (Librarian_ID, Name, HireDate) VALUES ('L001', 'John Smith', '2020-08-15');
INSERT INTO Librarian (Librarian_ID, Name, HireDate) VALUES ('L002', 'Jane Doe', '2021-02-20');

-- 3. Add a foreign key in Borrow referencing Librarian(Librarian_ID)
-- Step 3a: Add the Librarian_ID column to the Borrow table
ALTER TABLE Borrow
ADD COLUMN Librarian_ID VARCHAR(5);

-- Step 3b: Add the foreign key constraint
ALTER TABLE Borrow
ADD CONSTRAINT fk_librarian FOREIGN KEY (Librarian_ID) REFERENCES Librarian(Librarian_ID);
```

### Explanation
1.  **Create `Librarian` Table**: We first create the new `Librarian` table and define `Librarian_ID` as its primary key, following the same pattern as in Task A.
2.  **Insert Data**: We populate the new `Librarian` table with two sample records.
3.  **Modify `Borrow` Table**: To link a borrowing event to a librarian, we must first add a column to the `Borrow` table to store the librarian's ID.
    *   **Step 3a**: We use `ALTER TABLE ... ADD COLUMN` to add a `Librarian_ID` column to the `Borrow` table.
    *   **Step 3b**: With the column in place, we use `ALTER TABLE ... ADD CONSTRAINT` to create a new foreign key (`fk_librarian`) that links the new `Librarian_ID` column in the `Borrow` table to the `Librarian_ID` primary key in the `Librarian` table.
```