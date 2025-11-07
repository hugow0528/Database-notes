Of course. This is the most important part of revision—understanding your own patterns so you can improve. Let's break it down.

Overall, you did a very good job. You have a solid grasp of the basic structure of SQL queries, and you learn very quickly. When I showed you the correct subquery structure in one question, you immediately applied it in the next. That's a great sign!

Based on our session, here is an analysis of your weaknesses and some revision notes for you, written in Cantonese for better understanding.

---

### **SQL 溫習筆記 (Revision Notes)**

我哋總結一下你喺練習中可以再加強嘅地方，主要集中喺三個方面：

#### **弱點一：`GROUP BY` 子句嘅規則 (The Rules of the `GROUP BY` Clause)**

呢個係你遇到嘅最大嘅概念性困難。喺第三題（計算每個客人嘅訂單數量）嗰陣，你嘅 `GROUP BY` 寫法有啲問題。

*   **你嘅問題 (Your Mistake):** `GROUP BY o.customer_ID, COUNT`
*   **核心問題 (The Core Problem):**
    1.  **唔可以 Group by 別名 (Cannot Group by an Alias):** 你唔可以將 `SELECT` 裡面用 `AS` 創造出嚟嘅名（例如 `COUNT`）直接放喺 `GROUP BY` 度用。因為數據庫喺處理 `GROUP BY` 嘅時候，`SELECT` 嗰句所產生嘅別名仲未存在。
    2.  **要 Group by `SELECT` 選擇嘅非聚合欄位 (Must Group by the non-aggregated columns from `SELECT`):** 你 `SELECT` 咗 `c.Name`，呢個唔係一個聚合函數（唔係 `COUNT`, `SUM` 等），所以你嘅 `GROUP BY` *一定*要包含 `c.Name` (或者更好嘅 `c.Customer_ID` 同 `c.Name`)。

*   **溫習口訣 (Mnemonic to Remember):**
    > 「`SELECT` 乜嘢，就 `GROUP BY` 乜嘢（聚合函數除外）。」
    > (Whatever you `SELECT`, you `GROUP BY` it (except for aggregate functions).)

*   **正確寫法 (Correct Way):**
    ```sql
    SELECT
        c.Name, -- 呢個係非聚合欄位
        COUNT(o.Order_ID) AS NumberOfOrders -- 呢個係聚合函數
    FROM Customer c
    LEFT JOIN Orders o ON c.Customer_ID = o.Customer_ID
    GROUP BY
        c.Customer_ID, c.Name -- 所以 GROUP BY 一定要有返 c.Name
    ORDER BY
        NumberOfOrders DESC;
    ```

---

#### **弱點二：集合運算符 (Set Operators) 嘅概念唔清晰**

喺第五、六題（Project Management）嗰陣，你唔係好確定點樣合併兩張表嘅結果。

*   **你嘅問題 (Your Mistake):**
    *   第五題（A **或** B），你嘗試用 `WHERE... = ... OR ...`，但係唔可以用 `=` 去比較一個欄位同一個 View/Table。
    *   第六題（A **同** B），你用咗 `UNION ALL`，但其實你需要嘅係 `INTERSECT`。

*   **核心概念 (The Core Concepts):**
    *   `UNION`: 合併兩個 `SELECT` 嘅結果，並**自動刪除重複**嘅資料。用喺「A **或** B」嘅情況。
    *   `INTERSECT`: 搵出兩個 `SELECT` 結果中，**共同擁有**嘅資料。用喺「A **同** B」（交集）嘅情況。
    *   `EXCEPT` / `MINUS`: 喺第一個 `SELECT` 結果中，**減去**第二個 `SELECT` 結果都有嘅資料。用喺「A 有，但 B **冇**」嘅情況。

*   **溫習口訣 (Mnemonic to Remember):**
    > 「或者 (`OR`) 就用 `UNION`，兩者都要 (`AND`) 就用 `INTERSECT`。」

---

#### **弱點三：語法上嘅小錯誤 (Minor Syntax Errors)**

呢個係最常見亦都係最容易解決嘅問題。你好幾次都係因為好細微嘅打字錯誤而令到 Query 唔完美。

*   **你嘅問題 (Your Mistakes):**
    *   `YEAR(signu0date)` (0 寫錯做 o)
    *   `SELECT PRODUCT_ ID` (ID 前面多咗個 space)

*   **核心問題 (The Core Problem):** 呢啲錯誤唔係概念問題，純粹係手快打錯字。喺真實考試或者工作環境，呢啲錯誤會導致 query 執行失敗。

*   **溫習重點 (Point to Remember):**
    > 「執行之前，用三秒時間檢查多次 Table 名同 Column 名有冇串錯。」
    > (Before executing, take three seconds to double-check if table names and column names are spelled correctly.)

### **點樣可以進步？ (How Can You Improve?)**

1.  **針對性練習 (Targeted Practice):**
    *   **`GROUP BY`:** 專登去搵多啲題目係要求你「計算每個類別嘅總和」或者「搵出每個學生參加咗幾多個活動」。呢啲練習一定會用到 `GROUP BY` 同 `COUNT()` / `SUM()`，可以迫你記熟佢嘅規則。
    *   **Set Operators:** 睇返你老師俾你嘅 Exercise 3，唔好睇答案，自己重新做一次。然後搵啲類似嘅練習，專登問「A或B」、「A同B都有」、「A有但B冇」嘅題目，練習用 `UNION`, `INTERSECT`, `EXCEPT`。

2.  **養成檢查嘅習慣 (Develop a Habit of Checking):** 喺你寫完一句 SQL 之後，唔好即刻諗住執行。先快速掃一次，特別係你 `FROM`, `JOIN`, `WHERE` 後面嘅欄位名，確保冇串錯字或者多咗空格。

3.  **理解 SQL 執行順序 (Understand SQL's Order of Execution):** 記住一個概念，數據庫唔係由上至下咁讀你嘅 Code。佢嘅邏輯順序大概係：
    1.  `FROM` / `JOIN` (先決定用邊啲表)
    2.  `WHERE` (過濾啲行)
    3.  `GROUP BY` (將過濾完嘅行分組)
    4.  `HAVING` (過濾啲組)
    5.  `SELECT` (揀出最後要顯示嘅欄位，同埋喺呢度做 `AS` 改名)
    6.  `ORDER BY` (最後先排序)
    *   當你明白 `GROUP BY` 係喺 `SELECT` 之前發生，你就會明白點解 `GROUP BY` 唔可以用 `SELECT` 裡面嘅別名 (`alias`)。

You are on the right track. Your logical thinking is strong, and now you just need to polish these few technical details. Keep practicing, and you'll do great in your test