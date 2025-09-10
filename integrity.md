# Database Integrity Notes / 資料庫完整性筆記

**Integrity (完整性)** refers to the accuracy, consistency, and reliability of data in a database. It ensures that the data is valid and follows specific rules. There are three main types of integrity: **entity integrity**, **referential integrity**, and **domain integrity**.

**完整性**指資料庫入面數據嘅準確性、一致性同可靠性。佢確保數據有效同符合特定規則。完整性有三個主要類型：**實體完整性**、**參照完整性**同**域完整性**。

## 1. Entity Integrity (實體完整性)
- **Definition (English)**: Entity integrity ensures that each row in a table is uniquely identifiable and has no missing (null) values in its **primary key**. This guarantees that every record can be distinguished from others.
- **定義 (Cantonese)**: 實體完整性確保表格入面每一行都可以獨一無二咁辨識，而且**主鍵**唔可以有空值（null）。呢個保證每條記錄都可以同其他記錄區分開。
- **Connection to Keys**: Relates to the **primary key** (chosen from candidate keys). The primary key must be unique and non-null to maintain entity integrity.
- **與鍵嘅關係**: 同**主鍵**有關（從候選鍵揀出）。主鍵必須獨特同唔可以有空值，以保持實體完整性。
- **Sample Usage**: In a **Students** table:
  - Columns: `StudentID` (primary key), `Name`, `Email`
  - Entity Integrity Rule: Every `StudentID` must be unique (e.g., no two students can have ID "S001"), and `StudentID` cannot be null (every student must have an ID).
  - **Explanation**: If `StudentID` were null or duplicated, you couldn’t reliably identify a student, breaking entity integrity.
- **例子**: 喺一個**學生**表格入面：
  - 欄位：`學生編號`（主鍵）, `姓名`, `電郵`
  - 實體完整性規則：每個`學生編號`必須獨一無二（例如，唔可以有兩個學生用"S001"），而且`學生編號`唔可以係空值（每個學生都要有編號）。
  - **解釋**: 如果`學生編號`係空值或者重複，就唔可以可靠咁辨識學生，會破壞實體完整性。

## 2. Referential Integrity (參照完整性)
- **Definition (English)**: Referential integrity ensures that a **foreign key** value in one table matches a valid **primary key** value in another table, or is null. This maintains consistent relationships between tables.
- **定義 (Cantonese)**: 參照完整性確保一個表格入面嘅**外鍵**值必須同另一個表格嘅**主鍵**值吻合，或者係空值。呢個保持表格之間嘅關係一致。
- **Connection to Keys**: Relates to **foreign keys** and their link to primary keys in related tables. It ensures that relationships (e.g., between students and their enrollments) are valid.
- **與鍵嘅關係**: 同**外鍵**同佢連繫到相關表格嘅主鍵有關。佢確保關係（例如學生同佢哋嘅報讀記錄）係有效。
- **Sample Usage**: In a **ClassEnrollments** table:
  - Columns: `StudentID` (foreign key), `CourseID` (foreign key), `EnrollmentDate`
  - Referential Integrity Rule: `StudentID` must match a `StudentID` in the **Students** table, and `CourseID` must match a `CourseID` in the **Courses** table. If a `StudentID` doesn’t exist in **Students**, the enrollment is invalid.
  - **Explanation**: This prevents adding enrollments for non-existent students or courses, keeping data consistent.
- **例子**: 喺一個**課程報讀**表格入面：
  - 欄位：`學生編號`（外鍵）, `課程編號`（外鍵）, `報讀日期`
  - 參照完整性規則：`學生編號`必須同**學生**表格入面嘅`學生編號`吻合，`課程編號`必須同**課程**表格入面嘅`課程編號`吻合。如果`學生編號`喺**學生**表格不存在，報讀記錄就無效。
  - **解釋**: 呢個防止加入不存在學生或課程嘅報讀記錄，保持數據一致。

## 3. Domain Integrity (域完整性)
- **Definition (English)**: Domain integrity ensures that all values in a column fall within a defined range or set of acceptable values (the "domain"). This is enforced through data types, constraints (e.g., NOT NULL, CHECK), or rules.
- **定義 (Cantonese)**: 域完整性確保一欄入面嘅所有值都喺定義嘅範圍或可接受值集合（「域」）之內。呢個透過數據類型、約束（例如 NOT NULL、CHECK）或規則來執行。
- **Connection to Keys**: While not directly tied to keys, domain integrity ensures that values in columns (including those used in candidate, primary, composite, or foreign keys) are valid for their purpose.
- **與鍵嘅關係**: 雖然同鍵無直接關係，但域完整性確保欄位值（包括用喺候選鍵、主鍵、複合鍵或外鍵嘅欄）符合佢哋嘅用途。
- **Sample Usage**: In a **Students** table:
  - Column: `GradeLevel` (e.g., for high school, values must be 9, 10, 11, or 12)
  - Domain Integrity Rule: A CHECK constraint ensures `GradeLevel` is between 9 and 12. For example, a value like "13" or "ABC" would be invalid.
  - **Explanation**: This ensures that only valid grade levels are entered, maintaining data accuracy.
- **例子**: 喺一個**學生**表格入面：
  - 欄位：`年級`（例如，高中年級必須係 9、10、11 或 12）
  - 域完整性規則：CHECK 約束確保`年級`喺 9 到 12 之間。例如，值"13"或"ABC"係無效。
  - **解釋**: 呢個確保只輸入有效嘅年級，保持數據準確。

---

## Example Database: School System / 學校系統例子
Let’s see how integrity applies in a school database:
- **Students Table / 學生表格**:
  - Columns: `StudentID` (primary key), `Name`, `Email`, `GradeLevel`
  - **Entity Integrity**: `StudentID` must be unique and non-null (e.g., "S001", "S002", no duplicates or nulls).
  - **Domain Integrity**: `GradeLevel` must be 9, 10, 11, or 12 (enforced by a CHECK constraint).
- **Courses Table / 課程表格**:
  - Columns: `CourseID` (primary key), `CourseName`, `Teacher`
  - **Entity Integrity**: `CourseID` must be unique and non-null (e.g., "C101", "C102").
  - **Domain Integrity**: `CourseName` must be a valid string (e.g., not a number).
- **ClassEnrollments Table / 課程報讀表格**:
  - Columns: `StudentID` (foreign key), `CourseID` (foreign key), `EnrollmentDate`
  - **Referential Integrity**: `StudentID` must exist in **Students** table, and `CourseID` must exist in **Courses** table.
  - **Entity Integrity**: Composite key (`StudentID` + `CourseID`) must be unique and non-null.
  - **Domain Integrity**: `EnrollmentDate` must be a valid date (e.g., not "2025-13-01").
- **Explanation**: 
  - Entity integrity ensures every student and course has a unique identifier.
  - Referential integrity ensures enrollments only reference valid students and courses.
  - Domain integrity ensures values like grades and dates are valid.
- **解釋**:
  - 實體完整性確保每個學生同課程有獨一無二嘅辨識符。
  - 參照完整性確保報讀記錄只參考有效嘅學生同課程。
  - 域完整性確保值如年級同日期係有效。

---

## Key Summary / 主要總結
| Integrity Type | Purpose | Related Key | Example Rule |
|----------------|---------|-------------|--------------|
| **Entity Integrity** | Ensures unique, non-null primary keys | Primary Key | `StudentID` must be unique and not null |
| **Referential Integrity** | Ensures valid foreign key relationships | Foreign Key | `StudentID` in **ClassEnrollments** must exist in **Students** |
| **Domain Integrity** | Ensures valid column values | Any column | `GradeLevel` must be 9–12 |

| 完整性類型 | 目的 | 相關鍵 | 例子規則 |
|------------|------|--------|----------|
| **實體完整性** | 確保獨特同非空嘅主鍵 | 主鍵 | `學生編號`必須獨特同唔可以係空值 |
| **參照完整性** | 確保有效嘅外鍵關係 | 外鍵 | **課程報讀**入面嘅`學生編號`必須喺**學生**表格存在 |
| **域完整性** | 確保欄位值有效 | 任何欄 | `年級`必須係 9–12 |

---

## Check Your Understanding / 檢查理解
**Question**: Imagine you’re designing a **LibraryLoans** table to track book borrowing:
1. How would you ensure **entity integrity** for this table?
2. How would you enforce **referential integrity** if it links to a **Books** table and a **Borrowers** table?
3. Suggest a **domain integrity** rule for a column like `LoanDate`.

**問題**: 假設你設計一個**圖書館借書**表格來記錄借書：
1. 你點樣確保呢個表格嘅**實體完整性**？
2. 如果呢個表格連繫到**書籍**表格同**借書人**表格，你點樣執行**參照完整性**？
3. 為一個欄位如`借書日期`建議一個**域完整性**規則。

