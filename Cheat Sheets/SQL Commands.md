# SQL Commands Cheat Sheet

Essential SQL commands for database operations.

## 🗄️ Database Operations

```sql
-- Create database
CREATE DATABASE mydb;
CREATE DATABASE IF NOT EXISTS mydb;

-- Use database
USE mydb;

-- Drop database
DROP DATABASE mydb;
DROP DATABASE IF EXISTS mydb;

-- Show databases
SHOW DATABASES;

-- Backup database (MySQL)
mysqldump -u username -p database_name > backup.sql

-- Restore database
mysql -u username -p database_name < backup.sql
```

## 📋 Table Operations

### Create Table
```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- With constraints
CREATE TABLE orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    total DECIMAL(10, 2) NOT NULL,
    status VARCHAR(20) DEFAULT 'pending',
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

### Alter Table
```sql
-- Add column
ALTER TABLE users ADD COLUMN age INT;
ALTER TABLE users ADD COLUMN phone VARCHAR(20) AFTER email;

-- Modify column
ALTER TABLE users MODIFY COLUMN age TINYINT;
ALTER TABLE users CHANGE age user_age INT;

-- Drop column
ALTER TABLE users DROP COLUMN age;

-- Add constraint
ALTER TABLE users ADD UNIQUE (email);
ALTER TABLE users ADD CONSTRAINT fk_user 
    FOREIGN KEY (user_id) REFERENCES users(id);

-- Drop constraint
ALTER TABLE users DROP FOREIGN KEY fk_user;
ALTER TABLE users DROP INDEX email;

-- Rename table
ALTER TABLE users RENAME TO customers;
RENAME TABLE users TO customers;
```

### Drop Table
```sql
DROP TABLE users;
DROP TABLE IF EXISTS users;

-- Truncate (remove all data, keep structure)
TRUNCATE TABLE users;
```

### Show Tables
```sql
SHOW TABLES;
DESCRIBE users;
DESC users;
SHOW CREATE TABLE users;
SHOW COLUMNS FROM users;
```

## 📝 CRUD Operations

### INSERT
```sql
-- Insert single row
INSERT INTO users (username, email, password) 
VALUES ('john_doe', 'john@email.com', 'hash123');

-- Insert multiple rows
INSERT INTO users (username, email, password) VALUES
    ('user1', 'user1@email.com', 'hash1'),
    ('user2', 'user2@email.com', 'hash2'),
    ('user3', 'user3@email.com', 'hash3');

-- Insert from another table
INSERT INTO users_backup 
SELECT * FROM users WHERE created_at < '2023-01-01';
```

### SELECT
```sql
-- Select all columns
SELECT * FROM users;

-- Select specific columns
SELECT username, email FROM users;

-- Select with alias
SELECT username AS name, email AS contact FROM users;

-- Select distinct
SELECT DISTINCT status FROM orders;

-- Select with LIMIT
SELECT * FROM users LIMIT 10;
SELECT * FROM users LIMIT 10 OFFSET 20;  -- Skip 20, get next 10

-- Select with WHERE
SELECT * FROM users WHERE age > 18;
SELECT * FROM users WHERE username = 'john_doe';
SELECT * FROM users WHERE created_at > '2024-01-01';

-- Multiple conditions
SELECT * FROM users WHERE age > 18 AND status = 'active';
SELECT * FROM users WHERE age < 18 OR age > 65;
SELECT * FROM users WHERE NOT status = 'banned';

-- IN operator
SELECT * FROM users WHERE id IN (1, 2, 3, 4);
SELECT * FROM users WHERE status IN ('active', 'pending');

-- BETWEEN
SELECT * FROM orders WHERE total BETWEEN 100 AND 500;
SELECT * FROM users WHERE created_at BETWEEN '2024-01-01' AND '2024-12-31';

-- LIKE (pattern matching)
SELECT * FROM users WHERE email LIKE '%@gmail.com';
SELECT * FROM users WHERE username LIKE 'john%';
SELECT * FROM users WHERE username LIKE '%_doe';
-- % matches any characters, _ matches single character

-- IS NULL / IS NOT NULL
SELECT * FROM users WHERE phone IS NULL;
SELECT * FROM users WHERE phone IS NOT NULL;

-- ORDER BY
SELECT * FROM users ORDER BY created_at DESC;
SELECT * FROM users ORDER BY age ASC, username DESC;

-- GROUP BY
SELECT status, COUNT(*) as count FROM users GROUP BY status;
SELECT DATE(created_at) as date, COUNT(*) FROM users GROUP BY date;

-- HAVING (filter after GROUP BY)
SELECT status, COUNT(*) as count 
FROM users 
GROUP BY status 
HAVING count > 10;
```

### UPDATE
```sql
-- Update single column
UPDATE users SET email = 'newemail@example.com' WHERE id = 1;

-- Update multiple columns
UPDATE users 
SET email = 'new@email.com', updated_at = NOW() 
WHERE id = 1;

-- Update with calculation
UPDATE products SET price = price * 1.1 WHERE category = 'electronics';

-- Update all rows (be careful!)
UPDATE users SET status = 'active';
```

### DELETE
```sql
-- Delete specific rows
DELETE FROM users WHERE id = 1;
DELETE FROM users WHERE status = 'inactive';

-- Delete with limit
DELETE FROM users WHERE status = 'temp' LIMIT 100;

-- Delete all rows (be careful!)
DELETE FROM users;
```

## 🔗 Joins

### INNER JOIN
```sql
-- Returns matching rows from both tables
SELECT users.username, orders.total 
FROM users 
INNER JOIN orders ON users.id = orders.user_id;

-- Multiple joins
SELECT u.username, o.total, p.product_name
FROM users u
INNER JOIN orders o ON u.id = o.user_id
INNER JOIN order_items oi ON o.id = oi.order_id
INNER JOIN products p ON oi.product_id = p.id;
```

### LEFT JOIN
```sql
-- Returns all rows from left table, matching rows from right
SELECT users.username, orders.total 
FROM users 
LEFT JOIN orders ON users.id = orders.user_id;

-- Find users with no orders
SELECT users.username 
FROM users 
LEFT JOIN orders ON users.id = orders.user_id 
WHERE orders.id IS NULL;
```

### RIGHT JOIN
```sql
-- Returns all rows from right table, matching rows from left
SELECT users.username, orders.total 
FROM users 
RIGHT JOIN orders ON users.id = orders.user_id;
```

### FULL OUTER JOIN
```sql
-- Returns all rows from both tables (MySQL doesn't support directly)
-- Emulate with UNION
SELECT users.username, orders.total 
FROM users 
LEFT JOIN orders ON users.id = orders.user_id
UNION
SELECT users.username, orders.total 
FROM users 
RIGHT JOIN orders ON users.id = orders.user_id;
```

### CROSS JOIN
```sql
-- Cartesian product (all combinations)
SELECT * FROM colors CROSS JOIN sizes;
```

### SELF JOIN
```sql
-- Join table to itself
SELECT e1.name as Employee, e2.name as Manager
FROM employees e1
LEFT JOIN employees e2 ON e1.manager_id = e2.id;
```

## 🔢 Aggregate Functions

```sql
-- COUNT
SELECT COUNT(*) FROM users;
SELECT COUNT(DISTINCT status) FROM users;

-- SUM
SELECT SUM(total) FROM orders;
SELECT SUM(total) FROM orders WHERE status = 'completed';

-- AVG
SELECT AVG(age) FROM users;
SELECT AVG(total) as average_order FROM orders;

-- MIN / MAX
SELECT MIN(price) as cheapest, MAX(price) as most_expensive FROM products;

-- GROUP_CONCAT (MySQL)
SELECT user_id, GROUP_CONCAT(product_name) as products
FROM orders
GROUP BY user_id;
```

## 🔍 Subqueries

```sql
-- Subquery in WHERE
SELECT * FROM users 
WHERE id IN (SELECT user_id FROM orders WHERE total > 1000);

-- Subquery in SELECT
SELECT username, 
       (SELECT COUNT(*) FROM orders WHERE orders.user_id = users.id) as order_count
FROM users;

-- Subquery in FROM
SELECT avg_age.average
FROM (SELECT AVG(age) as average FROM users) as avg_age;

-- EXISTS
SELECT * FROM users u
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.user_id = u.id);

-- NOT EXISTS
SELECT * FROM users u
WHERE NOT EXISTS (SELECT 1 FROM orders o WHERE o.user_id = u.id);
```

## 🎯 Advanced Queries

### UNION
```sql
-- Combine results from multiple queries
SELECT name FROM customers
UNION
SELECT name FROM suppliers;

-- UNION ALL (includes duplicates)
SELECT name FROM customers
UNION ALL
SELECT name FROM suppliers;
```

### CASE Statement
```sql
SELECT username,
       CASE 
           WHEN age < 18 THEN 'Minor'
           WHEN age >= 18 AND age < 65 THEN 'Adult'
           ELSE 'Senior'
       END as age_group
FROM users;
```

### Window Functions
```sql
-- ROW_NUMBER
SELECT username, 
       ROW_NUMBER() OVER (ORDER BY created_at) as row_num
FROM users;

-- RANK
SELECT username, score,
       RANK() OVER (ORDER BY score DESC) as rank
FROM users;

-- PARTITION BY
SELECT username, department,
       AVG(salary) OVER (PARTITION BY department) as dept_avg
FROM employees;
```

### Common Table Expressions (CTE)
```sql
WITH active_users AS (
    SELECT * FROM users WHERE status = 'active'
)
SELECT * FROM active_users WHERE age > 18;

-- Recursive CTE
WITH RECURSIVE numbers AS (
    SELECT 1 as n
    UNION ALL
    SELECT n + 1 FROM numbers WHERE n < 10
)
SELECT * FROM numbers;
```

## 🔐 Indexes

```sql
-- Create index
CREATE INDEX idx_email ON users(email);
CREATE INDEX idx_name_email ON users(username, email);
CREATE UNIQUE INDEX idx_unique_email ON users(email);

-- Drop index
DROP INDEX idx_email ON users;
ALTER TABLE users DROP INDEX idx_email;

-- Show indexes
SHOW INDEX FROM users;
```

## 👥 Users and Permissions

```sql
-- Create user
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
CREATE USER 'username'@'%' IDENTIFIED BY 'password';  -- Any host

-- Grant privileges
GRANT ALL PRIVILEGES ON mydb.* TO 'username'@'localhost';
GRANT SELECT, INSERT ON mydb.users TO 'username'@'localhost';
GRANT SELECT ON mydb.* TO 'readonly'@'%';

-- Revoke privileges
REVOKE INSERT ON mydb.users FROM 'username'@'localhost';

-- Show grants
SHOW GRANTS FOR 'username'@'localhost';

-- Drop user
DROP USER 'username'@'localhost';

-- Change password
ALTER USER 'username'@'localhost' IDENTIFIED BY 'newpassword';

-- Flush privileges
FLUSH PRIVILEGES;
```

## 🔄 Transactions

```sql
-- Start transaction
START TRANSACTION;
BEGIN;

-- Commit
COMMIT;

-- Rollback
ROLLBACK;

-- Example
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;

-- Savepoint
START TRANSACTION;
INSERT INTO users VALUES (1, 'user1');
SAVEPOINT sp1;
INSERT INTO users VALUES (2, 'user2');
ROLLBACK TO sp1;  -- Rollback to savepoint
COMMIT;
```

## 📊 Data Types

### Numeric
```sql
INT, TINYINT, SMALLINT, MEDIUMINT, BIGINT
DECIMAL(10,2), NUMERIC(10,2)
FLOAT, DOUBLE
```

### String
```sql
CHAR(50)          -- Fixed length
VARCHAR(255)      -- Variable length
TEXT              -- Long text
MEDIUMTEXT
LONGTEXT
ENUM('val1', 'val2')
```

### Date and Time
```sql
DATE              -- YYYY-MM-DD
TIME              -- HH:MM:SS
DATETIME          -- YYYY-MM-DD HH:MM:SS
TIMESTAMP         -- Unix timestamp
YEAR              -- YYYY
```

### Other
```sql
BLOB              -- Binary data
BOOLEAN           -- TRUE/FALSE (TINYINT(1))
JSON              -- JSON data (MySQL 5.7+)
```

## 💡 Useful Functions

### String Functions
```sql
CONCAT('Hello', ' ', 'World')                    -- Concatenate
UPPER('hello'), LOWER('HELLO')                   -- Case conversion
LENGTH('string'), CHAR_LENGTH('string')          -- Length
SUBSTRING('hello', 1, 3)                         -- Substring
TRIM('  text  '), LTRIM(), RTRIM()              -- Trim spaces
REPLACE('text', 'old', 'new')                    -- Replace
```

### Date Functions
```sql
NOW()                                            -- Current datetime
CURDATE()                                        -- Current date
CURTIME()                                        -- Current time
DATE_ADD(NOW(), INTERVAL 1 DAY)                 -- Add time
DATE_SUB(NOW(), INTERVAL 1 MONTH)               -- Subtract time
DATEDIFF(date1, date2)                          -- Date difference
DATE_FORMAT(NOW(), '%Y-%m-%d')                  -- Format date
```

### Numeric Functions
```sql
ROUND(3.14159, 2)                               -- Round
CEIL(3.1), FLOOR(3.9)                          -- Ceiling/Floor
ABS(-5)                                         -- Absolute value
POW(2, 3)                                       -- Power
RAND()                                          -- Random number
```

## Related Topics

- [[Development/Development Index|Development]]
- [[Computer Science/Database Fundamentals|Database Fundamentals]]

---

#sql #database #cheatsheet #reference
