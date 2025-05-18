# Experiment 3: DML Commands

## AIM
To study and implement DML (Data Manipulation Language) commands.

## THEORY

### 1. INSERT INTO
Used to add records into a relation.
These are three type of INSERT INTO queries which are as
A)Inserting a single record
**Syntax (Single Row):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES (value_1, value_2, ...);
```
**Syntax (Multiple Rows):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES
(value_1, value_2, ...),
(value_3, value_4, ...);
```
**Syntax (Insert from another table):**
```sql
INSERT INTO table_name SELECT * FROM other_table WHERE condition;
```
### 2. UPDATE
Used to modify records in a relation.
Syntax:
```sql
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```
### 3. DELETE
Used to delete records from a relation.
**Syntax (All rows):**
```sql
DELETE FROM table_name;
```
**Syntax (Specific condition):**
```sql
DELETE FROM table_name WHERE condition;
```
### 4. SELECT
Used to retrieve records from a table.
**Syntax:**
```sql
SELECT column1, column2 FROM table_name WHERE condition;
```
**Question 1**
--
Write a SQL statement to double the availability of the product with product_id 1.

products table
```
---------------
product_id
product_name
category_id
availability
```
```sql
update products
set availability = availability * 2
where product_id = 1;
```

**Output:**

![image](https://github.com/user-attachments/assets/22995305-5814-41ba-a545-d2410f3789bb)

**Question 2**
---
Write a SQL statement to Double the salary for employees in department 20 who have a job_id ending with 'MAN'

Employees table
```
---------------
employee_id
first_name
last_name
email
phone_number
hire_date
job_id
salary
commission_pct
manager_id
department_id
```
```sql
update Employees 
set SALARY = SALARY * 2 
where job_id like "%MAN"
```

**Output:**

![image](https://github.com/user-attachments/assets/e66fb081-fc98-4845-870e-4e213068948f)

**Question 3**
---
Write a SQL statement to Update the address to '58 Lakeview, Magnolia' where supplier ID is 5 in the suppliers table.

Suppliers Table 
```
name               type
-----------------  ---------------
supplier_id        INT
supplier_name      VARCHAR(100)
contact_person     VARCHAR(100)
phone_number       VARCHAR(20)
email              VARCHAR(100)
address            VARCHAR(250)
```
```sql
update Suppliers
set address = '58 Lakeview, Magnolia'
where supplier_id=5
```

**Output:**

![image](https://github.com/user-attachments/assets/47556b01-6642-46e8-90e0-d47dc429b5e8)

**Question 4**
---
Write a SQL statement to change the email column of employees table with 'Unavailable' for all employees in employees table.

Employees table
```
---------------
employee_id
first_name
last_name
email
phone_number
hire_date
job_id
salary
commission_pct
manager_id
department_id
```
```sql
update Employees 
set EMAIL = 'Unavailable'
```

**Output:**

![image](https://github.com/user-attachments/assets/06e2e3cd-3308-4e05-a3e9-d174d180b16d)

**Question 5**
---

Decrease the reorder level by 30 percent where the product name contains 'cream' and quantity in stock is higher than reorder level in the products table.

PRODUCTS TABLE
```
name               type
-----------------  ---------------
product_id         INT
product_name       VARCHAR(100)
category           VARCHAR(50)
cost_price         DECIMAL(10,2)
sell_price         DECIMAL(10,2)
reorder_lvl        INT
quantity           INT
supplier_id        INT
```
```sql
UPDATE  PRODUCTS 
set reorder_lvl = reorder_lvl * 0.7
where product_name like '%cream%' and quantity > reorder_lvl;
```

**Output:**

![image](https://github.com/user-attachments/assets/0dac1f67-600d-4e7b-a9e2-b30506b15e22)

**Question 6**
---
Write a SQL query to Delete customers from 'customer' table where 'CUST_CITY' is not 'New York' and 'OUTSTANDING_AMT' is greater than 5000.

Sample table: Customer
```
+-----------+-------------+-------------+--------------+--------------+-------+-------------+-------------+-------------+---------------+--------------+------------+  
|CUST_CODE  | CUST_NAME   | CUST_CITY   | WORKING_AREA | CUST_COUNTRY | GRADE | OPENING_AMT | RECEIVE_AMT | PAYMENT_AMT |OUTSTANDING_AMT| PHONE_NO     | AGENT_CODE |
+-----------+-------------+-------------+--------------+--------------+-------+-------------+-------------+-------------+---------------+--------------+------------+
| C00013    | Holmes      | London      | London       | UK           |     2 |     6000.00 |     5000.00 |     7000.00 |       4000.00 | BBBBBBB      | A003       |
| C00001    | Micheal     | New York    | New York     | USA          |     2 |     3000.00 |     5000.00 |     2000.00 |       6000.00 | CCCCCCC      | A008       |
| C00020    | Albert      | New York    | New York     | USA          |     3 |     5000.00 |     7000.00 |     6000.00 |       6000.00 |BBBBSBB      | A008       
```
```sql
delete FROM Customer
where CUST_CITY <>'New York' and OUTSTANDING_AMT > 5000;
```

**Output:**

![image](https://github.com/user-attachments/assets/78864722-0448-4e14-81d5-e8622fad2ced)

**Question 7**
---
Write a SQL query to Delete customers from 'customer' table where 'GRADE' is odd.

Sample table: Customer
```
+-----------+-------------+-------------+--------------+--------------+-------+-------------+-------------+-------------+---------------+--------------+------------+  
|CUST_CODE  | CUST_NAME   | CUST_CITY   | WORKING_AREA | CUST_COUNTRY | GRADE | OPENING_AMT | RECEIVE_AMT | PAYMENT_AMT |OUTSTANDING_AMT| PHONE_NO     | AGENT_CODE |
+-----------+-------------+-------------+--------------+--------------+-------+-------------+-------------+-------------+---------------+--------------+------------+
| C00013    | Holmes      | London      | London       | UK           |     2 |     6000.00 |     5000.00 |     7000.00 |       4000.00 | BBBBBBB      | A003       |
| C00001    | Micheal     | New York    | New York     | USA          |     2 |     3000.00 |     5000.00 |     2000.00 |       6000.00 | CCCCCCC      | A008       |
| C00020    | Albert      | New York    | New York     | USA          |     3 |     5000.00 |     7000.00 |     6000.00 |       6000.00 | BBBBSBB      | A008     
```
```sql
delete from Customer
where grade%2=1;
```

**Output:**

![image](https://github.com/user-attachments/assets/8a2ed82a-77c6-45ad-ba68-86ad9bf18f3a)

**Question 8**
---
Write a SQL query to Delete All Doctors with a NULL Specialization

Sample table: Doctors

attributes : doctor_id, first_name, last_name, specialization
```sql
delete from Doctors
where specialization is null;
```

**Output:**

![image](https://github.com/user-attachments/assets/4d4af6a2-6a17-4d4c-974d-74d7586510e2)

**Question 9**
---
Show the categoryName and description from the categories table sorted by categoryName.
```
name                     type
---------------       ---------------
CategoryID           INTEGER
CategoryName     VARCHAR(25)
Description          VARCHAR(255)
```
```sql
select CategoryName,Description from categories
order by CategoryName ASC;
```

**Output:**

![image](https://github.com/user-attachments/assets/d4041812-1932-4172-b8e1-e60c30d00330)

**Question 10**
---
Write a SQL query to categorize value1 in the Calculations table as 'High' if it is greater than 50, otherwise 'Low'.
```
cid         name        type        notnull     dflt_value  pk
----------  ----------  ----------  ----------  ----------  ----------
0           id          INTEGER     0                       1
1           value1      REAL        0                       0
2           value2      REAL        0                       0
3           base        INTEGER     0                       0
4           exponent    INTEGER     0                       0
5           number      REAL        0                       0
6           decimal     REAL        0                       0
```
```sql
select id,value1,
    case
        when value1 > 50 then 'High'
        else 'Low'
    end as value_category
from Calculations;
```

**Output:**

![image](https://github.com/user-attachments/assets/9d7ee5a6-d8af-433b-aa93-4911bcc16c4e)

## RESULT
Thus, the SQL queries to implement DML commands have been executed successfully.
