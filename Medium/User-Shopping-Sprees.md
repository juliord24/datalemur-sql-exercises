# User Shopping Sprees

**Company:** Amazon
**Difficulty:** Medium
**SQL Dialect:** PostgreSQL 14

## Problem Statement

In an effort to identify high-value customers, Amazon asked for your help to obtain data about users who go on shopping sprees. A shopping spree occurs when a user makes purchases on 3 or more consecutive days.

List the user IDs who have gone on at least 1 shopping spree in ascending order.

## Tables

### `transactions` Schema:

| Column Name | Type | Description |
|-------------|------|-------------|
| user_id | integer | The user ID |
| amount | float | The amount spent |
| transaction_date | timestamp | The transaction date |

### `transactions` Example Input:

| user_id | amount | transaction_date |
|---------|--------|------------------|
| 1 | 9.99 | 08/01/2022 10:00:00 |
| 1 | 55 | 08/17/2022 10:00:00 |
| 2 | 149.5 | 08/05/2022 10:00:00 |
| 2 | 4.89 | 08/06/2022 10:00:00 |
| 2 | 34 | 08/07/2022 10:00:00 |

## Example Output:

| user_id |
|---------|
| 2 |

**Explanation:** In this example, `user_id` 2 is the only one who has gone on a shopping spree.

## Solution

```sql
SELECT DISTINCT
  T1.user_id
FROM transactions AS T1
INNER JOIN transactions AS T2
  ON DATE(T2.transaction_date) = DATE(T1.transaction_date) + 1
INNER JOIN transactions AS T3
  ON DATE(T3.transaction_date) = DATE(T1.transaction_date) + 2
ORDER BY T1.user_id;
```
