# Odd and Even Measurements

**Company:** Google  
**Difficulty:** Medium  
**SQL Dialect:** PostgreSQL 14

## Problem Statement

Assume you're given a table with measurement values obtained from a Google sensor over multiple days with measurements taken multiple times within each day.

Write a query to calculate the sum of odd-numbered and even-numbered measurements separately for a particular day and display the results in two different columns.

**Definition:**
- Within a day, measurements taken at 1st, 3rd, and 5th times are considered odd-numbered measurements
- Measurements taken at 2nd, 4th, and 6th times are considered even-numbered measurements

## Tables

### `measurements` Table:

| Column Name | Type |
|----------------|----------|
| measurement_id | integer |
| measurement_value | decimal |
| measurement_time | datetime |

### `measurements` Example Input:

| measurement_id | measurement_value | measurement_time |
|----------------|------------------|------------------|
| 131233 | 1109.51 | 07/10/2022 09:00:00 |
| 135211 | 1662.74 | 07/10/2022 11:00:00 |
| 523542 | 1246.24 | 07/10/2022 13:15:00 |
| 143562 | 1124.50 | 07/11/2022 15:00:00 |
| 346462 | 1234.14 | 07/11/2022 16:45:00 |

## Example Output:

| measurement_day | odd_sum | even_sum |
|---------------------|---------|----------|
| 07/10/2022 00:00:00 | 2355.75 | 1662.74 |
| 07/11/2022 00:00:00 | 1124.50 | 1234.14 |

## Explanation

On 07/10/2022, the sum of the odd-numbered measurements is 2355.75, while the sum of the even-numbered measurements is 1662.74.

## Solution

```sql
WITH numbered_time AS(
  SELECT
    DATE(measurement_time) AS measurement_day,
    measurement_value,
    RANK() OVER(
      PARTITION BY EXTRACT(DAY FROM measurement_time)
      ORDER BY measurement_time) AS times
  FROM measurements
)
SELECT
  measurement_day,
  SUM(
    CASE
      WHEN times IN (1,3,5) THEN measurement_value ELSE 0
    END) AS odd_sum,
  SUM(
    CASE
      WHEN times IN (2,4,6) THEN measurement_value ELSE 0
    END) AS even_sum
FROM numbered_time
GROUP BY measurement_day
ORDER BY measurement_day ASC;
```
