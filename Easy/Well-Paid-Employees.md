# Well Paid Employees

**Company**: FAANG  
**Difficulty**: Easy  
**Category**: SQL  

## Problem Statement

Companies often perform salary analyses to ensure fair compensation practices. One useful analysis is to check if there are any employees earning more than their direct managers.

As a HR Analyst, you're asked to identify all employees who earn more than their direct managers. The result should include the employee's ID and name.

## Schema

### `employee` Table:

| column_name | type | description |
|-------------|------|-------------|
| employee_id | integer | The unique ID of the employee. |
| name | string | The name of the employee. |
| salary | integer | The salary of the employee. |
| department_id | integer | The department ID of the employee. |
| manager_id | integer | The manager ID of the employee. |

## Example Input

### `employee` Table:

| employee_id | name | salary | department_id | manager_id |
|-------------|------------------|--------|---------------|------------|
| 1 | Emma Thompson | 3800 | 1 | 6 |
| 2 | Daniel Rodriguez | 2230 | 1 | 7 |
| 3 | Olivia Smith | 7000 | 1 | 8 |
| 4 | Noah Johnson | 6800 | 2 | 9 |
| 5 | Sophia Martinez | 1750 | 1 | 11 |
| 6 | Liam Brown | 13000 | 3 | NULL |
| 7 | Ava Garcia | 12500 | 3 | NULL |
| 8 | William Davis | 6800 | 2 | NULL |

## Example Output

| employee_id | employee_name |
|-------------|---------------|
| 3 | Olivia Smith |

## Explanation

The output shows that Olivia Smith earns $7,000, surpassing her manager, William David who earns $6,800.

## Solution

```sql
SELECT 
  emp.employee_id,
  emp.name
FROM employee AS mgr
JOIN employee AS emp
  ON mgr.employee_id = emp.manager_id
WHERE mgr.salary < emp.salary;
```

## Key Concepts

- **Self-Join**: Join a table to itself to compare rows within the same table
- **Alias**: Use table aliases (mgr, emp) to distinguish between manager and employee records
- **JOIN**: Connect manager records with employee records using manager_id
- **Comparison**: Filter employees whose salary exceeds their manager's salary
