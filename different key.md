# Database Keys Notes / 資料庫鍵筆記

## 1. Candidate Key (候選鍵)
- **Definition (English)**: A candidate key is a column or set of columns in a table that can uniquely identify each row. It must be **unique** (no duplicates) and **minimal** (no extra columns needed). A table can have multiple candidate keys.
- **定義 (Cantonese)**: 候選鍵係表格入面嘅一欄或一組欄，可以獨一無二咁辨識每一行。佢必須係**獨特**（唔可以有重複值）同**最少欄數**（唔可以有多餘欄）。一個表格可以有多個候選鍵。
- **Sample Usage**: In a **Students** table:
  - Columns: `StudentID`, `Email`, `Name`, `Phone`
  - Candidate Keys: `StudentID` (unique for each student), `Email` (if every student has a unique email)
  - **Explanation**: Both `StudentID` and `Email` can uniquely identify a student, making them candidate keys.
- **例子**: 喺一個**學生**表格入面：
  - 欄位：`學生編號`, `電郵`, `姓名`, `電話`
  - 候選鍵：`學生編號`（每個學生獨一無二）, `電郵`（如果每個學生嘅電郵都唔同）
  - **解釋**: `學生編號`同`電郵`都可以獨特辨識一個學生，所以佢哋係候選鍵。

## 2. Primary Key (主鍵)
- **Definition (English)**: A primary key is a single candidate key chosen as the main way to identify rows in a table. It must be **unique**, **not null** (no empty values), and there’s only **one primary key** per table.
- **定義 (Cantonese)**: 主鍵係從候選鍵入面揀出嘅一個鍵，用來作為表格嘅主要辨識方法。佢必須係**獨特**、**唔可以有空值**，而且一個表格只有**一個主鍵**。
- **Sample Usage**: In the **Students** table:
  - Primary Key: `StudentID`
  - **Explanation**: `StudentID` is chosen over `Email` because it’s shorter, more reliable (emails can change), and guaranteed unique. Every row must have a `StudentID`, and no two students can share the same `StudentID`.
- **例子**: 喺**學生**表格入面：
  - 主鍵：`學生編號`
  - **解釋**: 揀`學生編號`而唔係`電郵`，因為佢更短、更可靠（電郵可能會變），而且保證獨一無二。每行都要有`學生編號`，唔可以有兩個學生用同一個`學生編號`。

## 3. Composite Key (複合鍵)
- **Definition (English)**: A composite key is a primary key (or candidate key) that consists of **two or more columns** to uniquely identify a row. It’s used when no single column is unique enough on its own.
- **定義 (Cantonese)**: 複合鍵係由**兩個或以上欄位**組成嘅主鍵（或候選鍵），用來獨一無二咁辨識一行。當單一欄唔夠獨特時，就會用複合鍵。
- **Sample Usage**: In a **ClassEnrollments** table:
  - Columns: `StudentID`, `CourseID`, `EnrollmentDate`
  - Composite Key: `StudentID` + `CourseID`
  - **Explanation**: Neither `StudentID` (a student can enroll in multiple courses) nor `CourseID` (a course has multiple students) is unique alone. Together, `StudentID` + `CourseID` ensures each enrollment is unique (a student can’t enroll in the same course twice).
- **例子**: 喺一個**課程報讀**表格入面：
  - 欄位：`學生編號`, `課程編號`, `報讀日期`
  - 複合鍵：`學生編號` + `課程編號`
  - **解釋**: 單獨嘅`學生編號`（一個學生可以報讀多個課程）同`課程編號`（一個課程有多個學生）都唔係獨特。但一齊用`學生編號` + `課程編號`，就可以確保每次報讀係獨一無二（一個學生唔可以重複報讀同一個課程）。

## 4. Foreign Key (外鍵)
- **Definition (English)**: A foreign key is a column (or set of columns) in one table that refers to the **primary key** in another table. It creates a relationship between tables and ensures data consistency (the foreign key value must exist in the referenced primary key).
- **定義 (Cantonese)**: 外鍵係一個表格入面嘅欄（或一組欄），佢會指向另一個表格嘅**主鍵**。佢用來建立表格之間嘅關係，確保資料一致性（外鍵值一定要喺參考嘅主鍵入面存在）。
- **Sample Usage**: In the **ClassEnrollments** table:
  - Foreign Key: `StudentID` (refers to `StudentID` in the **Students** table)
  - **Explanation**: The `StudentID` in **ClassEnrollments** must match a `StudentID` in the **Students** table, ensuring only valid students can be enrolled.
- **例子**: 喺**課程報讀**表格入面：
  - 外鍵：`學生編號`（指向**學生**表格嘅`學生編號`）
  - **解釋**: **課程報讀**表格入面嘅`學生編號`必須同**學生**表格入面嘅`學生編號`吻合，確保只有有效嘅學生先可以報讀課程。

---

## Example Database: School System / 學校系統例子
- **Students Table / 學生表格**:
  - Columns: `StudentID` (primary key), `Email`, `Name`, `Phone`
  - Candidate Keys: `StudentID`, `Email`
  - Primary Key: `StudentID` (chosen for reliability)
- **Courses Table / 課程表格**:
  - Columns: `CourseID` (primary key), `CourseName`, `Teacher`
  - Candidate Key: `CourseID`
  - Primary Key: `CourseID`
- **ClassEnrollments Table / 課程報讀表格**:
  - Columns: `StudentID`, `CourseID`, `EnrollmentDate`
  - Composite Key: `StudentID` + `CourseID` (primary key, as neither is unique alone)
  - Foreign Keys:
    - `StudentID` (links to `StudentID` in **Students**)
    - `CourseID` (links to `CourseID` in **Courses**)
- **Explanation**: 
  - **Students** and **Courses** use single-column primary keys.
  - **ClassEnrollments** uses a composite key because a student can enroll in multiple courses, and a course can have multiple students.
  - Foreign keys ensure only valid students and courses are referenced.
- **解釋**:
  - **學生**同**課程**表格用單一欄主鍵。
  - **課程報讀**表格用複合鍵，因為一個學生可以報讀多個課程，一個課程有多個學生。
  - 外鍵確保只可以參考有效嘅學生同課程。

---

## Key Differences / 主要分別
| Key Type | Description | Unique? | Null Allowed? | Multiple per Table? |
|----------|-------------|---------|---------------|---------------------|
| **Candidate Key** | Column(s) that can uniquely identify rows | Yes | No | Yes |
| **Primary Key** | Chosen candidate key for main identification | Yes | No | Only one |
| **Composite Key** | Primary/candidate key with multiple columns | Yes | No | Yes (if candidate keys) |
| **Foreign Key** | Links to primary key in another table | No (can repeat) | Yes (sometimes) | Yes |

| 鍵類型 | 描述 | 獨特？ | 允許空值？ | 表格可有多個？ |
|--------|------|-------|-----------|---------------|
| **候選鍵** | 可以獨特辨識行嘅欄 | 是 | 否 | 是 |
| **主鍵** | 揀定嘅主要辨識鍵 | 是 | 否 | 只有一個 |
| **複合鍵** | 多欄組成嘅主鍵/候選鍵 | 是 | 否 | 是（如果係候選鍵） |
| **外鍵** | 連繫到另一表格主鍵 | 唔一定（可重複） | 有時可以 | 是 |

---

## Check Your Understanding / 檢查理解
**Question**: Design a table for **LibraryBooks** (tracking books in a library). Suggest:
1. A possible candidate key.
2. A primary key and why you chose it.
3. A situation where a composite key might be needed.
4. A foreign key if this table links to a **Borrowers** table.

**問題**: 設計一個**圖書館書籍**表格（記錄圖書館嘅書）。請建議：
1. 一個可能嘅候選鍵。
2. 一個主鍵同點解揀佢。
3. 一個可能需要複合鍵嘅情況。
4. 如果呢個表格連繫到**借書人**表格，一個外鍵係點樣。

Try answering, and I’ll guide you if needed!

---

## Next Question Suggestions / 下一問題建議
1. Can you provide another example of a composite key in a different database context?  
   你可唔可以畀多一個喺唔同資料庫情境入面嘅複合鍵例子？  
2. What happens if I try to insert a foreign key value that doesn’t exist in the referenced primary key?  
   如果我試圖插入一個外鍵值，但佢唔存在於參考嘅主鍵，會發生咩事？  
3. How do I decide which candidate key to use as the primary key for a table?  
   我點樣決定用邊個候選鍵做表格嘅主鍵？