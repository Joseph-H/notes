### Principles of relational databases
Relational databases store data in tables which have relations to other tables
Nearly all of them use SQL for data definition and manipulation

### Terms:
- Primary Key - is an optional special database column or columns used to identify a database record. Needs to be unique
- Surrogate Key - is a type of Primary Key which uses a unique generated value. Should not have business value and should never change. Considered a best practice in database design
- Foreign key constraints - you have a parent table and a child table. Child table can’t delete a row if that foreign key exists in the parent table.
- One to one - record in table A matches exactly one record in table B
- One to many - record in table A matches many in Table B, but table B only matches only one record in table A
- Many to Many - record in table A matches many in Table B and vice versa
- Transactions - These are happening automatically in the database. ACID happens in a transaction. In a transaction you can begin it, then do things, you can rollback or save a certain point, and then finally you can commit to finish the transaction.

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