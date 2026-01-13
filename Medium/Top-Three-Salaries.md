# [Top Three Salaries](https://datalemur.com/questions/sql-top-three-salaries)

## Problem Statement

As part of an ongoing analysis of salary distribution within the company, your manager has requested a report identifying high earners in each department. A 'high earner' within a department is defined as an employee with a salary ranking among the top three salaries within that department.

You're tasked with identifying these high earners across all departments. Write a query to display the employee's name along with their department name and salary. In case of duplicates, sort the results of department name in ascending order, then by salary in descending order. If multiple employees have the same salary, then order them alphabetically.

**Note:** Ensure to utilize the appropriate ranking window function to handle duplicate salaries effectively.

## Tables

### `employee` Table:

| Column Name | Type | Description |
|---|---|---|
| employee_id | integer | The unique ID of the employee. |
| name | string | The name of the employee. |
| salary | integer | The salary of the employee. |
| department_id | integer | The department ID of the employee. |
| manager_id | integer | The manager ID of the employee. |

### `department` Table:

| Column Name | Type | Description |
|---|---|---|
| department_id | integer | The department ID of the employee. |
| department_name | string | The name of the department. |

## Example Input

**employee Table:**

| employee_id | name | salary | department_id | manager_id |
|---|---|---|---|---|
| 1 | Emma Thompson | 3800 | 1 | 6 |
| 2 | Daniel Rodriguez | 2230 | 1 | 7 |
| 3 | Olivia Smith | 2000 | 1 | 8 |
| 4 | Noah Johnson | 6800 | 2 | 9 |
| 5 | Sophia Martinez | 1750 | 1 | 11 |
| 6 | Liam Brown | 13000 | 3 | |
| 7 | Ava Garcia | 12500 | 3 | |
| 8 | William Davis | 6800 | 2 | |
| 9 | Isabella Wilson | 11000 | 3 | |
| 10 | James Anderson | 4000 | 1 | 11 |

**department Table:**

| department_id | department_name |
|---|---|
| 1 | Data Analytics |
| 2 | Data Science |

## Example Output

| department_name | name | salary |
|---|---|---|
| Data Analytics | James Anderson | 4000 |
| Data Analytics | Emma Thompson | 3800 |
| Data Analytics | Daniel Rodriguez | 2230 |
| Data Science | Noah Johnson | 6800 |
| Data Science | William Davis | 6800 |

## Solution

```sql
WITH ranked_salary AS (
  SELECT 
    name,
    salary,
    department_id,
    DENSE_RANK() OVER (
      PARTITION BY department_id ORDER BY salary DESC
    ) AS ranking
  FROM employee
)
SELECT 
  d.department_name,
  s.name,
  s.salary
FROM ranked_salary AS s
INNER JOIN department AS d
  ON s.department_id = d.department_id
WHERE s.ranking <= 3
ORDER BY d.department_name ASC, s.salary DESC, s.name ASC;
```

## Explanation

### Step 1: Identify High Earners in Each Department

To determine the high earners in each department, we assign a ranking to each employee within their respective department based on salary. By using the `DENSE_RANK()` window function, we generate unique ranks for each employee's salary within their department with higher salaries receiving lower ranks.

**Why use DENSE_RANK() over RANK() or ROW_NUMBER()?**

- `DENSE_RANK()` assigns consecutive ranks to rows ensuring that ties in salaries don't create gaps in the ranking sequence.
- `RANK()` is similar to `DENSE_RANK()`, but it might leave gaps in the ranking sequence when there are ties.
- `ROW_NUMBER()` simply numbers each row sequentially without considering ties.

### Step 2: Filter to Display Top Three Salaries

We build upon the query in Step 1 by wrapping it within a CTE named `ranked_salary`. Then, we filter the results to include only employees who are considered high earners within their departments (those with a ranking of 3 or lower). Finally, we order the results by department name in ascending order, followed by salary in descending order, and employee name in ascending order.
