# Swapped Food Delivery

**Company:** Zomato  
**Difficulty:** Medium  
**SQL Dialect:** PostgreSQL 14

## Problem Statement

Zomato is a leading online food delivery service that connects users with various restaurants and cuisines.

Recently, Zomato encountered an issue with their delivery system. Due to an error in the delivery driver instructions, each item's order was swapped with the item in the subsequent row.

Write a query to correct this swapping error and return the proper pairing of order ID and item.

**Note:** If the last item has an odd order ID, it should remain as the last item in the corrected data.

## Tables

### `orders` Schema:

| column_name | type | description |
|-------------|--------|-------------|
| order_id | integer | The ID of each Zomato order |
| item | string | The name of the food item in each order |

### `orders` Example Input:

| order_id | item |
|----------|--------------|
| 1 | Chow Mein |
| 2 | Pizza |
| 3 | Pad Thai |
| 4 | Butter Chicken |
| 5 | Eggrolls |
| 6 | Burger |
| 7 | Tandoori Chicken |

## Example Output:

| corrected_order_id | item |
|--------------------|--------------|
| 1 | Pizza |
| 2 | Chow Mein |
| 3 | Butter Chicken |
| 4 | Pad Thai |
| 5 | Burger |
| 6 | Eggrolls |
| 7 | Tandoori Chicken |

## Explanation

Order ID 1 is now associated with Pizza and Order ID 2 is paired with Chow Mein. Order ID 7 remains unchanged with Tandoori Chicken.

## Solution

```sql
SELECT
  CASE
    WHEN order_id = (SELECT COUNT(*) FROM orders) THEN order_id
    WHEN MOD(order_id,2) = 0 THEN order_id - 1
    WHEN MOD(order_id,2) != 0 THEN order_id + 1
  END AS corrected_order_id,
  item
FROM orders
ORDER BY corrected_order_id;
```
