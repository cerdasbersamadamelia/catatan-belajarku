# Database and SQL Programming - Part 1

## Apa itu Database?

Database adalah sistem untuk menyimpan dan mengelola data secara terstruktur. Seperti lemari arsip digital yang terorganisir dengan baik.

**Kenapa butuh database?**
- Menyimpan data dalam jumlah besar
- Akses data cepat dan efisien
- Data terstruktur dan terorganisir
- Mudah di-query dan dianalisis

## Jenis-Jenis Database

### 1. Relational Database (SQL)
- Data disimpan dalam tabel dengan baris dan kolom
- Menggunakan SQL (Structured Query Language)
- Contoh: PostgreSQL, MySQL, SQL Server

**Karakteristik:**
- Schema yang ketat (harus define struktur dulu)
- ACID compliance (Atomicity, Consistency, Isolation, Durability)
- Cocok untuk data transaksional

### 2. NoSQL Database
- Data disimpan dalam format fleksibel
- Tidak butuh schema yang ketat
- Contoh: MongoDB, Cassandra, Redis

**Jenis-jenis NoSQL:**
- Document-based (MongoDB)
- Key-value (Redis)
- Column-family (Cassandra)
- Graph database (Neo4j)

## PostgreSQL

### Kenapa PostgreSQL?
- Open source dan gratis
- Powerful dan feature-rich
- Community support yang kuat
- Cocok untuk production

### Instalasi
1. Download dari postgresql.org
2. Install dengan default settings
3. Set password untuk user postgres
4. Buka pgAdmin untuk GUI management

### Tools
**pgAdmin:**
- GUI untuk manage PostgreSQL
- Bisa create database, table, query
- Visualisasi data yang mudah

## SQL Basics

### DDL (Data Definition Language)
Commands untuk define struktur database:

#### CREATE DATABASE
```sql
CREATE DATABASE nama_database;
```

#### CREATE TABLE
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    age INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Tipe Data Umum:**
- `INTEGER`: bilangan bulat
- `VARCHAR(n)`: string dengan max length n
- `TEXT`: string tanpa batas
- `TIMESTAMP`: tanggal dan waktu
- `BOOLEAN`: true/false
- `SERIAL`: auto-increment integer

#### ALTER TABLE
```sql
-- Tambah kolom
ALTER TABLE users ADD COLUMN phone VARCHAR(15);

-- Hapus kolom
ALTER TABLE users DROP COLUMN age;

-- Rename kolom
ALTER TABLE users RENAME COLUMN name TO full_name;
```

#### DROP TABLE
```sql
DROP TABLE users;
```

### DML (Data Manipulation Language)
Commands untuk manipulasi data:

#### INSERT
```sql
-- Insert single row
INSERT INTO users (name, email, age) 
VALUES ('John Doe', 'john@example.com', 25);

-- Insert multiple rows
INSERT INTO users (name, email, age) VALUES
    ('Alice', 'alice@example.com', 30),
    ('Bob', 'bob@example.com', 28);
```

#### UPDATE
```sql
-- Update specific row
UPDATE users 
SET age = 26 
WHERE name = 'John Doe';

-- Update multiple columns
UPDATE users 
SET age = 31, email = 'alice.new@example.com' 
WHERE name = 'Alice';
```

#### DELETE
```sql
-- Delete specific row
DELETE FROM users WHERE name = 'Bob';

-- Delete all rows (HATI-HATI!)
DELETE FROM users;
```

### DQL (Data Query Language)
Commands untuk query data:

#### SELECT
```sql
-- Select all columns
SELECT * FROM users;

-- Select specific columns
SELECT name, email FROM users;

-- Select with condition
SELECT * FROM users WHERE age > 25;

-- Select with multiple conditions
SELECT * FROM users 
WHERE age > 25 AND email LIKE '%example.com';
```

#### WHERE Clause
```sql
-- Equal
SELECT * FROM users WHERE age = 25;

-- Not equal
SELECT * FROM users WHERE age != 25;
SELECT * FROM users WHERE age <> 25;

-- Greater than
SELECT * FROM users WHERE age > 25;

-- Less than or equal
SELECT * FROM users WHERE age <= 30;

-- BETWEEN
SELECT * FROM users WHERE age BETWEEN 25 AND 30;

-- IN
SELECT * FROM users WHERE age IN (25, 28, 30);

-- LIKE (pattern matching)
SELECT * FROM users WHERE name LIKE 'J%';  -- starts with J
SELECT * FROM users WHERE email LIKE '%@gmail.com';  -- ends with @gmail.com
```

#### ORDER BY
```sql
-- Ascending (default)
SELECT * FROM users ORDER BY age;
SELECT * FROM users ORDER BY age ASC;

-- Descending
SELECT * FROM users ORDER BY age DESC;

-- Multiple columns
SELECT * FROM users ORDER BY age DESC, name ASC;
```

#### LIMIT dan OFFSET
```sql
-- Get first 10 rows
SELECT * FROM users LIMIT 10;

-- Skip first 10, get next 10 (pagination)
SELECT * FROM users LIMIT 10 OFFSET 10;
```

## Aggregate Functions

### COUNT
```sql
-- Count all rows
SELECT COUNT(*) FROM users;

-- Count specific column (exclude NULL)
SELECT COUNT(email) FROM users;

-- Count distinct values
SELECT COUNT(DISTINCT age) FROM users;
```

### SUM, AVG, MIN, MAX
```sql
-- Sum
SELECT SUM(age) FROM users;

-- Average
SELECT AVG(age) FROM users;

-- Minimum
SELECT MIN(age) FROM users;

-- Maximum
SELECT MAX(age) FROM users;
```

### GROUP BY
```sql
-- Count users by age
SELECT age, COUNT(*) as total 
FROM users 
GROUP BY age;

-- Average age by city (if ada column city)
SELECT city, AVG(age) as avg_age 
FROM users 
GROUP BY city;
```

### HAVING
```sql
-- Filter after grouping (WHERE tidak bisa digunakan dengan aggregate)
SELECT age, COUNT(*) as total 
FROM users 
GROUP BY age 
HAVING COUNT(*) > 5;
```

## JOIN Operations

### INNER JOIN
Mengambil data yang match di kedua table:
```sql
SELECT users.name, orders.order_date 
FROM users 
INNER JOIN orders ON users.id = orders.user_id;
```

### LEFT JOIN
Mengambil semua data dari table kiri, dan data yang match dari table kanan:
```sql
SELECT users.name, orders.order_date 
FROM users 
LEFT JOIN orders ON users.id = orders.user_id;
```

### RIGHT JOIN
Mengambil semua data dari table kanan, dan data yang match dari table kiri:
```sql
SELECT users.name, orders.order_date 
FROM users 
RIGHT JOIN orders ON users.id = orders.user_id;
```

### FULL OUTER JOIN
Mengambil semua data dari kedua table:
```sql
SELECT users.name, orders.order_date 
FROM users 
FULL OUTER JOIN orders ON users.id = orders.user_id;
```

## Best Practices

### 1. Naming Convention
- Gunakan lowercase
- Gunakan underscore untuk separator (snake_case)
- Hindari reserved keywords
- Nama harus deskriptif

### 2. Indexing
- Buat index pada kolom yang sering di-query
- Primary key otomatis punya index
- Foreign key sebaiknya di-index

```sql
CREATE INDEX idx_users_email ON users(email);
```

### 3. Constraints
**PRIMARY KEY:**
- Unique identifier untuk setiap row
- Tidak bisa NULL
- Satu table satu primary key

**FOREIGN KEY:**
- Reference ke primary key di table lain
- Maintain referential integrity

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    order_date TIMESTAMP
);
```

**UNIQUE:**
- Memastikan nilai unik dalam kolom
```sql
ALTER TABLE users ADD CONSTRAINT unique_email UNIQUE (email);
```

**NOT NULL:**
- Kolom tidak boleh kosong
```sql
ALTER TABLE users ALTER COLUMN email SET NOT NULL;
```

### 4. Transactions
Untuk menjaga consistency:
```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;

-- Jika ada error, bisa rollback
ROLLBACK;
```

## Tips Praktis

### 1. Query Optimization
- Gunakan `EXPLAIN` untuk analyze query performance
- Avoid `SELECT *` kalau tidak perlu semua kolom
- Gunakan index pada kolom yang sering di-filter

### 2. Security
- Gunakan parameterized queries untuk avoid SQL injection
- Jangan simpan password dalam plain text (gunakan hashing)
- Set proper permissions untuk users

### 3. Backup
- Backup database secara rutin
- Test restore process
- Simpan backup di tempat yang aman

## Key Takeaways

1. **PostgreSQL adalah pilihan solid** untuk relational database
2. **DDL untuk define struktur**, DML untuk manipulasi data, DQL untuk query
3. **JOIN operations** untuk menggabungkan data dari multiple tables
4. **Aggregate functions dan GROUP BY** untuk analisis data
5. **Indexing penting** untuk performance
6. **Constraints** memastikan data integrity
7. **Best practices** seperti naming convention dan security harus diikuti
