### Principles of relational databases
Relational databases store data in tables which have relations to other tables
Nearly all of them use SQL for data definition and manipulation

### Terms:
- Primary Key - is an optional special database column or columns used to identify a database record. Needs to be unique
- Surrogate Key - is a type of Primary Key which uses a unique generated value. Should not have business value and should never change. Considered a best practice in database design
- Foreign key constraints - you have a parent table and a child table. FK in child table maps to a PK in parent table.
- One to one - record in table A matches exactly one record in table B
- One to many - record in table A matches many in Table B, but table B only matches only one record in table A
- Many to Many - record in table A matches many in Table B and vice versa
- Transactions - These are happening automatically in the database. ACID happens in a transaction. In a transaction you can begin it, then do things, you can rollback or save a certain point, and then finally you can commit to finish the transaction.

### Normalization
- First Normal Form - Fields must be atomic, ie. first name, last name in their own columns. Can't have repeating values like item1, item2
- Second Normal Form - All non key columns must depend on the primary key. Meaning each table stores data about one subject like a customer
- Third Normal Form - All non key columns must be mutually independent. This means we don't store calculations. Instead, we use lookup tables.

### Denormalization
Used to increase performance. For example - summarizing invoice data in an accounting table
- downside is that you need to keep summary data current
- you need to document why you denormalize
- ensure that the denormalization increases performance

```
SELECT
  *
FROM
  employees
WHERE
  first_name IN (
    SELECT
      first_name
    FROM
      employees
    WHERE
      first_name LIKE '%E'
)
```

```
SELECT prod_id,
   quantity,
   item_price,
   quantity*item_price AS expanded_price
FROM OrderItems
WHERE order_num = 20008
```


### Inserts
```
INSERT INTO Customers(cust_id, name)
VALUES (1, 'Bob')
```

### Select Insert - will insert all the rows retrieved into the Customers table
```
INSERT INTO Customers(name)
SELECT name
From CustomerOld
```

### Copy to another table
```
SELECT *
INTO CustomerCopy
FROM Customers
```

### Update
```
UPDATE Customers
SET cust_email = 'name@gmail.com'
WHERE cust_id = '1122'
```

#### To update a value in a row
```
UPDATE Customers
SET cust_email = NULL
Where cust_id = '1122'
```

### Delete
```
DELETE FROM Customers
WHERE cust_id = '1122'
```

### DDL Data Definition Language
```
CREATE TABLE tasks (
  task_name VARCHAR(255),
  complete CHAR(1)
);
```

### DML Data Manipulation Language
```
INSERT INTO tasks VALUES ('Study SQL', 'Y');
```

Aggregate functions
COUNT
- AVG
- SUM
- MAX
- MIN