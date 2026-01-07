# Final Account Balance

**Company**: PayPal

**Difficulty**: Easy

**Source**: [DataLemur](https://datalemur.com/questions/final-account-balance)

---

## Problem Statement

Given a table containing information about bank deposits and withdrawals made using Paypal, write a query to retrieve the final account balance for each account, taking into account all the transactions recorded in the table with the assumption that there are no missing transactions.

## Table Schema

### `transactions` Table:

| Column Name | Type |
|-------------|----------|
| transaction_id | integer |
| account_id | integer |
| amount | decimal |
| transaction_type | varchar |

### `transactions` Example Input:

| transaction_id | account_id | amount | transaction_type |
|----------------|------------|--------|------------------|
| 123 | 101 | 10.00 | Deposit |
| 124 | 101 | 20.00 | Deposit |
| 125 | 101 | 5.00 | Withdrawal |
| 126 | 201 | 20.00 | Deposit |
| 128 | 201 | 10.00 | Withdrawal |

### Example Output:

| account_id | final_balance |
|------------|---------------|
| 101 | 25.00 |
| 201 | 10.00 |

### Explanation:

Using account ID 101 as an example, $30.00 was deposited into this account, while $5.00 was withdrawn. Therefore, the final account balance can be calculated as the difference between the total deposits and withdrawals which is $30.00 - $5.00, resulting in a final balance of $25.00.

---

## Solution

```sql
SELECT 
  account_id,
  SUM(
    CASE 
      WHEN transaction_type = 'Deposit' THEN amount
      WHEN transaction_type = 'Withdrawal' THEN amount * -1
    END
  ) as final_balance
FROM transactions
GROUP BY account_id;
```

### Explanation:

1. Use a **CASE** statement to handle deposits and withdrawals:
   - For deposits, use the amount directly
   - For withdrawals, multiply the amount by -1 to subtract it

2. **SUM()** aggregate function calculates the total for each account

3. **GROUP BY** account_id to get results for each account separately
