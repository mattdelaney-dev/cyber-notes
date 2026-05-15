# Database SQL Basics

## Overview

This room introduced the fundamentals of databases and SQL (Structured Query Language), focusing on how data is stored, organized, and retrieved from database systems.

Topics explored included:
- Tables, rows, and columns
- Basic SQL queries
- Retrieving data
- Managing database information
- Database structure fundamentals

## Key Concepts Learned

### Databases

A database is an organized collection of data designed for efficient storage, retrieval, and management.

Databases are widely used in:
- Websites
- Applications
- Business systems
- Cyber security tools

---

## Tables, Rows, and Columns

Databases organize information into tables.

### Tables
Tables store related data.

Example:
- Users
- Products
- Orders

### Rows
Rows represent individual records within a table.

Example:
- One customer account
- One product entry

### Columns
Columns define the type of data stored.

Examples:
- Name
- Email
- Price
- ID

---

## SQL (Structured Query Language)

SQL is used to interact with databases.

SQL allows users to:
- Retrieve data
- Insert data
- Update records
- Delete information

---

## Basic SQL Queries

### SELECT

The `SELECT` statement retrieves data from a table.

Example:

```sql
SELECT * FROM users;
```

This retrieves all records from the `users` table.

---

### Selecting Specific Columns

Example:

```sql
SELECT name, email FROM users;
```

This retrieves only selected columns from the table.

---

### WHERE Clause

The `WHERE` clause filters results.

Example:

```sql
SELECT * FROM users
WHERE age > 18;
```

This retrieves records matching specific conditions.

---

## Why This Matters

Understanding SQL and databases is important for:
- Web development
- Backend systems
- Cyber security
- Data analysis
- System administration

Databases are commonly targeted in cyber attacks, making database knowledge valuable for both developers and security professionals.

---

## Reflection

This room provided foundational knowledge about databases and SQL queries, including how tables are structured and how information can be retrieved and managed using SQL commands.