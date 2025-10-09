
# SQL Practice Exercise — Data Definition Language (DDL)

Here are the complete solutions for the SQL practice exercise using Oracle SQL.

## Database Structure

You are helping a library set up its database to manage books, members, and borrowing records. Use SQL DDL statements to create the following tables and insert some sample data.

### Table: Member

| Field | Data Type | Description |
| :--- | :--- | :--- |
| Member_ID | VARCHAR(5) | Member ID (Primary Key) |
| Name | VARCHAR(30) | Member's Name |
| JoinDate | DATE | The date when the member joined the library |

### Table: Book

| Field | Data Type | Description |
| :--- | :--- | :--- |
| Book_ID | VARCHAR(4) | Book ID (Primary Key) |
| Title | VARCHAR(50) | Book Title |
| Author | VARCHAR(30) | Author's Name |

### Table: Borrow

| Field | Data Type | Description |
| :--- | :--- | :--- |
| Member_ID | VARCHAR(5) | Member ID (Foreign Key → Member.Member_ID) |
| Book_ID | VARCHAR(4) | Book ID (Foreign Key → Book.Book_ID) |
| BorrowDate | DATE | Date the book was borrowed |

---

## Task A – Create Tables

**Question:** Write SQL statements to create all three tables with appropriate primary and foreign keys.

**Solution:**

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

---

## Task B – Insert Sample Data

**Question:** Insert the following sample records into each table.

**Solution:**

```sql
-- a) Insert data into Member table
INSERT INTO Member (Member_ID, Name, JoinDate) VALUES ('M001', 'Alice Lau', TO_DATE('2022-03-15', 'YYYY-MM-DD'));
INSERT INTO Member (Member_ID, Name, JoinDate) VALUES ('M002', 'Kelvin Ng', TO_DATE('2023-07-02', 'YYYY-MM-DD'));
INSERT INTO Member (Member_ID, Name, JoinDate) VALUES ('M003', 'May Lee', TO_DATE('2024-01-10', 'YYYY-MM-DD'));

-- b) Insert data into Book table
INSERT INTO Book (Book_ID, Title, Author) VALUES ('B001', 'Introduction to Python', 'J. Wong');
INSERT INTO Book (Book_ID, Title, Author) VALUES ('B002', 'Database Design Basics', 'C. Chan');
INSERT INTO Book (Book_ID, Title, Author) VALUES ('B003', 'Learn HTML and CSS', 'P. Ho');

-- c) Insert data into Borrow table
INSERT INTO Borrow (Member_ID, Book_ID, BorrowDate) VALUES ('M001', 'B001', TO_DATE('2024-09-05', 'YYYY-MM-DD'));
INSERT INTO Borrow (Member_ID, Book_ID, BorrowDate) VALUES ('M002', 'B002', TO_DATE('2024-09-07', 'YYYY-MM-DD'));
INSERT INTO Borrow (Member_ID, Book_ID, BorrowDate) VALUES ('M003', 'B003', TO_DATE('2024-09-08', 'YYYY-MM-DD'));
```

---

## Task C – Alter Table Practice

**Question:** Perform the following modifications using `ALTER TABLE` statements:
1.  Add a new field `Phone` VARCHAR(10) to the `Member` table.
2.  Remove the foreign key constraint that links `Borrow` to `Book`.
3.  Add a new foreign key constraint again linking `Borrow.Book_ID` to `Book.Book_ID`.

**Solution:**

```sql
-- 1. Add a new field Phone VARCHAR(10) to the Member table
ALTER TABLE Member
ADD (Phone VARCHAR(10));

-- 2. Remove the foreign key constraint that links Borrow to Book
-- Note: The constraint name 'fk_book' was defined in Task A.
ALTER TABLE Borrow
DROP CONSTRAINT fk_book;

-- 3. Add a new foreign key constraint again linking Borrow.Book_ID to Book.Book_ID
ALTER TABLE Borrow
ADD CONSTRAINT fk_book FOREIGN KEY (Book_ID) REFERENCES Book(Book_ID);
```

---

## Task D – Challenge Yourself 💡

**Question:**
1.  Create a table named `Librarian` with the following fields:
    *   `Librarian_ID` (VARCHAR(5), Primary Key)
    *   `Name` (VARCHAR(30))
    *   `HireDate` (DATE)
2.  Insert at least two librarian records.
3.  Add a foreign key in `Borrow` referencing `Librarian(Librarian_ID)`.

**Solution:**

```sql
-- 1. Create a table named Librarian
CREATE TABLE Librarian (
    Librarian_ID VARCHAR(5) PRIMARY KEY,
    Name VARCHAR(30),
    HireDate DATE
);

-- 2. Insert at least two librarian records
INSERT INTO Librarian (Librarian_ID, Name, HireDate) VALUES ('L001', 'John Smith', TO_DATE('2020-08-15', 'YYYY-MM-DD'));
INSERT INTO Librarian (Librarian_ID, Name, HireDate) VALUES ('L002', 'Jane Doe', TO_DATE('2021-02-20', 'YYYY-MM-DD'));

-- 3. Add a foreign key in Borrow referencing Librarian(Librarian_ID)
-- First, add the Librarian_ID column to the Borrow table
ALTER TABLE Borrow
ADD (Librarian_ID VARCHAR(5));

-- Then, add the foreign key constraint
ALTER TABLE Borrow
ADD CONSTRAINT fk_librarian FOREIGN KEY (Librarian_ID) REFERENCES Librarian(Librarian_ID);
```
```