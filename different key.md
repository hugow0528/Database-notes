

Notes on Database Keys / 資料庫鍵筆記
1. Candidate Key (候選鍵)
	•	Definition (English): A candidate key is a column or a set of columns in a table that can uniquely identify each row. It must be unique (no duplicate values) and minimal (no extra columns than needed). A table can have multiple candidate keys.
	•	定義 (Cantonese): 候選鍵係表格入面嘅一欄或一組欄，可以獨一無二咁辨識每一行。佢必須係獨特（唔可以有重複值）同最少欄數（唔可以有多餘欄）。一個表格可以有多個候選鍵。
	•	Sample Usage: In a Students table:
	◦	Columns: StudentID, Email, Name, Phone
	◦	Possible candidate keys: StudentID (unique for each student) or Email (if every student has a unique email).
	◦	Explanation: Both StudentID and Email can identify a student uniquely, so they’re candidate keys.
	•	例子: 喺一個學生表格入面：
	◦	欄位：學生編號, 電郵, 姓名, 電話
	◦	可能嘅候選鍵：學生編號（每個學生獨一無二）或電郵（如果每個學生嘅電郵都唔同）。
	◦	解釋：學生編號同電郵都可以獨特辨識一個學生，所以佢哋係候選鍵。
2. Primary Key (主鍵)
	•	Definition (English): A primary key is a single candidate key chosen to be the main way to identify rows in a table. It must be unique, not null (no empty values), and there’s only one primary key per table.
	•	定義 (Cantonese): 主鍵係從候選鍵入面揀出嘅一個鍵，用來作為表格嘅主要辨識方法。佢必須係獨特、唔可以有空值，而且一個表格只有一個主鍵。
	•	Sample Usage: In the Students table:
	◦	Chosen primary key: StudentID
	◦	Explanation: StudentID is chosen over Email because it’s shorter, more reliable (emails can change), and guaranteed unique. Every row must have a StudentID, and no two students can share the same StudentID.
	•	例子: 喺學生表格入面：
	◦	揀定嘅主鍵：學生編號
	◦	解釋：揀學生編號而唔係電郵，因為佢更短、更可靠（電郵可能會變），而且保證獨一無二。每行都要有學生編號，唔可以有兩個學生用同一個學生編號。
3. Composite Key (複合鍵)
	•	Definition (English): A composite key is a primary key (or candidate key) that consists of two or more columns to uniquely identify a row. It’s used when no single column is unique enough on its own.
	•	定義 (Cantonese): 複合鍵係由兩個或以上欄位組成嘅主鍵（或候選鍵），用來獨一無二咁辨識一行。當單一欄唔夠獨特時，就會用複合鍵。
	•	Sample Usage: In a ClassEnrollments table:
	◦	Columns: StudentID, CourseID, EnrollmentDate
	◦	Composite key: StudentID + CourseID
	◦	Explanation: Neither StudentID (a student can enroll in multiple courses) nor CourseID (a course has multiple students) is unique alone. Together, StudentID + CourseID ensures each enrollment is unique (a student can’t enroll in the same course twice).
	•	例子: 喺一個課程報讀表格入面：
	◦	欄位：學生編號, 課程編號, 報讀日期
	◦	複合鍵：學生編號 + 課程編號
	◦	解釋：單獨嘅學生編號（一個學生可以報讀多個課程）同課程編號（一個課程有多個學生）都唔係獨特。但一齊用學生編號 + 課程編號，就可以確保每次報讀係獨一無二（一個學生唔可以重複報讀同一個課程）。
4. Foreign Key (外鍵)
	•	Definition (English): A foreign key is a column (or set of columns) in one table that refers to the primary key in another table. It creates a relationship between tables and ensures data consistency (the foreign key value must exist in the referenced primary key).
	•	定義 (Cantonese): 外鍵係一個表格入面嘅欄（或一組欄），佢會指向另一個表格嘅主鍵。佢用來建立表格之間嘅關係，確保資料一致性（外鍵值一定要喺參考嘅主鍵入面存在）。
	•	Sample Usage: In the ClassEnrollments table:
	◦	Foreign key: StudentID (refers to StudentID in the Students table)
	◦	Explanation: The StudentID in ClassEnrollments must match a StudentID in the Students table. This ensures only valid students can be enrolled in courses.
	•	例子: 喺課程報讀表格入面：
	◦	外鍵：學生編號（指向學生表格嘅學生編號）
	◦	解釋：課程報讀表格入面嘅學生編號必須同學生表格入面嘅學生編號吻合。呢個確保只有有效嘅學生先可以報讀課程。

Example Database: School System / 學校系統例子
Let’s tie it all together with a sample database for a school:
	•	Students Table:
	◦	Columns: StudentID (primary key), Email, Name, Phone
	◦	Candidate Keys: StudentID, Email
	◦	Primary Key: StudentID (chosen for reliability)
	•	Courses Table:
	◦	Columns: CourseID (primary key), CourseName, Teacher
	◦	Candidate Key: CourseID
	◦	Primary Key: CourseID
	•	ClassEnrollments Table:
	◦	Columns: StudentID, CourseID, EnrollmentDate
	◦	Composite Key: StudentID + CourseID (primary key, as neither is unique alone)
	◦	Foreign Keys:
	▪	StudentID (links to StudentID in Students)
	▪	CourseID (links to CourseID in Courses)
	•	Explanation:
	◦	The Students and Courses tables use single-column primary keys (StudentID, CourseID).
	◦	The ClassEnrollments table uses a composite key because a student can enroll in multiple courses, and a course can have multiple students.
	◦	Foreign keys in ClassEnrollments ensure that only valid students and courses are referenced.
Cantonese 例子:
	•	學生表格：
	◦	欄位：學生編號（主鍵）, 電郵, 姓名, 電話
	◦	候選鍵：學生編號, 電郵
	◦	主鍵：學生編號（因為更可靠而揀）
	•	課程表格：
	◦	欄位：課程編號（主鍵）, 課程名稱, 教師
	◦	候選鍵：課程編號
	◦	主鍵：課程編號
	•	課程報讀表格：
	◦	欄位：學生編號, 課程編號, 報讀日期
	◦	複合鍵：學生編號 + 課程編號（主鍵，因為單獨一欄唔夠獨特）
	◦	外鍵：
	▪	學生編號（連繫到學生表格嘅學生編號）
	▪	課程編號（連繫到課程表格嘅課程編號）
	•	解釋：
	◦	學生同課程表格用單一欄主鍵（學生編號, 課程編號）。
	◦	課程報讀表格用複合鍵，因為一個學生可以報讀多個課程，一個課程有多個學生。
	◦	外鍵確保只可以參考有效嘅學生同課程。

Key Differences / 主要分別
	•	Candidate Key: Any column(s) that can uniquely identify rows. A table can have multiple candidate keys. 候選鍵：任何可以獨特辨識行嘅欄，一個表格可以有多個候選鍵。
	•	Primary Key: The chosen candidate key to act as the main identifier. Only one per table, no nulls. 主鍵：揀定嘅候選鍵作為主要辨識鍵，一個表格只有一個，唔可以有空值。
	•	Composite Key: A primary (or candidate) key with multiple columns for uniqueness. 