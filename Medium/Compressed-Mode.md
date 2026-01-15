# Compressed Mode

**Company:** Alibaba
**Difficulty:** Medium
**SQL Dialect:** PostgreSQL 14

## Problem Statement

You're given a table containing the item count for each order on Alibaba, along with the frequency of orders that have the same item count. Write a query to retrieve the mode of the order occurrences. Additionally, if there are multiple item counts with the same mode, the results should be sorted in ascending order.

**Clarifications:**
- `item_count`: Represents the number of items sold in each order.
- `order_occurrences`: Represents the frequency of orders with the corresponding number of items sold per order.
- For example, if there are 800 orders with 3 items sold in each order, the record would have an `item_count` of 3 and an `order_occurrences` of 800.

## Tables

### `items_per_order` Schema:

| Column Name | Type |
|-------------|------|
| item_count | integer |
| order_occurrences | integer |

### `items_per_order` Example Input:

| item_count | order_occurrences |
|------------|-------------------|
| 1 | 500 |
| 2 | 1000 |
| 3 | 800 |

## Example Output:

| mode |
|------|
| 2 |

**Explanation:** Based on the example output, the `order_occurrences` value of 1000 corresponds to the highest frequency among all item counts. This means that item count of 2 has occurred 1000 times, making it the mode of order occurrences.

## Solution

### Method #1: Using MAX()

```sql
SELECT item_count AS mode
FROM items_per_order
WHERE order_occurrences = (
  SELECT MAX(order_occurrences)
  FROM items_per_order)
ORDER BY item_count;
```

### Method #2: Using MODE() WITHIN GROUP ()

```sql
SELECT item_count AS mode
FROM items_per_order
WHERE order_occurrences = (
  SELECT MODE() WITHIN GROUP (ORDER BY order_occurrences DESC)
  FROM items_per_order)
ORDER BY item_count;
```
