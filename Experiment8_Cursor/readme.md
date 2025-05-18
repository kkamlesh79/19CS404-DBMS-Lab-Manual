# Experiment 8: PL/SQL Cursor Programs

## AIM
To write and execute PL/SQL programs using cursors and exception handling to manage runtime errors effectively and display appropriate messages.

## THEORY

In PL/SQL, cursors are used to handle query result sets row-by-row. 

There are two types of cursors:

- Implicit Cursors: Automatically created by PL/SQL for single-row queries.
- Explicit Cursors: Declared and controlled by the programmer for multi-row queries.

Types of Explicit Cursors:

1. Simple Cursor: Basic cursor to iterate over multiple rows.

2. Parameterized Cursor: Accepts parameters to filter the result dynamically.

3. Cursor FOR Loop: Simplifies cursor operations (open, fetch, close).

4. %ROWTYPE Cursor: Fetches entire row into a record using %ROWTYPE.

5. Cursor with FOR UPDATE: Used for row-level locking and updating the rows while looping.

**Syntax:**
```sql
DECLARE 
   <declarations section> 
BEGIN 
   <executable command(s)>
EXCEPTION 
   <exception handling> 
END;
```

### Basic Components of PL/SQL Block:

- DECLARE: Section to declare variables and constants.
- BEGIN: The execution section that contains PL/SQL statements.
- EXCEPTION: Handles errors or exceptions that occur in the program.
- END: Marks the end of the PL/SQL block.

**Exception Handling**

PL/SQL provides a robust mechanism to handle runtime errors using exception handling blocks. When an error occurs during execution, control is passed to the EXCEPTION section, where specific or general errors can be handled gracefully.

### Components of Exception Handling:
- Predefined Exceptions: Automatically raised by PL/SQL for common errors (e.g., NO_DATA_FOUND, TOO_MANY_ROWS, ZERO_DIVIDE).
- User-defined Exceptions: Declared explicitly in the declaration section using the EXCEPTION keyword.
- WHEN OTHERS: A generic handler for all exceptions not handled explicitly.

```sql
BEGIN
   -- Statements
EXCEPTION
   WHEN exception_name THEN
      -- Handling code
   WHEN OTHERS THEN
      -- Handling for unknown errors
END;
```

### **Question 1: Simple Cursor with Exception Handling**

**Write a PL/SQL program using a simple cursor to fetch employee names and designations from the `employees` table. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: When no rows are fetched.
2. **OTHERS**: Any other unexpected errors during execution.

**Steps:**

- Create an `employees` table with fields `emp_id`, `emp_name`, and `designation`.
- Insert some sample data into the table.
- Use a simple cursor to fetch and display employee names and designations.
- Implement exception handling to catch the relevant exceptions and display appropriate messages.

**Output:**  
The program should display the employee details or an error message.

**Program:**

```sql
-- Step 1: Create the table
CREATE TABLE employees (
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50)
);

-- Step 2: Insert sample data
INSERT INTO employees VALUES (1, 'Alice', 'Manager');
INSERT INTO employees VALUES (2, 'Bob', 'Developer');
COMMIT;

-- Step 3: PL/SQL Program
DECLARE
    CURSOR emp_cursor IS
        SELECT emp_name, designation FROM employees;

    v_name employees.emp_name%TYPE;
    v_desg employees.designation%TYPE;

BEGIN
    OPEN emp_cursor;
    LOOP
        FETCH emp_cursor INTO v_name, v_desg;
        EXIT WHEN emp_cursor%NOTFOUND;
        DBMS_OUTPUT.PUT_LINE('Name: ' || v_name || ', Designation: ' || v_desg);
    END LOOP;
    CLOSE emp_cursor;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employee data found.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```

**Output:**

![Screenshot 2025-05-17 094131](https://github.com/user-attachments/assets/c8411607-db60-419f-98dc-2ad8684222f0)

---

### **Question 2: Parameterized Cursor with Exception Handling**

**Write a PL/SQL program using a parameterized cursor to retrieve and display employees with a salary in a given range. Implement exception handling for the following errors:**

1. **NO_DATA_FOUND**: When no employees meet the salary criteria.
2. **OTHERS**: For any unexpected errors during the execution.

**Steps:**

- Modify the `employees` table by adding a `salary` column.
- Insert sample salary values for the employees.
- Use a parameterized cursor to accept a salary range as input and fetch employees within that range.
- Implement exception handling to catch and display relevant error messages.

**Output:**  
The program should display the employee details within the specified salary range or an error message if no data is found.

**Program:**

```sql
-- Step 1: Alter table to add salary
ALTER TABLE employees ADD (salary NUMBER);

-- Step 2: Update sample salaries
UPDATE employees SET salary = 50000 WHERE emp_id = 1;
UPDATE employees SET salary = 30000 WHERE emp_id = 2;
COMMIT;

-- Step 3: PL/SQL Program
DECLARE
    CURSOR salary_cursor(p_min NUMBER, p_max NUMBER) IS
        SELECT emp_name, salary FROM employees
        WHERE salary BETWEEN p_min AND p_max;

    v_name employees.emp_name%TYPE;
    v_salary employees.salary%TYPE;
    v_count NUMBER := 0;

BEGIN
    FOR emp_rec IN salary_cursor(25000, 55000) LOOP
        DBMS_OUTPUT.PUT_LINE('Name: ' || emp_rec.emp_name || ', Salary: ' || emp_rec.salary);
        v_count := v_count + 1;
    END LOOP;

    IF v_count = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the given salary range.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error occurred: ' || SQLERRM);
END;
/
```

**Output:**

![Screenshot 2025-05-17 094231](https://github.com/user-attachments/assets/fb61873b-43a6-4757-924c-424b703e25d8)

---

### **Question 3: Cursor FOR Loop with Exception Handling**

**Write a PL/SQL program using a cursor FOR loop to retrieve and display all employee names and their department numbers from the `employees` table. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: If no employees are found in the database.
2. **OTHERS**: For any other unexpected errors.

**Steps:**

- Modify the `employees` table by adding a `dept_no` column.
- Insert sample department numbers for employees.
- Use a cursor FOR loop to fetch and display employee names along with their department numbers.
- Implement exception handling to catch the relevant exceptions.

**Output:**  
The program should display employee names with their department numbers or the appropriate error message if no data is found.

**Program:**

```sql
-- Step 1: Alter table to add dept_no
ALTER TABLE employees ADD (dept_no NUMBER);

-- Step 2: Update department numbers
UPDATE employees SET dept_no = 10 WHERE emp_id = 1;
UPDATE employees SET dept_no = 20 WHERE emp_id = 2;
COMMIT;

-- Step 3: PL/SQL Program
DECLARE
    v_count NUMBER := 0;
BEGIN
    FOR emp_rec IN (SELECT emp_name, dept_no FROM employees) LOOP
        DBMS_OUTPUT.PUT_LINE('Name: ' || emp_rec.emp_name || ', Department: ' || emp_rec.dept_no);
        v_count := v_count + 1;
    END LOOP;

    IF v_count = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employee records found.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```

**Output:**

![Screenshot 2025-05-17 094247](https://github.com/user-attachments/assets/2d086561-d573-45f1-9822-347ab610f0a6)

---

### **Question 4: Cursor with `%ROWTYPE` and Exception Handling**

**Write a PL/SQL program that uses a cursor with `%ROWTYPE` to fetch and display complete employee records (emp_id, emp_name, designation, salary). Implement exception handling for the following errors:**

1. **NO_DATA_FOUND**: When no employees are found in the database.
2. **OTHERS**: For any other errors that occur.

**Steps:**

- Modify the `employees` table by adding `emp_id`, `emp_name`, `designation`, and `salary` fields.
- Insert sample data into the `employees` table.
- Declare a cursor using `%ROWTYPE` to fetch complete rows from the `employees` table.
- Implement exception handling to catch the relevant exceptions and display appropriate messages.

**Output:**  
The program should display employee records or the appropriate error message if no data is found.

**Program:**

```sql
-- Step 1: Already created table includes emp_id, emp_name, designation, and salary

-- Step 2: PL/SQL Program
DECLARE
    CURSOR emp_cursor IS
        SELECT emp_id, emp_name, designation, salary FROM employees;

    emp_record emp_cursor%ROWTYPE;
    v_count NUMBER := 0;

BEGIN
    OPEN emp_cursor;
    LOOP
        FETCH emp_cursor INTO emp_record;
        EXIT WHEN emp_cursor%NOTFOUND;
        DBMS_OUTPUT.PUT_LINE('ID: ' || emp_record.emp_id || 
                             ', Name: ' || emp_record.emp_name ||
                             ', Designation: ' || emp_record.designation ||
                             ', Salary: ' || emp_record.salary);
        v_count := v_count + 1;
    END LOOP;
    CLOSE emp_cursor;

    IF v_count = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employee records found.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```

**Output:**

![Screenshot 2025-05-17 094310](https://github.com/user-attachments/assets/4497ea79-0195-4ae2-86a9-916e2a7593bc)

---

### **Question 5: Cursor with FOR UPDATE Clause and Exception Handling**

**Write a PL/SQL program using a cursor with the `FOR UPDATE` clause to update the salary of employees in a specific department. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: If no rows are affected by the update.
2. **OTHERS**: For any unexpected errors during execution.

**Steps:**

- Modify the `employees` table to include a `dept_no` and `salary` field.
- Insert sample data into the `employees` table with different department numbers.
- Use a cursor with the `FOR UPDATE` clause to lock the rows of employees in a specific department and update their salary.
- Implement exception handling to handle `NO_DATA_FOUND` or other errors that may occur.

**Output:**  
The program should update employee salaries and display a message, or it should display an error message if no data is found.

**Program:**

```sql
-- Step 1: Employees table already has dept_no and salary fields

-- Step 2: PL/SQL Program
DECLARE
    CURSOR dept_cursor IS
        SELECT emp_id, salary FROM employees
        WHERE dept_no = 10
        FOR UPDATE;

    v_count NUMBER := 0;

BEGIN
    FOR emp_rec IN dept_cursor LOOP
        UPDATE employees
        SET salary = emp_rec.salary + 5000
        WHERE emp_id = emp_rec.emp_id;
        v_count := v_count + 1;
    END LOOP;

    IF v_count = 0 THEN
        RAISE NO_DATA_FOUND;
    ELSE
        DBMS_OUTPUT.PUT_LINE(v_count || ' employee(s) updated.');
    END IF;

    COMMIT;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the specified department.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error occurred during update: ' || SQLERRM);
END;
/
```

**Output:**

![Screenshot 2025-05-17 094335](https://github.com/user-attachments/assets/d17c9b75-7947-44eb-b593-fcb2fb61237b)

---

## RESULT
Thus, the program successfully executed and displayed employee details using a cursor. 

