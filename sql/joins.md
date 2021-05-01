- INNER JOIN: returns rows when there is a match in both tables.
- LEFT JOIN: returns all rows from the left table, even if there are no matches in the right table.
- RIGHT JOIN: returns all rows from the right table, even if there are no matches in the left table.
- FULL JOIN: It combines the results of both left and right outer joins.
The joined table will contain all records from both the tables and fill in NULLs for missing matches on either side.
- SELF JOIN: is used to join a table to itself as if the table were two tables, temporarily renaming at least one table in the SQL statement.
- CARTESIAN JOIN: returns the Cartesian product of the sets of records from the two or more joined tables.



### Inner join/equi join - Select all records from Table A and Table B, where the join condition is met
```
SELECT col1, col2
FROM Vendors, Products
WHERE Vendors.vend_id = Products.vend_id
```
```
SELECT col1, col2
FROM Vendors INNER JOIN Products
ON Vendors.vend_id = Products.vend_id
```

Both of the syntaxes above are correct. The second one is preferred though

### Self join - makes use of Alias (AS) so that there is no ambiguity
```
SELECT col1, col2 [or you can do c1.col1, c2.*]
FROM Customers AS c1, Customers AS c2
WHERE c1.cust_name = c2.cust_name
  AND c2.cust_contact = 'Jones'
```

### Outer join - Select all records from Table A, along with records from Table B for which the join condition is met (if at all)
```
SELECT col1, col2
FROM Customers LEFT OUTER JOIN Orders
ON Customers.cust_id = Orders.cust_id
```

### Unions - syntax below (they automatically exclude duplicates) if you want duplicates use UNION ALL
```
SELECT column
FROM table
WHERE vend_id = 'DLL01'
UNION
SELECT column
FROM table
WHERE vend_id = 'DLL03'
```
