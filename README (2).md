# Joins-practice-task-0
**COMPANY**: CODTECH IT SOLUTIONS
**NAME**: NAGAMANI ANANGI
**INTERN ID**:CT6WOXI
**DOMAIN**: SQL
**DURATION**: JAN 25th TO MAR 10th
**MENTOR NAME**:Neela Santhosh Kumar
**DESCRIPTION**:INNER JOIN: Only returns rows with matching values in both tables.
LEFT JOIN: Returns all rows from the left table and matched rows from the right table.
RIGHT JOIN: Returns all rows from the right table and matched rows from the left table.
FULL JOIN: Returns all rows from both tables, with NULLs for unmatched rows.

 SQL queries for performing different types of joins: **INNER JOIN**, **LEFT JOIN**, **RIGHT JOIN**, and **FULL JOIN**. I will assume there are two tables involved: `employees` and `departments`, with the following structure:

- `employees` table:  
  - `employee_id`
  - `employee_name`
  - `department_id`

- `departments` table:  
  - `department_id`
  - `department_name`

---

### 1. **INNER JOIN**

An **INNER JOIN** returns only the rows where there is a match in both tables.

```sql
SELECT e.employee_id, e.employee_name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id;
```

### 2. **LEFT JOIN** (or **LEFT OUTER JOIN**)

A **LEFT JOIN** returns all rows from the left table (`employees`), and the matched rows from the right table (`departments`). If no match is found, NULL values are returned for columns from the right table.

```sql
SELECT e.employee_id, e.employee_name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.department_id;
```

### 3. **RIGHT JOIN** (or **RIGHT OUTER JOIN**)

A **RIGHT JOIN** returns all rows from the right table (`departments`), and the matched rows from the left table (`employees`). If no match is found, NULL values are returned for columns from the left table.

```sql
SELECT e.employee_id, e.employee_name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.department_id;
```

### 4. **FULL JOIN** (or **FULL OUTER JOIN**)

A **FULL JOIN** returns all rows when there is a match in either the left (`employees`) or right (`departments`) table. If there is no match, NULL values are returned for the missing side.

```sql
SELECT e.employee_id, e.employee_name, d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.department_id;
```

---

### Explanation:

- **INNER JOIN**: Only returns rows with matching values in both tables.
- **LEFT JOIN**: Returns all rows from the left table and matched rows from the right table.
- **RIGHT JOIN**: Returns all rows from the right table and matched rows from the left table.
- **FULL JOIN**: Returns all rows from both tables, with NULLs for unmatched rows.

