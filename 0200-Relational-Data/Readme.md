# Relational Databases and SQL

## 1. Relational Databases

A **relational database** stores structured data in **tables**. This model provides a standardized, efficient, and flexible way to organize, access, and query information.

Relational databases are widely used for:

- Customer management
- Inventory systems
- Ecommerce transactions
- Financial systems
- Business applications
- Mission-critical enterprise data

### Tables, Rows, and Columns

A **table** represents an entity, such as:

- `Customer`
- `Product`
- `Order`
- `LineItem`

Each table contains:

- **Rows**: individual instances of an entity.
- **Columns**: attributes describing the entity.
- **Data types**: define what kind of information a column can contain.

Example:

| CustomerID | FirstName | LastName | Email |
|---|---|---|---|
| 1 | Jane | Smith | jane@example.com |
| 2 | Bob | Jones | bob@example.com |

Typical data types include:

| Type | Example |
|---|---|
| Integer | `INT` |
| Decimal | `DECIMAL` |
| Text | `VARCHAR` |
| Date/Time | `DATE`, `DATETIME` |

Columns can also allow **NULL**, which represents a missing or unknown value.

---

# 2. Normalization

**Normalization** is the process of designing a relational database to:

- Reduce duplicated data.
- Improve data integrity.
- Organize data into logical entities.
- Make data easier to maintain.

A practical approach to normalization is:

1. Store each **entity** in its own table.
2. Store each discrete **attribute** in its own column.
3. Identify each row using a **primary key**.
4. Connect related tables using **foreign keys**.

Instead of storing everything in one table:

```text {preview="false"}
OrderNo | CustomerName | CustomerAddress | ProductName | Price | Quantity
```

The information can be separated into:

```text {preview="false"}
Customer
Product
Order
LineItem
```

This prevents information such as customer addresses or product prices from being unnecessarily duplicated.

---

# 3. Primary Keys and Foreign Keys

## Primary Key

A **primary key (PK)** uniquely identifies each row in a table.

Example:

```sql {preview="false"}
Customer
--------
CustomerID  PK
FirstName
LastName
Address
City
```

`CustomerID` uniquely identifies each customer.

## Foreign Key

A **foreign key (FK)** references the primary key of another table.

Example:

```text {preview="false"}
Customer
--------
CustomerID PK

Order
-----
OrderID    PK
CustomerID FK
```

The foreign key creates the relationship:

```text {preview="false"}
Customer 1 ---- N Order
```

One customer can have multiple orders.

An RDBMS can enforce **referential integrity**, preventing relationships with nonexistent records. For example, an order cannot reference a customer that doesn't exist.

## Composite Key

A key can also consist of multiple columns.

For example:

```text {preview="false"}
LineItem
--------
OrderNo   PK
ItemNo    PK
ProductID
Quantity
```

The combination of `OrderNo` and `ItemNo` uniquely identifies each line item.

---

# 4. SQL

**SQL (Structured Query Language)** is the standard language used to interact with relational databases.

Common systems that support SQL include:

- Microsoft SQL Server
- Azure SQL Database
- Azure SQL Managed Instance
- SQL Server on Azure Virtual Machines
- PostgreSQL
- MySQL
- Oracle

Different database systems can implement their own SQL dialect.

Examples:

| Platform | SQL dialect |
|---|---|
| SQL Server / Azure SQL | T-SQL |
| PostgreSQL | PL/pgSQL |
| Oracle | PL/SQL |

Microsoft SQL Server and Azure SQL services based on the SQL Server engine use **Transact-SQL (T-SQL)**.

---

# 5. SQL Statement Categories

SQL statements can be grouped into three major categories:

```text {preview="false"}
SQL
├── DDL - Data Definition Language
├── DCL - Data Control Language
└── DML - Data Manipulation Language
```

---

## 5.1 DDL - Data Definition Language

DDL manages the **structure of the database**.

Main commands:

| Command | Purpose |
|---|---|
| `CREATE` | Create an object |
| `ALTER` | Modify an object |
| `DROP` | Remove an object |
| `RENAME` | Rename an object |

Example:

```sql {preview="false"}
CREATE TABLE Product
(
    ID INT PRIMARY KEY,
    Name VARCHAR(20) NOT NULL,
    Price DECIMAL NULL
);
```

### NULL vs NOT NULL

`NOT NULL` means the column must contain a value.

```sql {preview="false"}
Name VARCHAR(20) NOT NULL
```

If `NOT NULL` isn't specified, the column can generally contain `NULL`.

---

## 5.2 DCL - Data Control Language

DCL controls **permissions and access** to database objects.

Main commands:

| Command | Purpose |
|---|---|
| `GRANT` | Grant permission |
| `DENY` | Deny permission |
| `REVOKE` | Remove previously granted permission |

Example:

```sql {preview="false"}
GRANT SELECT, INSERT, UPDATE
ON Product
TO user1;
```

This allows `user1` to read, insert, and update data in the `Product` table.

---

## 5.3 DML - Data Manipulation Language

DML is used to work with the **data stored in tables**.

The four main commands are:

| Command | Purpose |
|---|---|
| `SELECT` | Read data |
| `INSERT` | Insert data |
| `UPDATE` | Modify data |
| `DELETE` | Delete data |

---

# 6. SELECT

Retrieve data from a table:

```sql {preview="false"}
SELECT *
FROM Customer;
```

Filter rows using `WHERE`:

```sql {preview="false"}
SELECT *
FROM Customer
WHERE City = 'Seattle';
```

Retrieve specific columns:

```sql {preview="false"}
SELECT FirstName, LastName, Address, City
FROM Customer
WHERE City = 'Seattle';
```

---

# 7. ORDER BY

`ORDER BY` sorts query results.

```sql {preview="false"}
SELECT FirstName, LastName, Address, City
FROM Customer
WHERE City = 'Seattle'
ORDER BY LastName;
```

Without `ORDER BY`, the database doesn't guarantee a particular row order.

---

# 8. JOIN

A `JOIN` combines related data from multiple tables.

Typically, the relationship connects:

```text {preview="false"}
Primary Key <--> Foreign Key
```

Example:

```sql {preview="false"}
SELECT
    o.OrderNo,
    o.OrderDate,
    c.Address,
    c.City
FROM [Order] AS o
JOIN Customer AS c
    ON o.Customer = c.ID;
```

Here:

- `o` is an alias for `Order`.
- `c` is an alias for `Customer`.
- The `JOIN` links the customer associated with each order.

---

# 9. INSERT

Insert a new row:

```sql {preview="false"}
INSERT INTO Product (ID, Name, Price)
VALUES (99, 'Drill', 4.99);
```

---

# 10. UPDATE

Modify existing rows:

```sql {preview="false"}
UPDATE Customer
SET Address = '123 High St.'
WHERE ID = 1;
```

> ⚠️ **Warning:** Without a `WHERE` clause, `UPDATE` can modify every row in the table.

---

# 11. DELETE

Delete rows:

```sql {preview="false"}
DELETE FROM Product
WHERE ID = 162;
```

> ⚠️ **Warning:** Without a `WHERE` clause, `DELETE` removes every row from the table.

---

# 12. Database Objects

Relational databases contain more than tables. Important database objects include:

- **Views**
- **Stored Procedures**
- **Indexes**

---

## 12.1 Views

A **view** is a virtual table based on the result of a `SELECT` query.

It provides a reusable way to access information from one or more underlying tables.

Example:

```sql {preview="false"}
CREATE VIEW Deliveries
AS
SELECT
    o.OrderNo,
    o.OrderDate,
    c.FirstName,
    c.LastName,
    c.Address,
    c.City
FROM [Order] AS o
JOIN Customer AS c
    ON o.Customer = c.ID;
```

The view can then be queried like a table:

```sql {preview="false"}
SELECT OrderNo, OrderDate, LastName, Address
FROM Deliveries
WHERE City = 'Seattle';
```

---

## 12.2 Stored Procedures

A **stored procedure** contains SQL statements stored in the database that can be executed when needed.

Stored procedures can accept parameters and encapsulate database logic.

Example:

```sql {preview="false"}
CREATE PROCEDURE RenameProduct
    @ProductID INT,
    @NewName VARCHAR(20)
AS
UPDATE Product
SET Name = @NewName
WHERE ID = @ProductID;
```

Execute it with:

```sql {preview="false"}
EXEC RenameProduct 201, 'Spanner';
```

Stored procedures are useful for implementing reusable database operations.

---

## 12.3 Indexes

An **index** improves the speed of data retrieval.

It works similarly to the index of a book: instead of scanning every row, the database can use the index to locate relevant rows more efficiently.

Example:

```sql {preview="false"}
CREATE INDEX idx_ProductName
ON Product(Name);
```

Queries that filter using `Name` may then be able to retrieve rows more efficiently.

### Index trade-off

Indexes improve **read/query performance**, but they aren't free.

They require:

- Additional storage.
- Additional work during `INSERT`.
- Additional work during `UPDATE`.
- Additional work during `DELETE`.

Therefore, database designers must balance:

```text {preview="false"}
More indexes
    ↓
Faster searches
    ↓
More storage + slower data modifications
```

---

# Key Concepts

| Concept | Purpose |
|---|---|
| **Table** | Stores instances of an entity |
| **Row** | Represents one entity instance |
| **Column** | Represents an entity attribute |
| **Primary Key** | Uniquely identifies a row |
| **Foreign Key** | Creates relationships between tables |
| **Composite Key** | Identifies a row using multiple columns |
| **Normalization** | Reduces duplication and improves integrity |
| **DDL** | Defines database structures |
| **DCL** | Controls permissions |
| **DML** | Reads and modifies data |
| **View** | Virtual table based on a query |
| **Stored Procedure** | Encapsulates reusable SQL logic |
| **Index** | Improves data retrieval performance |

## Quick SQL Reference

| Task | SQL |
|---|---|
| Create table | `CREATE TABLE` |
| Modify table | `ALTER TABLE` |
| Delete object | `DROP` |
| Query data | `SELECT` |
| Add data | `INSERT` |
| Modify data | `UPDATE` |
| Delete data | `DELETE` |
| Filter rows | `WHERE` |
| Sort results | `ORDER BY` |
| Combine tables | `JOIN` |
| Grant permissions | `GRANT` |
| Deny permissions | `DENY` |
| Remove permissions | `REVOKE` |
| Create a view | `CREATE VIEW` |
| Create procedure | `CREATE PROCEDURE` |
| Create index | `CREATE INDEX` |
