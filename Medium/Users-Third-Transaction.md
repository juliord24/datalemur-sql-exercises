# [User's Third Transaction](https://datalemur.com/questions/sql-third-transaction) - Uber

- **Topics:** Window Functions, CTE
- **Description:** Write a query to obtain the third transaction of every user. Output the user id, spend and transaction date

## 📊 Tables

### `transactions` Table:

| Column Name | Type |
|---|---|
| user_id | integer |
| spend | decimal |
| transaction_date | timestamp |

### Example Input:

| user_id | spend | transaction_date |
|---|---|---|
| 111 | 100.50 | 01/08/2022 12:00:00 |
| 111 | 55.00 | 01/10/2022 12:00:00 |
| 121 | 36.00 | 01/18/2022 12:00:00 |
| 145 | 24.99 | 01/26/2022 12:00:00 |
| 111 | 89.60 | 02/05/2022 12:00:00 |

### Example Output:

| user_id | spend | transaction_date |
|---|---|---|
| 111 | 89.60 | 02/05/2022 12:00:00 |

## 💡 Solution

```sql
WITH users_rank AS (
  SELECT 
    user_id,
    spend,
    transaction_date,
    DENSE_RANK() OVER(
      PARTITION BY user_id 
      ORDER BY transaction_date
    ) rango
  FROM transactions
)

SELECT 
  user_id,
  spend,
  transaction_date
FROM users_rank
WHERE rango = 3;
```

## 📝 Explanation

This solution uses a Common Table Expression (CTE) with a window function to rank transactions for each user:

1. **CTE Creation**: Create a `users_rank` CTE that adds a ranking column to each transaction
2. **Window Function**: Use `DENSE_RANK()` partitioned by `user_id` and ordered by `transaction_date` to assign ranks
3. **Filter**: Select only rows where the rank equals 3 to get the third transaction

## 🔑 Key Concepts

- **Window Functions**: `DENSE_RANK()` assigns ranks without gaps
- **PARTITION BY**: Groups data by user_id before applying the ranking
- **ORDER BY**: Determines the ranking order based on transaction date
- **CTE**: Makes the query more readable and maintainable
