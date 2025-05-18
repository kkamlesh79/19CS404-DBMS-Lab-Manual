# Experiment 2: DDL Commands

## AIM
To study and implement DDL commands and different types of constraints.

## THEORY

### 1. CREATE
Used to create a new relation (table).

**Syntax:**
```sql
CREATE TABLE (
  field_1 data_type(size),
  field_2 data_type(size),
  ...
);
```
### 2. ALTER
Used to add, modify, drop, or rename fields in an existing relation.
(a) ADD
```sql
ALTER TABLE std ADD (Address CHAR(10));
```
(b) MODIFY
```sql
ALTER TABLE relation_name MODIFY (field_1 new_data_type(size));
```
(c) DROP
```sql
ALTER TABLE relation_name DROP COLUMN field_name;
```
(d) RENAME
```sql
ALTER TABLE relation_name RENAME COLUMN old_field_name TO new_field_name;
```
### 3. DROP TABLE
Used to permanently delete the structure and data of a table.
```sql
DROP TABLE relation_name;
```
### 4. RENAME
Used to rename an existing database object.
```sql
RENAME TABLE old_relation_name TO new_relation_name;
```
### CONSTRAINTS
Constraints are used to specify rules for the data in a table. If there is any violation between the constraint and the data action, the action is aborted by the constraint. It can be specified when the table is created (using CREATE TABLE) or after it is created (using ALTER TABLE).
### 1. NOT NULL
When a column is defined as NOT NULL, it becomes mandatory to enter a value in that column.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) NOT NULL
);
```
### 2. UNIQUE
Ensures that values in a column are unique.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) UNIQUE
);
```
### 3. CHECK
Specifies a condition that each row must satisfy.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) CHECK (logical_expression)
);
```
### 4. PRIMARY KEY
Used to uniquely identify each record in a table.
Properties:
Must contain unique values.
Cannot be null.
Should contain minimal fields.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) PRIMARY KEY
);
```
### 5. FOREIGN KEY
Used to reference the primary key of another table.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size),
  FOREIGN KEY (column_name) REFERENCES other_table(column)
);
```
### 6. DEFAULT
Used to insert a default value into a column if no value is specified.

Syntax:
```sql
CREATE TABLE Table_Name (
  col_name1 data_type,
  col_name2 data_type,
  col_name3 data_type DEFAULT 'default_value'
);
```

**Question 1**

Create a table named **Bonuses** with the following constraints:<br>
&emsp;**BonusID** as **INTEGER** should be the primary key.<br>
&emsp;**EmployeeID** as **INTEGER** should be a foreign key referencing Employees(EmployeeID).<br>
&emsp;**BonusAmount** as **REAL** should be greater than 0.<br>
&emsp;**BonusDate** as **DATE**.<br>
&emsp;**Reason** as **TEX**T should not be **NULL**.<br>

```sql
CREATE TABLE Bonuses(
    BonusID INT PRIMARY KEY,
    EmployeeID INT,
    BonusAmount REAL CHECK(BonusAmount > 1),
    BonusDate DATE,
    Reason TEXT NOT NULL,
    FOREIGN KEY(EmployeeID) references Employees(EmployeeID)
);
```

**Output:**

![image](https://github.com/user-attachments/assets/cdd261a8-6b7f-4d19-a87f-0d02c7d217b5)


**Question 2**

Create a table named **Invoices** with the following constraints:<br>
&emsp;**InvoiceID** as **INTEGER** should be the primary key.<br>
&emsp;**InvoiceDate** as **DATE**.<br>
&emsp;**Amount** as **REAL** should be greater than 0.<br>
&emsp;**DueDate** as **DATE** should be greater than the **InvoiceDate**.<br>
&emsp;**OrderID** as **INTEGER** should be a foreign key referencing **Orders(OrderID)**.<br>

```sql
CREATE TABLE Invoices(
    InvoiceID INT PRIMARY KEY,
    InvoiceDate DATE,
    Amount REAL CHECK(Amount > 0),
    DueDate DATE CHECK (DueDate > InvoiceDate),
    OrderID INT,
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
);
```

**Output:**

![image](https://github.com/user-attachments/assets/5260fe3c-3e22-4356-9c05-e3ece1a2c9de)

**Question 3**

Insert all customers from **Old_customers** into **Customers**:<br>
Table attributes are **CustomerID**, **Name**, **Address**, **Email**.<br>

```sql
INSERT INTO Customers SELECT * FROM Old_customers; 
```

**Output:**

![image](https://github.com/user-attachments/assets/0f7ca206-9d2c-4d55-9606-93f17a28966c)

**Question 4**
---
Insert the following customers into the **Customers** table:<br>
| CustomerID | Name         | Address     | City    | ZipCode |
|------------|--------------|-------------|---------|---------|
| 302        | Laura Croft  | 456 Elm St  | Seattle | 98101   |
| 303        | Bruce Wayne  | 789 Oak St  | Gotham  | 10001   |

```sql
INSERT INTO Customers VALUES(302,'Laura Croft','456 Elm St','Seattle',98101);
INSERT INTO Customers VALUES(303,'Bruce Wayne','789 Oak St','Gotham',10001);
```

**Output:**

![image](https://github.com/user-attachments/assets/e0343ec2-02fa-4f7e-8ead-f803890f64f5)

**Question 5**

Create a table named **Products** with the following columns:<br>
&emsp;**ProductID** as **INTEGER**<br>
&emsp;**ProductName** as **TEXT**<br>
&emsp;**Price** as **REAL**<br>
&emsp;**Stock** as **INTEGER**<br>

```sql
CREATE TABLE Products(
    ProductID INTEGER,
    ProductName TEXT,
    Price REAL,
    Stock INTEGER
);
```

**Output:**

![image](https://github.com/user-attachments/assets/fe80fb64-0c7a-494d-b8f8-a1f3732910fa)

**Question 6**

Create a table named **Shipments** with the following constraints:<br>
&emsp;**ShipmentID** as **INTEGER** should be the primary key.<br>
&emsp;**ShipmentDate** as **DATE**.<br>
&emsp;**SupplierID** as **INTEGER** should be a foreign key referencing **Suppliers(SupplierID)**.<br>
&emsp;**OrderID** as **INTEGER** should be a foreign key referencing **Orders(OrderID)**.<br>

```sql
CREATE TABLE Shipments(
    ShipmentID INTEGER PRIMARY KEY,
    ShipmentDate DATE,
    SupplierID INTEGER,
    OrderID INTEGER,
    FOREIGN KEY(SupplierID) REFERENCES Suppliers(SupplierID),
    FOREIGN KEY(OrderID) REFERENCES Orders(OrderID)
);
```

**Output:**

![image](https://github.com/user-attachments/assets/abc827a8-adf0-4924-b2be-616e681c7e84)

**Question 7**

Insert a student with **RollNo** 201, **Name** David Lee, **Gender** M, **Subject** Physics, and **MARKS** 92 into the **Student_details** table.

```sql
INSERT INTO Student_details VALUES(201,'David Lee','M','Physics',92);
```

**Output:**

![image](https://github.com/user-attachments/assets/d0c14eba-d4d3-4437-a445-264ebe6a4d3f)

**Question 8**

Write an SQL Query to add the attributes **designation**, **net_salary**, and **dob** to the **Companies** table with the following data types:<br>
&emsp;**designation** as **VARCHAR(50)** <br>
&emsp;**net_salary** as **NUMBER**<br>
&emsp;**dob** as **DATE**

```sql
ALTER TABLE Companies ADD designation varchar(50);
ALTER TABLE Companies ADD net_salary number;
ALTER TABLE Companies ADD dob date;
```

**Output:**

![image](https://github.com/user-attachments/assets/51c4272a-ea22-483c-a555-7834bd1d1871)

**Question 9**

Create a new table named **products** with the following specifications:<br>
&emsp;**product_id** as **INTEGER** and primary key.<br>
&emsp;**product_name** as **TEXT** and not NULL.<br>
&emsp;**list_price** as **DECIMAL (10, 2)** and not NULL.<br>
&emsp;**discount** as **DECIMAL (10, 2)** with a default value of 0 and not NULL.<br>
&emsp;A **CHECK** constraint at the table level to ensure:<br>
&emsp;&emsp;**list_price** is greater than or equal to **discount**<br>
&emsp;&emsp;**discount** is greater than or equal to 0<br>
&emsp;&emsp;**list_price** is greater than or equal to 0

```sql
CREATE TABLE products(
    product_id integer primary key,
    product_name text not null,
    list_price decimal(10,2) not null check (list_price >= discount),
    discount decimal(10,2) default 0 check (discount >= 0)
);
```

**Output:**

![image](https://github.com/user-attachments/assets/6badb06f-eb18-41ff-a01b-e1561cdb5c4d)

**Question 10**

Insert the following customers into the **Customers** table:<br>  
| CustomerID | Name         | Address     | City    | ZipCode |
|------------|--------------|-------------|---------|---------|
| 302        | Laura Croft  | 456 Elm St  | Seattle | 98101   |
| 303        | Bruce Wayne  | 789 Oak St  | Gotham  | 10001   |

```sql
ALTER TABLE customer ADD birth_date timestamp;
```

**Output:**

![image](https://github.com/user-attachments/assets/9c1dd310-e28a-4726-9d3f-43d822aa7135)

**MODULE-1 GRADE**

![image](https://github.com/user-attachments/assets/bcf28330-d708-40e2-9dd8-b929ff92f580)

## RESULT
Thus, the SQL queries to implement different types of constraints and DDL commands have been executed successfully.
