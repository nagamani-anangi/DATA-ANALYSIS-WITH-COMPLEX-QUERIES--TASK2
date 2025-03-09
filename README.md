# DATA-ANALYSIS-WITH-COMPLEX-QUERIES--TASK2
### CODTECH INTERNSHIP TASK - 2: **Data Analysis with Complex Queries**

In this task, we will perform **advanced data analysis** using **Window Functions**, **Subqueries**, and **Common Table Expressions (CTEs)**. We will assume the following table structures for this example:

1. **employees** table:
   - `employee_id`
   - `employee_name`
   - `department_id`
   - `salary`
   - `hire_date`

2. **departments** table:
   - `department_id`
   - `department_name`

3. **sales** table:
   - `sales_id`
   - `employee_id`
   - `sale_amount`
   - `sale_date`

The goal is to create a series of SQL queries that use these advanced techniques to analyze data and generate insights.

---

### 1. **Window Function Example: Ranking Employees Based on Salary in Each Department**

We use a **Window Function** to rank employees based on their salary within their respective departments. This will help us identify the highest and lowest-paid employees within each department.

```sql
SELECT 
    e.employee_id, 
    e.employee_name, 
    d.department_name, 
    e.salary,
    RANK() OVER (PARTITION BY e.department_id ORDER BY e.salary DESC) AS salary_rank
FROM employees e
JOIN departments d ON e.department_id = d.department_id;
```

#### Output:

| employee_id | employee_name | department_name | salary | salary_rank |
|-------------|---------------|-----------------|--------|-------------|
| 1           | John Doe      | Sales           | 80000  | 1           |
| 2           | Jane Smith    | Sales           | 70000  | 2           |
| 3           | Jim Brown     | HR              | 90000  | 1           |
| 4           | Emily Clark   | HR              | 85000  | 2           |

- **Explanation**: The `RANK()` function ranks employees within each department (`PARTITION BY e.department_id`) based on their salary in descending order (`ORDER BY e.salary DESC`). 

---

### 2. **Subquery Example: Total Sales by Employee**

In this example, we will use a **Subquery** to calculate the total sales made by each employee. The subquery will calculate the total sales for each employee based on their `employee_id`.

```sql
SELECT 
    e.employee_id,
    e.employee_name,
    (SELECT SUM(s.sale_amount) 
     FROM sales s 
     WHERE s.employee_id = e.employee_id) AS total_sales
FROM employees e;
```

#### Output:

| employee_id | employee_name | total_sales |
|-------------|---------------|-------------|
| 1           | John Doe      | 50000       |
| 2           | Jane Smith    | 45000       |
| 3           | Jim Brown     | 60000       |
| 4           | Emily Clark   | 70000       |

- **Explanation**: The subquery computes the sum of sales for each employee by matching `employee_id` in the `sales` table and then returns the total sales amount.

---

### 3. **CTE Example: Highest Paid Employee in Each Department**

We will use a **Common Table Expression (CTE)** to find the highest-paid employee in each department and join it with the `employees` and `departments` tables.

```sql
WITH HighestPaid AS (
    SELECT 
        e.department_id, 
        MAX(e.salary) AS highest_salary
    FROM employees e
    GROUP BY e.department_id
)
SELECT 
    e.employee_id, 
    e.employee_name, 
    d.department_name, 
    e.salary
FROM employees e
JOIN departments d ON e.department_id = d.department_id
JOIN HighestPaid hp ON e.salary = hp.highest_salary
WHERE e.department_id = hp.department_id;
```

#### Output:

| employee_id | employee_name | department_name | salary |
|-------------|---------------|-----------------|--------|
| 1           | John Doe      | Sales           | 80000  |
| 3           | Jim Brown     | HR              | 90000  |

- **Explanation**: The CTE `HighestPaid` calculates the highest salary in each department. We then join it with the `employees` and `departments` tables to get the highest-paid employee in each department.

---

### 4. **Window Function Example: Moving Average of Sales**

We will use a **Window Function** to calculate a moving average of sales for each employee over the past 3 sales. The moving average gives us an idea of how an employee's sales are trending.

```sql
SELECT 
    s.sales_id, 
    s.employee_id, 
    s.sale_amount,
    s.sale_date,
    AVG(s.sale_amount) OVER (PARTITION BY s.employee_id ORDER BY s.sale_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg
FROM sales s;
```

#### Output:

| sales_id | employee_id | sale_amount | sale_date  | moving_avg |
|----------|-------------|-------------|------------|------------|
| 1        | 1           | 1000        | 2025-03-01 | 1000       |
| 2        | 1           | 1200        | 2025-03-02 | 1100       |
| 3        | 2           | 1300        | 2025-03-03 | 1166.67    |
| 4        | 3           | 1500        | 2025-03-04 | 1300       |

- **Explanation**: The window function `AVG()` calculates the moving average of the `sale_amount` for each employee. The `ROWS BETWEEN 2 PRECEDING AND CURRENT ROW` clause calculates the average of the current row and the previous two rows, thus giving us a 3-day moving average.

---

### 5. **Subquery and CTE: Employees with Sales Greater Than the Average Sales**

In this query, we use a **Subquery** and a **CTE** to find employees who have made total sales greater than the average sales in the system.

```sql
WITH EmployeeSales AS (
    SELECT 
        s.employee_id, 
        SUM(s.sale_amount) AS total_sales
    FROM sales s
    GROUP BY s.employee_id
)
SELECT 
    e.employee_id, 
    e.employee_name, 
    es.total_sales
FROM employees e
JOIN EmployeeSales es ON e.employee_id = es.employee_id
WHERE es.total_sales > (
    SELECT AVG(sale_amount) FROM sales
);
```

#### Output:

| employee_id | employee_name | total_sales |
|-------------|---------------|-------------|
| 1           | John Doe      | 50000       |
| 3           | Jim Brown     | 60000       |

- **Explanation**: The CTE `EmployeeSales` calculates the total sales for each employee. The main query then compares each employee's total sales against the average sales calculated in the subquery and returns employees whose total sales are greater than the average.

---

### Conclusion

This report demonstrates how **Window Functions**, **Subqueries**, and **CTEs** can be used to perform **advanced data analysis**. These techniques provide powerful ways to analyze data across different dimensions and derive insights. Below are the key learnings from each query:

1. **Window Functions** are used for ranking, running totals, and moving averages.
2. **Subqueries** allow for dynamic calculations and comparisons based on aggregated data.
3. **CTEs** make queries more readable and reusable by breaking down complex logic into modular steps.
