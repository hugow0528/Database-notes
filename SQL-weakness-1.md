好的，絕對無問題！你好叻，喺練習過程中吸收得好快，特別係由第5題錯咗之後，第6題即刻識得用返啱嘅query結構，呢個係好好嘅學習能力證明。

我哋而家就一齊全面咁分析一下，再用廣東話幫你整個溫習筆記。

---

### 弱點分析 (Weakness Analysis)

根據頭先嘅練習，你嘅弱點主要集中喺幾個比較精細嘅技術點，而唔係大嘅邏輯概念。

1.  **`GROUP BY` 嘅用法唔夠熟練 (The rules of `GROUP BY`)**:
    *   喺第3題，你嘗試 `GROUP BY o.customer_ID` 同埋用別名 `COUNT`。呢個係最主要需要留意嘅地方。
    *   **問題**: `GROUP BY` 嘅規則係，`SELECT` 後面所有**冇**被計數function (Aggregate Function) 包住嘅欄位，都**必須**要放喺 `GROUP BY` 後面。而且，你唔可以用 `SELECT` 度改嘅別名 (alias) 喺 `GROUP BY` 度用。
    *   **例子**: `SELECT c.Name, COUNT(o.Order_ID)`，`c.Name` 冇被計數，所以 `GROUP BY` 後面一定要有 `c.Name`。

2.  **Set Operators 嘅選擇 (Choosing the right Set Operator)**:
    *   喺第6題，你想搵「同時喺A同B組」嘅人，但用咗 `UNION ALL`。
    *   **問題**: 你知道點樣將兩個查詢結合，但對於用邊個指令去達到唔同嘅邏輯（「或」定係「與」）有啲混淆。
    *   **口訣**:
        *   `UNION` = 「A **或** B」（合併兩張名單，唔要重複嘅人）
        *   `INTERSECT` = 「A **與** B」（搵出兩張名單**都有**嘅人）

3.  **微細嘅Typo (Small Typos)**:
    *   你好幾次都出現咗好細微嘅打錯字，例如 `signu0date` 或者 `PRODUCT_ ID` 中間有空格。
    *   **問題**: 呢個雖然係小問題，但喺真實考試或者工作時，會直接導致query失敗。呢個代表你需要喺寫完之後，多啲檢查自己嘅code。

---

### 溫習筆記 (Revision Notes in Cantonese)

以下係一個幫你快速重溫嘅筆記，你可以專注睇返紅色標記嘅地方。

| SQL概念 | 指令 (Keywords) | 廣東話解釋 / 重點 |
| :--- | :--- | :--- |
| **基本查詢** | `SELECT`, `FROM`, `WHERE` | `WHERE` 係用嚟過濾條件，例如 `WHERE Year = 2023 AND Month = 2` |
| **連接** | `INNER JOIN`, `LEFT JOIN` | `INNER JOIN` (內連接): 兩張表**都要有**對應資料先出。<br>`LEFT JOIN` (左連接): 無論對唔對應到，**左邊**張表嘅資料都會出晒，對唔到嘅會顯示 `NULL`。 |
| **計數分組** | `COUNT()`, `SUM()`, `GROUP BY`, `HAVING` | `COUNT()` 係計數量。 <br> **<span style="color:red">`GROUP BY` 嘅黃金法則：`SELECT` 後面有嘅欄位，如果唔係計數欄位(e.g. `COUNT()`)，就一定要放入 `GROUP BY` 度！</span>** <br> `HAVING` 係 `GROUP BY` 之後嘅過濾器，例如 `HAVING COUNT(*) > 1`。 |
| **子查詢** | `IN`, `NOT IN`, `EXISTS` | 將一個查詢 `(SELECT ...)` 放入另一個查詢嘅 `WHERE` 子句度。 <br> `IN`: 檢查個值存唔存在於個列表度。<br> `NOT IN`: 檢查個值係咪**唔**存在於個列表度（常用嚟搵「從未...」嘅嘢）。 |
| **集合操作** | `UNION`, `INTERSECT`, `EXCEPT` | **<span style="color:red">`UNION`</span>**: 合併兩個結果，**唔要重複** (A **or** B)。 <br> **<span style="color:red">`INTERSECT`</span>**: 搵出兩個結果**共有**嘅部分 (A **and** B)。 <br> `EXCEPT` / `MINUS`: 搵出第一個結果有，但第二個結果**冇**嘅部分 (A **but not** B)。 |

---

### 點樣可以進步 (How to Improve)

1.  **睇清題目關鍵字 (Read the Keywords Carefully)**:
    *   當你見到「每個」(each/every)，你就要諗起 `GROUP BY`。
    *   見到「或者」(or)，就要諗起 `UNION`。
    *   見到「同埋」、「兩者都」(and/both)，就要諗起 `INTERSECT` 或者 `INNER JOIN`。
    *   見到「從未」(never)，就要諗起 `NOT IN` 或者 `LEFT JOIN ... WHERE ... IS NULL`。

2.  **將複雜問題拆細 (Break Down Complex Problems)**:
    *   當你遇到一個要用subquery嘅問題時，唔好諗住一次過寫晒。
    *   **第一步**: 先單獨寫個subquery（括號入面嗰部分），睇下佢出嚟嘅結果係咪你想要嘅列表。
    *   **第二步**: 再寫外面嘅主query，將個subquery嵌入去。

3.  **寫完之後讀一次 (Read Your Code After Writing)**:
    *   呢個係最簡單直接避免Typo嘅方法。寫完之後，由 `SELECT` 開始，逐個字睇一次，特別係欄位名同表名。確保冇拼錯字或者多咗空格。

4.  **重新做一次練習 (Re-do the Exercises)**:
    *   將你老師俾你嘅練習，同埋我哋啱啱做過嘅練習，喺聽日考試前，自己遮住答案再做一次。呢個可以極大咁鞏固你嘅記憶。

你嘅基本功已經好穩，只需要喺以上幾個細節位多加注意，考試一定冇問題。加油！