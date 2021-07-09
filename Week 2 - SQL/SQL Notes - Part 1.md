# SQL Notes - Part 1

A series of SQL notes and breakdowns, from the 2nd week of training.

----------------------
### CREATE Table

#### Example 1
```sql
CREATE TABLE film_table
(
	film_name VARCHAR(10),
	film_type VARCHAR(6),
	
);
```

#### Example 2

```sql
CREATE TABLE film_table
(
	id int IDENTITY(1,1) PRIMARY KEY,
	name VARCHAR(30) NOT NULL,
	genre VARCHAR(20),
	release_date DATE,
	director VARCHAR(40),
	writer VARCHAR(40),
	star VARCHAR(40),
	language VARCHAR(20),
	website VARCHAR(40),
	plot_summary VARCHAR(MAX)
)
```
#### Example 3

```sql
CREATE TABLE Customer
(
	CustomerID INT IDENTITY (1,1) PRIMARY KEY,
	name VARCHAR(255) NOT NULL DEFAULT 'Nish',
	height DECIMAL (7,3),
	is_happy BIT
)
```

#### Example 4
```sql
CREATE TABLE Orders
(
	OrderID INT IDENTITY (1,1) PRIMARY KEY,
	Order_Number INT NOT NULL,
	CustomerID INT FOREIGN KEY REFERENCES Customer (CustomerID)
	--CustomerID int,
	--FOREIGN KEY (CustomerID) REFERENCES Customer (CustomerID)
)
```


-----------------
### SELECT
#### Example 1
```sql
SELECT c.NAME, o.Order_Number
FROM Customer c
INNER JOIN Orders o
On o.CustomerID = c.CustomerID;
```
#### Example 2
```sql
  SELECT * FROM film_table
```

#### WHERE - Example 1
```sql
SELECT COUNT(*) From Employees
WHERE City = 'London';
```

#### WHERE - Example 2
```sql
SELECT CompanyName, City, Country, Region
FROM Customers
WHERE REGION = 'BC'
ORDER BY CompanyName
```

#### TOP
```sql
SELECT Top 10 CompanyName, City
FROM Customers
WHERE Country = 'France'
```

#### TOP WITH TIES
```sql
SELECT TOP 1 WITH TIES * 
```
#### COUNT - Example 1
```sql
SELECT COUNT (Distinct Country)
FROM Customers
WHERE ContactTitle = 'Owner'
```

#### Count - Example 2
``` sql
SELECT COUNT(*) FROM Orders
WHERE EmployeeID = 5 or EmployeeID = 7
```

-----------------
### INSERT Into
#### Example 1
```SQL
insert into film_table
values (
	'the matrix', 
	'sci-fi', 
	'1-5-1999',
	null, 
	null, 
	'keanu reeves',
	'english', 
	'www.matrix.com',
	'best film ever'
	)
```
#### Example 2
```sql
INSERT INTO Customer
VALUES ('Christopher Rafferty', 190, 1),
		('Nish Mandal', 190, 0)
		```

-----------------
### Columns
#### ADD Column

```sql
 ALTER TABLE film_table
ADD Country VARCHAR(10);

```

#### Example 3
```sql
INSERT INTO Orders
VALUES (0033, 1), (0034, 2)
```

-----------------
### DROP Table

```sql
 DROP TABLE IF EXISTS film_table;
```

----------------
### DROP Column

```sql
ALTER TABLE film_table
 DROP COLUMN rating
```

--------------
### UPDATE Column 
```sql
UPDATE film_table
SET genre = 'Period'
Where Director = 'Joe Russo'
SELECT * FROM film_table
```

--------------
### ALTER Table 
#### Example 1
```sql
ALTER TABLE video_games_developer
ADD developerid INT IDENTITY (1,1) PRIMARY KEY
```
#### Example 2
```sql
ALTER TABLE video_games
ADD DeveloperID INT,
FOREIGN KEY (DeveloperID) REFERENCES video_games_developer(DeveloperID);
```
---------------

### COUNT
#### Example 1
```sql
SELECT COUNT(*) FROM Customers
WHERE Region IS NOT NULL ```

#### Example 2
````sql
SELECT COUNT(*) From Employees
WHERE City = 'London'; ```

-------------------
### JOIN
#### Example 1
```sql
SELECT v.videogamename, d.developername
FROM video_games v
INNER JOIN video_games_developer d ON v.DeveloperID = d.developerid
```

-------------------
### WILDCARD
#### Example 1
```sql
SELECT * FROM Customers
WHERE Country LIKE '_S_'
```

#### Example 2
```sql
SELECT ProductName
From Products WHERE ProductName LIKE 'ch%'
```

#### Example 3
```sql
SELECT * FROM Categories
Where CategoryName LIKE '[bs]%'
```


