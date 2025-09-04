# Notes on Database Anomalies and Normalization (資料庫異常同標準化筆記)

## Part 1: Database Anomalies (資料庫異常)

Database anomalies (資料庫異常) are problems in poorly designed databases that cause issues when you **insert (插入)**, **update (更新)**, or **delete (刪除)** data. These happen because data is not organized efficiently, often in tables that aren’t **normalized (標準化)**.

### 1. Insertion Anomaly (插入異常)

- **Definition**: You can’t add new data to a table without including unnecessary or unrelated data due to the table’s design.
- **In Cantonese**: 插入異常係因為資料庫設計問題，想加新資料但要連同唔需要嘅資料一齊加，否則加唔到。
- **Example in Food Order System**:  
  **Table (Not Normalized)**:  

  | Order_ID | Customer_Name | Food_Item | Customer_Phone |
  |----------|---------------|-----------|----------------|
  | 001      | 阿明          | Pizza     | 12345678       |

  If the system requires a `Food_Item` to create an order, you can’t add a new customer (e.g., just their name and phone) without selecting a food item, even if they haven’t decided yet.
- **Why It’s a Problem**: Forces you to include data (like `Food_Item`) that may not be available, limiting flexibility.
- **In Cantonese**: 喺點餐系統，如果一定要有食物項目先可以加訂單，你唔可以只加客戶名同電話，限制咗操作。

### 2. Update Anomaly (更新異常)

- **Definition**: Updating data causes inconsistencies because the same information is stored in multiple rows.
- **In Cantonese**: 更新異常係當你改資料，但因為同一資料喺多處重複儲存，改咗一處但漏咗其他，會有唔一致。
- **Example in Food Order System**:  
  **Table (Not Normalized)**:  

  | Order_ID | Customer_Name | Food_Item | Restaurant_Phone |
  |----------|---------------|-----------|------------------|
  | 001      | 阿明          | Pizza     | 98765432         |
  | 002      | 阿明          | Burger    | 98765432         |

  If the restaurant changes its phone number, you must update every row. Missing one row means some orders have the old number, causing confusion.
- **Why It’s a Problem**: Redundant data makes updates tedious and prone to errors.
- **In Cantonese**: 餐廳電話喺每個訂單重複儲存，改電話要改晒所有行，漏改一兩個會有舊同新電話混亂。

### 3. Deletion Anomaly (刪除異常)

- **Definition**: Deleting one piece of data accidentally removes other important data because it’s stored in the same table.
- **In Cantonese**: 刪除異常係當你刪除一項資料，連其他重要資料都一齊刪咗，因為佢哋喺同一表。
- **Example in Food Order System**:  
  **Table (Not Normalized)**:  

  | Order_ID | Customer_Name | Food_Item | Customer_Phone |
  |----------|---------------|-----------|----------------|
  | 001      | 阿明          | Pizza     | 12345678       |

  If you delete this order (e.g., customer cancels), you lose `Customer_Phone`. If this was the only order for 阿明, you lose their contact info entirely.
- **Why It’s a Problem**: You lose valuable data unintentionally, reducing the database’s usefulness.
- **In Cantonese**: 刪除訂單時，連客戶電話都唔見，如果嗰個客戶冇其他訂單，你就完全失去佢嘅聯繫方式。

## Part 2: Normalization (標準化)

Normalization organizes a database into tables to eliminate anomalies. It follows steps called **normal forms (範式)**: **1NF**, **2NF**, and **3NF**. Each step makes the database more efficient.

### 1. First Normal Form (1NF, 第一範式)

- **Definition**: A table is in 1NF if:  
  1. Each column has only one value (no lists or multiple values).  
  2. Each row is unique (no duplicates).  
  3. Each column has a clear purpose.
- **In Cantonese**: 1NF 確保每欄只有一個值，無重複行，每欄有清晰用途。
- **Example in Food Order System**:  
  **Table (Not 1NF)**:  

  | Order_ID | Customer_Name | Food_Items         | Customer_Phone |
  |----------|---------------|--------------------|----------------|
  | 001      | 阿明          | Pizza, Burger      | 12345678       |

  Problem: `Food_Items` has multiple values (“Pizza, Burger”).  
  **Fix for 1NF**: Split into two tables:  
  - **Order Table**:  

    | Order_ID | Customer_Name | Customer_Phone |
    |----------|---------------|----------------|
    | 001      | 阿明          | 12345678       |

  - **Order_Items Table**:  

    | Order_ID | Food_Item |
    |----------|-----------|
    | 001      | Pizza     |
    | 001      | Burger    |

- **How It Helps Anomalies**:  
  - **Insertion**: Add an order without needing a food item.  
  - **Update**: Update a single food item without affecting others.  
  - **Deletion**: Delete a food item without losing customer data.
- **In Cantonese**: 如果 `Food_Items` 有好多值（例如 Pizza, Burger），唔係 1NF。分開成獨立行，每個食物一條記錄，解決插入、更新同刪除問題。

### 2. Second Normal Form (2NF, 第二範式)

- **Definition**: A table is in 2NF if:  
  1. It’s in 1NF.  
  2. All non-key columns fully depend on the **entire primary key** (not just part of it).  
- **In Cantonese**: 2NF 係喺 1NF 基礎上，確保非主鍵欄完全依賴整個主鍵，而唔係一部分。
- **Example in Food Order System**:  
  **Table (1NF but not 2NF)**:  

  | Order_ID | Food_Item | Food_Price | Customer_Name |
  |----------|-----------|------------|---------------|
  | 001      | Pizza     | $100       | 阿明          |
  | 001      | Burger    | $80        | 阿明          |

  Primary key: `(Order_ID, Food_Item)`. Problem: `Customer_Name` depends only on `Order_ID`, not `Food_Item`, violating 2NF.  
  **Fix for 2NF**: Split into:  
  - **Order Table**:  

    | Order_ID | Customer_Name |
    |----------|---------------|
    | 001      | 阿明          |

  - **Order_Items Table**:  

    | Order_ID | Food_Item | Food_Price |
    |----------|-----------|------------|
    | 001      | Pizza     | $100       |
    | 001      | Burger    | $80        |

- **How It Helps Anomalies**:  
  - **Insertion**: Add a customer without needing a food item.  
  - **Update**: Change `Customer_Name` once in the Order Table.  
  - **Deletion**: Delete a food item without losing `Customer_Name`.
- **In Cantonese**: `Customer_Name` 只同 `Order_ID` 有關，唔同 `Food_Item` 有關，唔係 2NF。分開表，`Customer_Name` 放喺 Order Table，解決異常。

### 3. Third Normal Form (3NF, 第三範式)

- **Definition**: A table is in 3NF if:  
  1. It’s in 2NF.  
  2. No non-key column depends on another non-key column (no transitive dependencies).  
- **In Cantonese**: 3NF 係喺 2NF 基礎上，確保非主鍵欄只依賴主鍵，唔可以依賴其他非主鍵欄。
- **Example in Food Order System**:  
  **Table (2NF but not 3NF)**:  

  | Order_ID | Food_Item | Food_Price | Food_Category |
  |----------|-----------|------------|---------------|
  | 001      | Pizza     | $100       | Main Dish     |
  | 002      | Cake      | $50        | Dessert       |

  Primary key: `Order_ID`. Problem: `Food_Category` depends on `Food_Item` (e.g., Pizza is always “Main Dish”), not `Order_ID`, violating 3NF.  
  **Fix for 3NF**: Split into:  
  - **Order_Items Table**:  

    | Order_ID | Food_Item | Food_Price |
    |----------|-----------|------------|
    | 001      | Pizza     | $100       |
    | 002      | Cake      | $50        |

  - **Food Table**:  

    | Food_Item | Food_Category |
    |-----------|---------------|
    | Pizza     | Main Dish     |
    | Cake      | Dessert       |

- **How It Helps Anomalies**:  
  - **Insertion**: Add a new food item and category without an order.  
  - **Update**: Change `Food_Category` once in the Food Table.  
  - **Deletion**: Delete an order without losing `Food_Category`.
- **In Cantonese**: `Food_Category` 只同 `Food_Item` 有關，唔同 `Order_ID` 有關，唔係 3NF。將 `Food_Category` 分開去 Food Table，改資料只改一次，插入同刪除都無問題。

## Fully Normalized Food Order System

To avoid all anomalies, the database should look like this:  
- **Customer Table**:  

  | Customer_ID | Customer_Name | Customer_Phone |
  |-------------|---------------|----------------|
  | C001        | 阿明          | 12345678       |

- **Order Table**:  

  | Order_ID | Customer_ID |
  |----------|-------------|
  | 001      | C001        |

- **Order_Items Table**:  

  | Order_ID | Food_Item | Food_Price |
  |----------|-----------|------------|
  | 001      | Pizza     | $100       |
  | 001      | Burger    | $80        |

- **Food Table**:  

  | Food_Item | Food_Category |
  |-----------|---------------|
  | Pizza     | Main Dish     |
  | Burger    | Main Dish     |

This structure ensures:  
- **No Insertion Anomalies**: Add customers or foods without orders.  
- **No Update Anomalies**: Update customer or food data once in one table.  
- **No Deletion Anomalies**: Delete orders without losing customer or food info.

## Summary of How Normalization Solves Anomalies

- **1NF**: Ensures single values per column, making data easier to manage.  
- **2NF**: Removes partial dependencies (e.g., `Customer_Name` only in Order Table).  
- **3NF**: Removes transitive dependencies (e.g., `Food_Category` only in Food Table).  
Together, they eliminate redundancy and ensure data consistency.

**Study Tip (記憶提示)**:  
- **Insertion Anomaly**: Can’t add what you want (唔可以加想要嘅資料).  
- **Update Anomaly**: Change one thing, mess up others (改一處，亂晒其他).  
- **Deletion Anomaly**: Delete one thing, lose something else (刪一項，連其他都唔見).  
- **Normalization**: 1NF = single values (單一值), 2NF = full key dependency (全主鍵依賴), 3NF = no non-key dependencies (無非主鍵依賴).

## Quick Review Activity

**Question to Test Understanding**:  
In the food order system, if you have a table with `Order_ID`, `Food_Item`, `Food_Price`, and `Restaurant_Name`, why might storing `Restaurant_Name` in this table cause a problem (hint: think about 3NF)? Suggest how you’d fix it by splitting the table.

**Answer this**, and I’ll check your understanding or clarify further. If you want a practice quiz or another example, just let me know!

**Next Question Suggestions**:  
1. “Can you give a practice quiz on anomalies and normalization?”  
2. “How would I normalize a different system, like a library database?”  
3. “Can you explain the normalization steps with a new table example?”
