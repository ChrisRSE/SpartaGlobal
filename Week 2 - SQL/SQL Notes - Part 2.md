# SQL Notes - Part 2

A series of SQL notes and breakdowns, from the 2nd week of training.

----------------------
### ALIAS

#### Example 1
```sql
SELECT CompanyName AS 'Company Name',
City + ', ' + Country AS City,
Region
FROM Customers
WHERE Region IS NOT NULL
```

#### Example 2
```sql
SELECT UnitPrice, Quantity, Discount,
UnitPrice * Quantity AS 'Gross Total',
UnitPrice * Quantity * (1 - Discount) AS 'Total Including Discount'
FROM [Order Details]
ORDER BY 'Gross Total' DESC
```

----------------------
### SUBSTRING

```sql
SELECT SUBSTRING ('Samuel', 1, 3)
```

----------------------
### CHARINDEX

```sql
SELECT CHARINDEX ('i', 'Sabir')
```

----------------------
### LEFT and RIGHT

```sql
SELECT LEFT('Hannah', 3)
SELECT Right('Hannah', 3)
```

----------------------
### LTRIM & RTRIM

```sql
SELECT LTRIM ('                Denzel')
```

----------------------
### LEN

```sql
SELECT LEN('Ronil')
```

----------------------
### REPLACE

```sql
SELECT REPLACE ('Nish', 'i', 'a')
```
----------------------
### UPPER and LOWER

```sql
SELECT UPPER ('nish')
SELECT LOWER ('NISH')
```

----------------------
### DATEADD / DATEDIFF
#### Functions
```sql
SELECT GETDATE();
SELECT SYSDATETIME();
SELECT YEAR(GETDATE())
SELECT MONTH(GETDATE())
SELECT DATEADD(dd, 30, '1996/11/02')
SELECT DATEDIFF(dd, '02/11/1989', GETDATE())
```

#### Example 1
```sql
SELECT
DATEADD(dd, 5, OrderDate) AS 'Due Date',
DATEDIFF(dd, OrderDate, ShippedDate) AS 'Ship Days'
FROM Orders
SELECT Format (GETDATE(), 'dd------MM-------yyyy')
```

#### Example 2
```sql
SELECT FORMAT(o.OrderDate, 'dd/MM/yyyy') AS 'Order Date',
FORMAT ((od.UnitPrice * od.Quantity) * (1-od.Discount), 'C', 'en-GB') AS 'Net Total'
FROM Orders o
INNER JOIN [Order Details] od ON o.OrderID = od.OrderID
INNER JOIN Customers c ON c.CustomerID = o.CustomerID
WHERE (od.UnitPrice * od.Quantity) * (1-od.Discount) > 100
```

------------------------
### CASE (IF / ELSE)
#### Example 1
```sql
SELECT 
OrderDate, ShippedDate,

CASE 
WHEN DATEDIFF(dd, OrderDate, ShippedDate) < 5 THEN 'Early'
WHEN DATEDIFF(dd, OrderDate, ShippedDate) < 10 THEN 'On Time'
ELSE 'Overdue'
END
AS 'Status'

FROM Orders

 CASE TASK
SELECT Name, Age,

CASE
WHEN Age >= 65 THEN 'Retired'
WHEN Age >= 60 THEN 'Retirement Due'
ELSE 'More than 5 Years to go'
END
AS 'Status'

FROM Employee_Birthdate
```

#### Example 2
```sql
SELECT COUNT(Region) FROM Customers
SELECT COUNT(*) FROM Customers WHERE Region is NOT NULL

SELECT 
SupplierID,
SUM(UnitsOnOrder) AS 'Total On Order',
AVG(UnitsOnOrder) AS 'AVG On Order',
MIN(UnitsOnOrder) AS 'Min On Order',
MAX(UnitsOnOrder) AS 'Max On Order'
FROM PRODUCTS
GROUP By SupplierID

Task
SELECT TOP 1 with ties CategoryID, AVG(ReorderLevel) AS 'Average Reorder Level'
FROM Products
GROUP BY CategoryID
ORDER BY [Average Reorder Level] DESC
```
-----------------
#### Example 1

```sql
SELECT SupplierID,
SUM(UnitsOnOrder) AS 'Total On Order',
AVG(UnitsOnOrder) AS 'Average On Order'
FROM Products
GROUP BY SupplierID
HAVING AVG(UnitsOnOrder) > 5
```

#### Example 2 - With Join

```sql
SELECT et.EmployeeID, e.FirstName + ' ' + e.LastName AS 'Employee Name',
COUNT(TerritoryID) AS 'No. of Territories covered'
FROM EmployeeTerritories et
JOIN Employees e ON et.EmployeeID = e.EmployeeID
GROUP BY et.EmployeeID, e.FirstName, e.LastName
HAVING COUNT(TerritoryID) > 5
```

--------------
### JOIN
#### Example 1

```sql
SELECT 
p.ProductName,
p.UnitPrice,
s.CompanyName,
c.CategoryName
FROM Products p
JOIN Suppliers s ON p.SupplierID = s.SupplierID
JOIN Categories c ON p.CategoryID = c.CategoryID
GROUP BY p.ProductName, p.UnitPrice, s.CompanyName, c.CategoryName
```

#### Example 2
```sql
SELECT o.OrderID, o.OrderDate, o.Freight, c.ContactName,
e.FirstName + ' ' + e.LastName as 'Employee Name'
FROM Orders o
Join Customers c ON c.CustomerID = o.CustomerID
join Employees e ON e.EmployeeID = o.EmployeeID
GROUP BY o.OrderID, o.OrderDate, o.Freight, c.ContactName
```

------------------

### SUBQUERY
```sql
SELECT
od.ProductID,
sq1.Totalamt,
UnitPrice,
UnitPrice/Totalamt * 100 AS '% of Total'
FROM [Order Details] od
INNER JOIN
	(SELECT ProductID,
		SUM(UnitPrice * Quantity) AS Totalamt
		FROM[Order Details]
		GROUP BY ProductID) sq1 ON sq1.ProductID = od.ProductID

WITH MeanAbove30 AS
	(
		SELECT CategoryID FROM Products
		GROUP BY CategoryID HAVING AVG(UnitPrice) > 30
	)
SELECT ProductName, ProductID, m.CategoryID FROM Products p
JOIN MeanAbove30 m ON p.CategoryID = m.CategoryID
```











