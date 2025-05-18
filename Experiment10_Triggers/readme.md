# Experiment 10: PL/SQL – Triggers

## AIM
To write and execute PL/SQL trigger programs for automating actions in response to specific table events like INSERT, UPDATE, or DELETE.

---

## THEORY

A **trigger** is a stored PL/SQL block that is automatically executed or fired when a specified event occurs on a table or view. Triggers can be used for enforcing business rules, auditing changes, or automatic updates.

### Types of Triggers:
- **Before Trigger**: Executes before the operation (INSERT, UPDATE, DELETE).
- **After Trigger**: Executes after the operation.
- **Row-level Trigger**: Executes for each affected row.
- **Statement-level Trigger**: Executes once for the triggering statement.

**Basic Syntax:**
```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE ON table_name
[FOR EACH ROW]
BEGIN
   -- trigger logic
END;
```

## 1. Write a trigger to log every insertion into a table.
**Steps:**
- Create two tables: `employees` (for storing data) and `employee_log` (for logging the inserts).
- Write an **AFTER INSERT** trigger on the `employees` table to log the new data into the `employee_log` table.

**Expected Output:**
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.

**Program:**

```sql
-- Create main and log tables
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(100),
    salary NUMBER
);
CREATE TABLE employee_log (
    log_id NUMBER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    emp_id NUMBER,
    emp_name VARCHAR2(100),
    salary NUMBER,
    log_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    message VARCHAR2(255)
);
-- Trigger to log insert
CREATE OR REPLACE TRIGGER trg_log_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log (emp_id, emp_name, salary, message)
    VALUES (:NEW.emp_id, :NEW.emp_name, :NEW.salary,
            'Inserted: ' || :NEW.emp_name || ', Salary: ' || :NEW.salary);
END;
/
-- Test the trigger
INSERT INTO employees VALUES (1, 'Alice', 5000);
SELECT * FROM employee_log;
```

**Output:**

![Screenshot 2025-05-17 084005](https://github.com/user-attachments/assets/e91be0b4-c101-4ed1-8246-df52abbe1fe7)

---

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.

**Expected Output:**
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`

**Program:**

```sql
-- Create table
CREATE TABLE sensitive_data (
    id NUMBER PRIMARY KEY,
    secret_info VARCHAR2(255)
);
-- Insert test data
INSERT INTO sensitive_data VALUES (1, 'Top Secret');
-- Trigger to prevent delete
CREATE OR REPLACE TRIGGER trg_prevent_sensitive_delete
BEFORE DELETE ON sensitive_data
FOR EACH ROW
BEGIN
    RAISE_APPLICATION_ERROR(-20001, 'ERROR: Deletion not allowed on this table.');
END;
/
-- Test: This will raise an error
DELETE FROM sensitive_data WHERE id = 1;
```

**Output:**

![Screenshot 2025-05-17 084312](https://github.com/user-attachments/assets/719e637c-db00-4a6e-b794-865185c53389)

---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.

**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.

**Program:**

```sql
-- Create table with timestamp column
CREATE TABLE products (
    product_id NUMBER PRIMARY KEY,
    product_name VARCHAR2(100),
    price NUMBER,
    last_modified TIMESTAMP
);
-- Insert test data
INSERT INTO products VALUES (1, 'Monitor', 10000, NULL);
-- Trigger to update last_modified on update
CREATE OR REPLACE TRIGGER trg_update_last_modified
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    :NEW.last_modified := SYSTIMESTAMP;
END;
/
-- Test the trigger
UPDATE products SET price = 12000 WHERE product_id = 1;
SELECT * FROM products;
```

**Output:**

![Screenshot 2025-05-17 084434](https://github.com/user-attachments/assets/382c0761-97a4-4621-80bb-05ef9ed24908)

---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.

**Expected Output:**
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.

**Program:**

```sql
-- Create main and audit tables
CREATE TABLE customer_orders (
    order_id NUMBER PRIMARY KEY,
    customer_name VARCHAR2(100),
    order_amount NUMBER
);
CREATE TABLE audit_log (
    id NUMBER PRIMARY KEY,
    update_count NUMBER
);
-- Insert test data
INSERT INTO customer_orders VALUES (1, 'John Doe', 5000);
INSERT INTO audit_log VALUES (1, 0);
-- Trigger to count updates
CREATE OR REPLACE TRIGGER trg_count_updates
AFTER UPDATE ON customer_orders
FOR EACH ROW
BEGIN
    UPDATE audit_log
    SET update_count = update_count + 1
    WHERE id = 1;
END;
/
-- Test the trigger
UPDATE customer_orders SET order_amount = 5500 WHERE order_id = 1;
UPDATE customer_orders SET customer_name = "John Doee" WHERE order_id = 1;
SELECT * FROM audit_log;
```

**Output:**

![Screenshot 2025-05-17 084820](https://github.com/user-attachments/assets/b0b4d634-693a-4ae6-8e6f-9fb24e383b83)

---

## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.

**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

**Program:**

```sql
-- Create employees table
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(100),
    salary NUMBER
);
-- Trigger to block insert if salary < 3000
CREATE OR REPLACE TRIGGER trg_salary_check
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF :NEW.salary < 3000 THEN
        RAISE_APPLICATION_ERROR(-20002, 'ERROR: Salary below minimum threshold.');
    END IF;
END;
/
-- Test 1: Valid insert
INSERT INTO employees VALUES (2, 'Bob', 4000);
-- Test 2: Invalid insert (will raise error)
INSERT INTO employees VALUES (3, 'Eve', 2500);
```

**Output:**

![Screenshot 2025-05-17 085101](https://github.com/user-attachments/assets/5ccc8086-f2fd-4ce1-b6fb-e7820488f74d)

## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.
