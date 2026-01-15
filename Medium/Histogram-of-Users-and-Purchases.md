# Histogram of Users and Purchases

**Company:** Walmart
**Difficulty:** Medium
**SQL Dialect:** MySQL

## Problem Statement

Assume you're given a table on Walmart user transactions. Based on their most recent transaction date, write a query that retrieve the users along with the number of products they bought.

Output the user's most recent transaction date, user ID, and the number of products, sorted in chronological order by the transaction date.

## Tables

### `user_transactions` Schema:

| Column Name | Type | Description |
|-------------|------|-------------|
| product_id | integer | The product ID |
| user_id | integer | The user ID |
| spend | decimal | The amount spent |
| transaction_date | timestamp | The transaction date |

### `user_transactions` Example Input:

| product_id | user_id | spend | transaction_date |
|------------|---------|-------|------------------|
| 3673 | 123 | 68.90 | 07/08/2022 12:00:00 |
| 9623 | 123 | 274.10 | 07/08/2022 12:00:00 |
| 1467 | 115 | 19.90 | 07/08/2022 12:00:00 |
| 2513 | 159 | 25.00 | 07/08/2022 12:00:00 |
| 1452 | 159 | 74.50 | 07/10/2022 12:00:00 |

## Example Output:

| transaction_date | user_id | purchase_count |
|------------------|---------|----------------|
| 07/08/2022 12:00:00 | 115 | 1 |
| 07/08/2022 12:00:00 | 123 | 2 |
| 07/10/2022 12:00:00 | 159 | 1 |

## Solution

```sql
WITH latest_transactions_cte AS (
  SELECT
    transaction_date,
    user_id,
    product_id,
    RANK() OVER (
      PARTITION BY user_id
      ORDER BY transaction_date DESC) AS transaction_rank
  FROM user_transactions)

SELECT
  transaction_date,
  user_id,
  COUNT(product_id) AS purchase_count
FROM latest_transactions_cte
WHERE transaction_rank = 1
GROUP BY transaction_date, user_id
ORDER BY transaction_date;
```
