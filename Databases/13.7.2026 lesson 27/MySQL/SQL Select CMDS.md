# The Logic for Tackling `SELECT` Exercises

When building your SQL queries, ask yourself these **5 questions** in this exact order:

### 1. From which table? ➔ `FROM`

Determine the source of your data.

- `FROM employees`
    
- `FROM products`
    

### 2. Which columns? ➔ `SELECT`

Decide what data you want to pull. If you want "everything", use the asterisk (`*`). If you want specific data, list the column names.

- `SELECT *`
    
- `SELECT firstName, lastName, price`
    

### 3. Is there a condition? ➔ `WHERE`

Filter the data based on specific rules.

- `WHERE price < 100`
    
- `WHERE title LIKE '%Sales%'`
    
- `WHERE stock > 0`
    

### 4. Is there a sorting order? ➔ `ORDER BY`

Decide how the results should be organized (ascending or descending).

- `ORDER BY price DESC`
    
- `ORDER BY name ASC`
    

### 5. Is there a row limit? ➔ `LIMIT` / `OFFSET`

Determine if you only want to see a specific number of results.

- `LIMIT 10`
    
- `LIMIT 10 OFFSET 20`
    

## 📌 The Unchanging Syntax Order

No matter how complex the query gets, the command order is **always**: **`SELECT` ➔ `FROM` ➔ `WHERE` ➔ `ORDER BY` ➔ `LIMIT`**

## 💡 Example Walkthrough

**Prompt:** _"Return the name and price of products cheaper than 100, ordered by price."_

Let's run it through the logic:

- **From which table?** ➔ `products`
    
- **Which columns?** ➔ `name, price`
    
- **Is there a condition?** ➔ `price < 100`
    
- **Is there an order?** ➔ `ORDER BY price`
    
- **Is there a limit?** ➔ No
    

**The Final Query:**

SQL

```
SELECT name, price 
FROM products 
WHERE price < 100 
ORDER BY price;
```