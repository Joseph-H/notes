### Create table with constraints and indexes
```
-- temporary tables start with # ie: #tblCustomers
CREATE TABLE tblCustomers(
    CustomerId INT PRIMARY KEY NOT NULL IDENTITY (1, 1),
    CompanyName NVARCHAR(50),
    Address NVARCHAR(50),
    City NVARCHAR(50),
    -- constraint syntax is Constraint Const_Name contstraint_type (value)
    State NCHAR(2) CONSTRAINT DF_State DEFAULT 'CA' CHECK (State IN('CA', 'AZ', 'OR', 'WA')),
    ZipCode NVARCHAR(10),
    CreditLimit MONEY CONSTRAINT DF_CreditLimit DEFAULT 5000,
    IntroDate DATE CONSTRAINT DF_IntroDate DEFAULT getdate() CONSTRAINT chk_IntroDate CHECK (IntroDate <= GetDate()),
);

CREATE TABLE tblOrders(
    OrderId INT PRIMARY KEY NOT NULL IDENTITY(1, 1),
    CustomerId INT,
    OrderDate DATE NOT NULL,
    ShipVia INT NOT NULL
);
```

### Add index
```
CREATE INDEX ix_tblCustomers_CreditLimit ON tblCustomers(CreditLimit ASC);
CREATE INDEX ix_tblCustomers_IntroDate ON tblCustomers(IntroDate ASC);
```

### Other ways of adding constraints
```
ALTER TABLE tblCustomers
    ADD CONSTRAINT chk_tblCustomers_CreditLimit CHECK (CreditLimit BETWEEN 0 AND 10000);

ALTER TABLE tblOrders
    ADD CONSTRAINT FK_tblOrders_tblCustomers FOREIGN KEY (CustomerId)
        REFERENCES tblCustomers (CustomerId)
        ON DELETE NO ACTION
        ON UPDATE NO ACTION
;
```

### Select quirks
```
-- place column inside "[ ]" if there is a "space" in the column name
SELECT [First Name], [Last Name], age from Customers;
SELECT TOP 5 * from tblCustomers;
```

### Insert into
```
Select Into statement
SELECT cols
INTO anotherTable FROM sourceTable
WHERE criteria
```

### Deleting
```
-- removes all rows from a table, keeps the table structure in tact
TRUNCATE TABLE myTable
```

### Views
Limitations:
- A view can be created only in the current database
- Can have a max of 1024 columns
```
CREATE VIEW myView AS SELECT * FROM sourceTable

CREATE VIEW myView AS
SELECT cols
FROM sourceTable AS s JOIN otherTable as o
ON s.something = o.something
```

### Stored Procedures
SQL statements that are compiled and saved to the database. SPs can have write access to the data where users only have read
```
CREATE PROCEDURE procMyProcedure
    -- Add the parameters for the stored procedure here
    @InputId int
    @Locale nvarchar(35) = NULL OUTPUT -- this is an output variable youd use it to return stuff
AS
BEGIN
    -- SET NOCOUNT ON added to prevent extra result sets from
    -- interfering with SELECT statements.
    SET NOCOUNT ON;

    DECLARE @UpperFirstName nvarchar(35);

    -- Insert statements for procedure here
    -- Using the variable and setting a value to it
    SELECT @UpperFirstName = Upper([First Name]) From Customers
    WHERE Id = @InputId
    -- returning the variable
    SELECT @UpperFirstName
END
GO
```

### Stored Procedure Flow Control
#### If/else
```
IF @Country = 'USA'
  BEGIN
    SELECT @Locale = 'Domestic'
    PRINT 'This is a print statement'
  END
ELSE
  BEGIN
    SELECT @Locale = 'Foreign'
  END
```


#### Case statement
```
SELECT [Order Id], [Order Date],
  CASE [Shipper Id]
    WHEN 1 THEN 'UPS'
    WHEN 2 THEN 'FedEx'
    WHEN 3 THEN 'U.S. Mail'
  END AS Shipper,
[Shipping Fee]
FROM Orders
```

#### Loops
```
CREATE TABLE MyTable(
  LoopId INT,
  LoopText VarChar(25)
)

DECLARE @LoopValue INT
DECLARE @LoopText CHAR(25)

SELECT @LoopValue = 1

WHILE (@LoopValue < 100)
  BEGIN
    SELECT @LoopText = 'Iteration #' + CONVERT(VarChar(25), @LoopValue)
    INSERT INTO MyTable(LoopId, LoopText)
    VALUES (@LoopValue, @LoopText)
    SELECT @LoopValue = @LoopValue + 1
  END
```



### Error handling
- Required inputs to a stored proc have to be manually validated
- Do things like this to return info
  - __@@Functions are built in functions__
  - SELECT @OrderId = @@IDENTITY, @LocalError = @@ERROR, @LocalRows = @@ROWCOUNT
  - SELECT @OrderId, @LocalError, @LocalRows

### Transactions
```
BEGIN TRANSACTION
-- if no error
COMMIT TRANSACTION

-- if error
ROLLBACK TRANSACTION
```

### Cursors
used to iterate through the records that are returned in a result set. You can open a cursor and then iterate through it's values

### Functions
can have:
- User defined,
- Scalar - returns a single value (has begin end)
- inline table valued - returns a table (no begin end)
- multi statement table valued - returns a table (has begin end, can do complex logic)

### Triggers
can perform an action on Insert or Update on a table. Almost like a job

