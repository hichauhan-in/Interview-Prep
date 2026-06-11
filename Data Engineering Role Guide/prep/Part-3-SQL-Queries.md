# Part 3 — SQL Queries & Data Retrieval

> Section goal: Master `SELECT` — the most-used command in all of data engineering — including filtering, sorting, limiting, pattern matching, built-in functions, aliases, and user-defined functions.

Covers index items **3** (Module 1, Class 2: SELECT, in-built functions, aliases, LIMIT, ORDER BY, conditional & logical operators, LIKE, UDFs).

---

## 1. The SELECT Statement — Anatomy

`SELECT` reads data. Every query you'll ever write starts here.

```sql
SELECT column1, column2          -- WHAT columns to return
FROM table_name                  -- WHERE the data lives
WHERE condition                  -- FILTER rows
ORDER BY column                  -- SORT results
LIMIT n;                         -- CAP the number of rows
```

### 🔍 Plain-English deep-dive: logical order of execution
SQL is written one way but *executed* in another. Knowing this explains many "why doesn't my alias work in WHERE?" bugs.

```mermaid
flowchart LR
    A[FROM] --> B[WHERE] --> C[GROUP BY] --> D[HAVING] --> E[SELECT] --> F[ORDER BY] --> G[LIMIT]
```

**Analogy:** It's like cooking — you gather ingredients (FROM), discard bad ones (WHERE), group them (GROUP BY), taste-test groups (HAVING), plate the dish (SELECT), arrange it (ORDER BY), then serve a portion (LIMIT). Because SELECT runs *after* WHERE, a column alias defined in SELECT isn't visible in WHERE.

---

## 2. Filtering with WHERE & Operators

### Conditional (comparison) operators
| Operator | Meaning | Example |
|----------|---------|---------|
| `=` | Equal | `salary = 60000` |
| `<>` or `!=` | Not equal | `dept <> 'HR'` |
| `>` `<` `>=` `<=` | Comparisons | `age >= 18` |
| `BETWEEN` | In a range (inclusive) | `salary BETWEEN 50000 AND 80000` |
| `IN` | Matches any in a list | `dept_id IN (1,2,3)` |
| `LIKE` | Pattern match | `name LIKE 'A%'` |
| `IS NULL` | Is empty | `email IS NULL` |

### Logical operators — AND, OR, NOT
```sql
SELECT * FROM employees
WHERE salary > 50000 AND dept_id = 1;          -- both must be true

SELECT * FROM employees
WHERE dept_id = 1 OR dept_id = 2;              -- either true

SELECT * FROM employees
WHERE NOT country = 'India';                   -- negation
```

> 💡 **Precedence trap:** `NOT` > `AND` > `OR`. Use parentheses to be explicit: `WHERE (a OR b) AND c`.

---

## 3. Pattern Matching with LIKE

`LIKE` searches text using **wildcards**:
- `%` = any number of characters (including zero)
- `_` = exactly one character

| Pattern | Matches |
|---------|---------|
| `'A%'` | Starts with A (Asha, Ankit) |
| `'%n'` | Ends with n (Karan, Aman) |
| `'%ar%'` | Contains "ar" (Karan, Sara) |
| `'_a%'` | Second letter is a (Ravi, Karan) |
| `'___'` | Exactly 3 characters |

```sql
SELECT name FROM employees WHERE name LIKE 'A%';      -- names starting with A
SELECT name FROM employees WHERE email LIKE '%@co.com';
```

> 💡 **Analogy:** `%` is a wildcard joker that stands for "anything"; `_` is a single blank tile.

---

## 4. Sorting (ORDER BY) and Limiting (LIMIT)

```sql
SELECT name, salary FROM employees
ORDER BY salary DESC;            -- highest first (ASC is default)

SELECT name, salary FROM employees
ORDER BY dept_id ASC, salary DESC;   -- sort by dept, then salary within dept

SELECT name, salary FROM employees
ORDER BY salary DESC
LIMIT 3;                         -- top 3 earners

SELECT name FROM employees
ORDER BY emp_id
LIMIT 5 OFFSET 10;               -- skip 10, take 5 (pagination)
```

> 💡 **For you:** `LIMIT ... OFFSET` powers "page 2, page 3" in every web app.

---

## 5. Aliases — Renaming for Readability

An **alias** is a temporary nickname for a column or table, set with `AS`.

```sql
SELECT
    first_name AS name,
    salary * 12 AS annual_salary       -- computed column gets a clean name
FROM employees e                        -- 'e' is a table alias
WHERE e.salary > 50000;
```

> 💡 **Analogy:** like calling your colleague "Bob" instead of "Robert Alexander Smith" — shorter, clearer, same person.

---

## 6. Built-in Functions

SQL ships with functions that transform values.

### String functions
| Function | Does | Example → Result |
|----------|------|------------------|
| `UPPER(s)` / `LOWER(s)` | Change case | `UPPER('asha')` → ASHA |
| `LENGTH(s)` | Character count | `LENGTH('Asha')` → 4 |
| `CONCAT(a,b)` | Join strings | `CONCAT('A','B')` → AB |
| `SUBSTRING(s,p,n)` | Extract part | `SUBSTRING('Hello',1,3)` → Hel |
| `TRIM(s)` | Remove spaces | `TRIM('  hi ')` → hi |
| `REPLACE(s,a,b)` | Replace text | `REPLACE('a-b','-','_')` → a_b |

### Numeric functions
| Function | Does |
|----------|------|
| `ROUND(n,d)` | Round to d decimals |
| `CEIL(n)` / `FLOOR(n)` | Round up / down |
| `ABS(n)` | Absolute value |
| `MOD(a,b)` | Remainder |

### Aggregate functions (summarize many rows into one)
| Function | Does |
|----------|------|
| `COUNT(*)` | Number of rows |
| `SUM(col)` | Total |
| `AVG(col)` | Average |
| `MIN(col)` / `MAX(col)` | Smallest / largest |

```sql
SELECT
    COUNT(*)        AS headcount,
    AVG(salary)     AS avg_salary,
    MAX(salary)     AS top_salary
FROM employees;
```

### Date functions
```sql
SELECT NOW();                          -- current datetime
SELECT CURDATE();                      -- current date
SELECT YEAR(hire_date) AS yr FROM employees;
SELECT DATEDIFF(NOW(), hire_date) AS days_employed FROM employees;
```

---

## 7. User-Defined Functions (UDFs)

When built-in functions aren't enough, you create your own reusable function.

```sql
DELIMITER //                            -- change statement terminator temporarily

CREATE FUNCTION annual_salary(monthly DECIMAL(10,2))
RETURNS DECIMAL(12,2)
DETERMINISTIC                            -- same input → same output
BEGIN
    RETURN monthly * 12;
END //

DELIMITER ;                              -- restore normal terminator

-- Use it like any built-in function
SELECT name, annual_salary(salary) AS yearly FROM employees;
```

### 🔍 Plain-English deep-dive: why UDFs?
- **Reusability** — write the logic once, call it everywhere. **Analogy:** saving a recipe instead of re-explaining it each time you cook.
- **Consistency** — every query uses the same calculation, avoiding copy-paste errors.
- **DETERMINISTIC** — tells the engine the function always returns the same result for the same input, allowing optimization/caching.

---

## 🧪 Lab 3 — Query a Sales Dataset

**Goal:** Practice filtering, sorting, functions, and aliases.

```sql
CREATE DATABASE shop_demo;
USE shop_demo;

CREATE TABLE sales (
    sale_id   INT PRIMARY KEY AUTO_INCREMENT,
    product   VARCHAR(50),
    category  VARCHAR(30),
    amount    DECIMAL(10,2),
    qty       INT,
    sale_date DATE
);

INSERT INTO sales (product, category, amount, qty, sale_date) VALUES
('Laptop','Electronics',60000,1,'2026-01-05'),
('Mouse','Electronics',800,3,'2026-01-07'),
('Desk','Furniture',12000,2,'2026-02-10'),
('Chair','Furniture',4500,4,'2026-02-12'),
('Phone','Electronics',35000,1,'2026-03-01'),
('Lamp','Furniture',1500,5,'2026-03-03');
```

### Tasks — try each:
```sql
-- 1. Electronics sales over 1000, highest first
SELECT product, amount FROM sales
WHERE category = 'Electronics' AND amount > 1000
ORDER BY amount DESC;

-- 2. Products whose name contains 'a'
SELECT product FROM sales WHERE product LIKE '%a%';

-- 3. Total revenue and average sale per category
SELECT category,
       SUM(amount) AS total_revenue,
       ROUND(AVG(amount),2) AS avg_sale,
       COUNT(*) AS num_sales
FROM sales
GROUP BY category;

-- 4. Compute revenue per line (amount * qty) with an alias
SELECT product, amount * qty AS line_total FROM sales
ORDER BY line_total DESC;

-- 5. Top 2 biggest single sales
SELECT product, amount FROM sales ORDER BY amount DESC LIMIT 2;

-- 6. Month of each sale
SELECT product, MONTHNAME(sale_date) AS month FROM sales;
```

✅ **Checkpoint:** You filtered with WHERE + operators, matched patterns with LIKE, summarized with aggregates, sorted, limited, and aliased. This is ~70% of everyday SQL work.

---

## ⭐ Likely Interview Questions for This Section

**Q1. "What is the logical order of execution of a SQL query?"**
> *Model answer:* FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT. This is why a column alias from SELECT can't be used in WHERE — WHERE runs first.

**Q2. "Difference between WHERE and HAVING?"**
> *Model answer:* WHERE filters individual rows before grouping; HAVING filters groups after GROUP BY and aggregation. You use aggregate functions like SUM in HAVING, not in WHERE.

**Q3. "What's the difference between `%` and `_` in LIKE?"**
> *Model answer:* `%` matches any number of characters (including none); `_` matches exactly one character.

**Q4. "How do you paginate results?"**
> *Model answer:* With `LIMIT n OFFSET m` — OFFSET skips m rows and LIMIT returns the next n, giving page-by-page navigation.

**Q5. "What is the difference between COUNT(*) and COUNT(column)?"**
> *Model answer:* COUNT(*) counts all rows including those with nulls; COUNT(column) counts only rows where that column is not null.

**Q6. "When would you create a user-defined function?"**
> *Model answer:* When I need reusable, consistent business logic (like a tax or annualization calculation) across many queries, avoiding duplicated expressions and ensuring everyone computes it the same way.

**Q7. "Why doesn't my column alias work in the WHERE clause?"**
> *Model answer:* Because WHERE executes before SELECT in the logical order, the alias doesn't exist yet. Repeat the expression in WHERE or wrap the query in a subquery/CTE.

---

## 🧠 30-Second Memory Hooks
- **Execution order:** FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT.
- **WHERE** = filter rows; **HAVING** = filter groups.
- **LIKE:** `%` = many chars, `_` = one char.
- **LIMIT/OFFSET** = pagination (page 2, page 3).
- **AS** = nickname for columns/tables.
- **Aggregates** = COUNT/SUM/AVG/MIN/MAX (many rows → one answer).
- **UDF** = your own saved recipe.

---

*Next suggested section:* **Part 4 — SQL Advanced: Joins & Subqueries** (single-table queries are easy; real data lives across many tables you must combine).
