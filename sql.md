mysqld --verbose --help | grep -A 1 "Default options"
mysqld --initialize
`mysqld --initialize-insecure` no password for root
```
if not exist my.ini (
  echo [mysqld]> my.ini
  echo datadir=".\data">> my.ini
)
mysqld --defaults-file="%cd%\my.ini" --console %*
```

mysql -u mysqladmin -p
==============================================================================
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password'; set new password for root
SHUTDOWN; shutdown server
QUIT; quit from client

SHOW DATABASES; list dbs
CREATE DATABASE testDB;
USE testDB;
SELECT DATABASE(); show which db is used
SHOW TABLES; list tables in used db
DROP DATABASE IF EXISTS testDB;
DROP TABLE table; delete table
TRUNCATE TABLE table_name; delete rows inside table
CREATE TABLE student(
  first_name VARCHAR(30) NOT NULL,
  last_name VARCHAR(30) NOT NULL,
  email VARCHAR(60) NULL,
  state CHAR(2) NOT NULL DEFAULT "PA",
  zip MEDIUMINT UNSIGNED NOT NULL,
  phone VARCHAR(20) NOT NULL,
  birth_date DATE NOT NULL,
  sex ENUM('M', 'F') NOT NULL,
  date_entered TIMESTAMP,
  lunch_cost FLOAT NULL,
  student_id INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY
);
DESCRIBE student; show db setup
CREATE TABLE score(
  student_id INT UNSIGNED NOT NULL,
  event_id INT UNSIGNED NOT NULL,
  score INT NOT NULL,
  PRIMARY KEY(event_id, student_id));
ALTER TABLE score CHANGE event_id test_id INT UNSIGNED NOT NULL;
ALTER TABLE test ADD maxscore INT NOT NULL AFTER type;
ALTER TABLE absences MODIFY COLUMN test_taken ENUM('T','F') NOT NULL DEFAULT 'F'; MySQL syntax
ALTER TABLE table_name MODIFY column_name datatype; Oracle 10g and later syntax
ALTER TABLE table_name ALTER COLUMN column_name datatype; MS Access syntax
ALTER TABLE Persons DROP COLUMN DateOfBirth;
ALTER TABLE absences CHANGE student_id student_id INT UNSIGNED NOT NULL;
INSERT INTO score VALUES
  (1, 1, 15),
  (1, 2, 14),
  (1, 3, 28),
  (1, 4, 29),
  # Missed this day (6, 3, 24),
  (1, 5, 15);
RENAME TABLE
  absence to absences,
  class to classes,
  score to scores,
  student to students,
  test to tests;
SELECT first_name, last_name FROM students LIMIT 5, 10;
SELECT CONCAT(first_name, " ", last_name) AS 'Name',
  CONCAT(city, ", ", state) AS 'Hometown'
  FROM students;
SELECT test_id AS 'Test',
  MIN(score) AS min,
  MAX(score) AS max,
  MAX(score)-MIN(score) AS 'range',
  SUM(score) AS total,
  AVG(score) AS average
  FROM scores
  GROUP BY test_id;

NOT NULL - Ensures that a column cannot have a NULL value
UNIQUE - Ensures that all values in a column are different
PRIMARY KEY - A combination of a NOT NULL and UNIQUE. Uniquely identifies each row in a table
FOREIGN KEY - Uniquely identifies a row/record in another table
CHECK - Ensures that all values in a column satisfies a specific condition
DEFAULT - Sets a default value for a column when no value is specified
INDEX

ABS(x), ACOS(x), ASIN(x), ATAN(x), ATAN2(x,y), COS(x), COT(x), SIN(x), TAN(x), CEILING(x), DEGREES(x), EXP(x), FLOOR(x), LOG(x), LOG10(x), MOD(x, y), PI(), POWER(x, y), RADIANS(x), RAND(), ROUND(x, d), SQRT(x), TRUNCATE(x)


Comments: `--`..., `/*`...`*/`, `#`...


`SELECT * FROM table;` get all records
`SELECT field1, field2 FROM table;` get only specific fields from records
`SELECT DISTINCT field1 FROM table;` get set of values that field1 takes
`SELECT COUNT(DISTINCT field1), COUNT(DISTINCT field2) FROM table;`
`SELECT * FROM table WHERE field1='value';`
=	Equal
<>	Not equal. Note: In some versions of SQL this operator may be written as !=
>	Greater than
<	Less than
>=	Greater than or equal
<=	Less than or equal
BETWEEN	Between an inclusive range
LIKE	Search for a pattern (% - 0 or more chars, _ or ? in MS Access - one char)
IN	To specify multiple possible values for a column
AND, OR, NOT, &&, ||, !
IS NULL, IS NOT NULL
ANY, ALL

`... WHERE Country IN ('Mexico', 'Poland');`
`... WHERE Country IN (SELECT Country FROM Supplier);`
`... WHERE CustomerName LIKE 'a__%';` // starts with a and at least 3 chars
`... WHERE Price BETWEEN 10 AND 20;`
`... WHERE ProductName BETWEEN 'Carnarvon Tigers' AND 'Mozzarella di Giovanni';`
`... WHERE OrderDate BETWEEN #07/04/1996# AND #07/09/1996#;`
`... ORDER BY field1 ASC, field2 DESC;`
`SELECT TOP 20 PERCENT * FROM Customers;` select first 20% of records (ANSI syntax)
`SELECT * FROM Customers LIMIT 3;` select first three records (MySQL syntax)
`SELECT * FROM Customers WHERE ROWNUM <= 3;` select first three records (Oracle syntax)
`SELECT MIN(Price) AS SmallestPrice FROM Products;`
`SELECT COUNT(ProductId) FROM Products;`
MIN, MAX, COUNT, AVG, SUM, STD

`SELECT * INTO CustomersGermany FROM Customers WHERE Country = 'Germany';`
`SELECT * INTO CustomersBackup2017 IN 'Backup.mdb' FROM Customers;` backup customers
`SELECT * INTO newtable FROM oldtable WHERE 1 = 0;` create newtable using schema of oldtable

`INSERT INTO table VALUES (value1, value2);` insert to all columns
`INSERT INTO table (columnt1, coulumn2) VALUES (value1, value2);`
`INSERT INTO Customers (CustomerName, City) SELECT SupplierName, City FROM Suppliers;`

`UPDATE Customers SET ContactName = 'Juan';` update all records
`UPDATE Customers SET ContactName = 'Alphred Schmidt', City = 'Frankfurt' WHERE CustomerId = 1;`

`DELETE FROM Customers;` delete all records
`DELETE * FROM Customers;` delete all records
`DELETE FROM Customers WHERE CustomerName = 'Alfreds Futterkiste';`

`SELECT City FROM Customers UNION SELECT City FROM Suppliers ORDER BY City;`
`SELECT City FROM Customers UNION ALL SELECT City FROM Suppliers ORDER BY City;`
`SELECT 'c' As Type, ContactName FROM Customers UNION SELECT 's', ContactName FROM Suppliers;`

```
SELECT o.OrderID AS [order id], o.OrderDate AS "order date", c.CustomerName AS customer
  FROM Customers AS c, Orders AS o
  WHERE c.CustomerName="Around the Horn" AND c.CustomerID=o.CustomerID;

SELECT Customers.CustomerName, Customers.Country, Orders.OrderDate
  FROM Customers, Orders
  WHERE Orders.OrderID = 10308 AND Customers.CustomerID = Orders.CustomerID;

SELECT o.OrderID, c.CustomerName, o.OrderDate, p.ProductName
  FROM Orders AS o, Customers AS c, OrderDetails AS od, Products AS p
  WHERE o.CustomerID = c.CustomerID AND o.OrderID = od.OrderID AND p.ProductID = od.ProductID;

SELECT o.OrderID, c.CustomerName, o.OrderDate, p.ProductName
  FROM Orders AS o
  INNER JOIN Customers AS c ON o.CustomerID = c.CustomerID
  LEFT JOIN OrderDetails AS od ON o.OrderID = od.OrderID
  LEFT JOIN Products AS p ON p.ProductID = od.ProductID;

SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
  FROM Orders
  INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;

INNER JOIN, LEFT [OUTER] JOIN, RIGHT [OUTER] JOIN, FULL [OUTER] JOIN

SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate, od.ProductID
  FROM Orders
  INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID
  INNER JOIN OrderDetails AS od ON Orders.OrderID = od.OrderID;

SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
  FROM Customers A, Customers B
  WHERE A.CustomerID <> B.CustomerID
  AND A.City = B.City 
  ORDER BY A.City;

SELECT Country, COUNT(CustomerID) AS "Number of customers"
  FROM Customers
  GROUP BY Country
  ORDER BY "Number of customers" DESC;

SELECT Shippers.ShipperName, COUNT(Orders.OrderID) AS NumberOfOrders FROM Orders
  LEFT JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID
  GROUP BY ShipperName;

SELECT COUNT(CustomerID), Country
  FROM Customers
  GROUP BY Country
  HAVING COUNT(CustomerID) > 5;

SELECT SupplierName
  FROM Suppliers
  WHERE EXISTS (
    SELECT ProductName 
    FROM Products 
    WHERE SupplierId = Suppliers.supplierId 
    AND Price < 20
  );

SELECT ProductName
  FROM Products
  WHERE ProductID = ANY (
    SELECT ProductID
    FROM OrderDetails 
    WHERE Quantity > 99
  );

CREATE TABLE new_table_name AS
    SELECT column1, column2,...
    FROM existing_table_name
    WHERE ....;
```

